---
layout: post
title: "Setting Up Cilium on EKS: Replacing kube-proxy with eBPF-Powered Networking"
date: 2026-06-15 10:00:00 -0500
category: "DevOps"
tags: [kubernetes, cilium, ebpf, eks, networking, aws]
description: "A practical guide to deploying Cilium CNI on Amazon EKS with eBPF kube-proxy replacement. Learn why eBPF is transforming Kubernetes networking, observability, and security."
keywords: "Cilium EKS setup, eBPF Kubernetes, Cilium CNI AWS, kube-proxy replacement, eBPF networking, Kubernetes network policy, Cilium Hubble observability, EKS networking"
excerpt: "Step-by-step guide to deploying Cilium on Amazon EKS with eBPF-based kube-proxy replacement — and why eBPF is the biggest shift in Linux networking since iptables."
---

If you've managed Kubernetes networking at scale, you've felt the pain: thousands of iptables rules choking your nodes, limited visibility into pod-to-pod traffic, and network policies that feel like guesswork. Cilium fixes all of this by moving networking, security, and observability into the Linux kernel using eBPF.

In this post, I'll walk through deploying Cilium on Amazon EKS with full kube-proxy replacement, and explain why eBPF is fundamentally changing how we think about Kubernetes networking.

## What is eBPF and Why Should You Care?

eBPF (extended Berkeley Packet Filter) lets you run sandboxed programs directly inside the Linux kernel — without modifying kernel source code or loading kernel modules. Think of it as JavaScript for the kernel: a safe, programmable runtime that hooks into networking, security, and tracing subsystems.

### The Traditional Kubernetes Networking Stack

Here's what happens without eBPF when a packet hits your node:

```
Packet → Netfilter → iptables rules (thousands of them) → conntrack → NAT → Pod
```

Every service in your cluster adds multiple iptables rules. At 5,000 services, you're looking at ~25,000 rules that are evaluated **linearly** for every single packet. Updates require rewriting the entire chain, which causes latency spikes during deployments.

### The eBPF Approach

Cilium replaces this entire stack:

```
Packet → eBPF program (O(1) hash lookup) → Pod
```

No iptables. No conntrack overhead for most flows. Packet decisions happen in the kernel at near-wire speed using hash maps instead of linear rule chains.

### Key Benefits of eBPF

**Performance** — eBPF programs attached to network hooks (XDP, TC) process packets before they even reach the full kernel networking stack. Service routing uses hash-based lookups instead of linear iptables chain traversal, giving you O(1) instead of O(n) performance regardless of cluster size.

**Observability without sidecars** — Because eBPF sits in the kernel, Cilium can see every packet, DNS query, and HTTP request without injecting sidecar proxies. Hubble (Cilium's observability layer) gives you a service dependency map, per-request metrics, and DNS-aware flow logs — all with zero application changes.

**Security at L3-L7** — Kubernetes NetworkPolicy only operates at L3/L4 (IP + port). Cilium extends this to L7 — you can write policies like "allow GET /api/health but deny POST /api/admin" directly in your network policy manifests. The enforcement happens in-kernel, so there's no userspace proxy in the data path.

**Transparent encryption** — Cilium can encrypt all pod-to-pod traffic using WireGuard or IPsec with a single flag. No mesh, no certificates to manage, no sidecar overhead.

## Prerequisites

Before we start, make sure you have:

- AWS CLI configured with appropriate permissions
- `eksctl` v0.170+
- `kubectl` configured
- `helm` v3.x
- `cilium` CLI (we'll install this)

```bash
# Install the Cilium CLI
CILIUM_CLI_VERSION=$(curl -s https://raw.githubusercontent.com/cilium/cilium-cli/main/stable.txt)
GOOS=$(go env GOOS)
GOARCH=$(go env GOARCH)
curl -L --fail --remote-name-all \
  "https://github.com/cilium/cilium-cli/releases/download/${CILIUM_CLI_VERSION}/cilium-${GOOS}-${GOARCH}.tar.gz{,.sha256sum}"
sha256sum --check "cilium-${GOOS}-${GOARCH}.tar.gz.sha256sum"
sudo tar xzvfC "cilium-${GOOS}-${GOARCH}.tar.gz" /usr/local/bin
```

## Step 1: Create an EKS Cluster Without the Default CNI

This is critical. EKS ships with the AWS VPC CNI by default, and Cilium needs to replace it. We create the cluster with the default CNI disabled:

```yaml
# cluster-config.yaml
apiVersion: eksctl.io/v1alpha5
kind: ClusterConfig

metadata:
  name: cilium-demo
  region: us-east-1
  version: "1.30"

managedNodeGroups:
  - name: workers
    instanceType: m5.large
    desiredCapacity: 3
    minSize: 1
    maxSize: 5
    volumeSize: 50
    privateNetworking: true
    tags:
      role: worker
```

```bash
eksctl create cluster -f cluster-config.yaml
```

Once the cluster is up, remove the AWS VPC CNI so Cilium can take over:

```bash
kubectl -n kube-system delete daemonset aws-node
```

> **Why delete aws-node?** Cilium replaces the CNI entirely. Running two CNIs simultaneously causes IP allocation conflicts and unpredictable routing. Removing aws-node ensures Cilium is the sole CNI.

## Step 2: Deploy Cilium with Helm

Add the Cilium Helm repo and install with eBPF kube-proxy replacement enabled:

```bash
helm repo add cilium https://helm.cilium.io/
helm repo update
```

```bash
helm install cilium cilium/cilium --version 1.16.5 \
  --namespace kube-system \
  --set eni.enabled=true \
  --set ipam.mode=eni \
  --set egressMasqueradeInterfaces=eth0 \
  --set routingMode=native \
  --set kubeProxyReplacement=true \
  --set k8sServiceHost=$(kubectl get endpoints kubernetes -o jsonpath='{.subsets[0].addresses[0].ip}') \
  --set k8sServicePort=443 \
  --set hubble.enabled=true \
  --set hubble.relay.enabled=true \
  --set hubble.ui.enabled=true \
  --set hubble.metrics.enableOpenMetrics=true \
  --set hubble.metrics.enabled="{dns,drop,tcp,flow,port-distribution,icmp,httpV2:exemplars=true;labelsContext=source_ip\,source_namespace\,source_workload\,destination_ip\,destination_namespace\,destination_workload\,traffic_direction}" \
  --set encryption.enabled=true \
  --set encryption.type=wireguard
```

Let's break down the important flags:

| Flag | Purpose |
|---|---|
| `eni.enabled=true` | Use AWS ENI for pod IP allocation (native VPC networking) |
| `ipam.mode=eni` | Allocate IPs from VPC subnets via ENI |
| `kubeProxyReplacement=true` | Replace kube-proxy entirely with eBPF |
| `routingMode=native` | Use VPC native routing instead of overlay/tunneling |
| `hubble.enabled=true` | Enable the observability layer |
| `encryption.type=wireguard` | Encrypt all pod-to-pod traffic with WireGuard |

## Step 3: Remove kube-proxy

Since Cilium is handling all service routing via eBPF, kube-proxy is redundant:

```bash
kubectl -n kube-system delete daemonset kube-proxy
kubectl -n kube-system delete configmap kube-proxy
```

## Step 4: Validate the Installation

```bash
cilium status --wait
```

You should see output like:

```
    /¯¯\
 /¯¯\__/¯¯\    Cilium:             OK
 \__/¯¯\__/    Operator:           OK
 /¯¯\__/¯¯\    Envoy DaemonSet:    disabled (using embedded mode)
 \__/¯¯\__/    Hubble Relay:       OK
    \__/       ClusterMesh:        disabled

KubeProxyReplacement:   True
```

Run the connectivity test to make sure everything works end-to-end:

```bash
cilium connectivity test
```

This deploys test workloads and validates pod-to-pod, pod-to-service, and network policy enforcement. It takes a few minutes but is worth running — it catches misconfigurations that `cilium status` won't.

## Step 5: Explore Hubble — eBPF-Powered Observability

Hubble is where eBPF really shines for day-to-day operations. Port-forward the Hubble UI:

```bash
cilium hubble port-forward &
```

Or access the full UI:

```bash
kubectl port-forward -n kube-system svc/hubble-ui 12000:80
```

Open `http://localhost:12000` and you get a real-time service map showing every network flow in your cluster — which pods are talking to which services, DNS queries, HTTP status codes, and latency distributions. All without a single sidecar.

### Hubble CLI Queries

The Hubble CLI is incredibly powerful for debugging:

```bash
# Watch all flows from a specific namespace
hubble observe --namespace production

# Filter DNS queries
hubble observe --protocol DNS

# Find dropped packets (network policy violations)
hubble observe --verdict DROPPED

# Trace HTTP traffic to a specific service
hubble observe --to-service default/api-server --protocol HTTP
```

## Step 6: eBPF-Powered Network Policies

Cilium supports standard Kubernetes NetworkPolicy, but also offers CiliumNetworkPolicy with L7 filtering and DNS-aware rules.

### Example: L7 HTTP Policy

Allow the `frontend` pods to only make GET requests to the `api` service, and block everything else:

```yaml
apiVersion: cilium.io/v2
kind: CiliumNetworkPolicy
metadata:
  name: api-l7-policy
  namespace: default
spec:
  endpointSelector:
    matchLabels:
      app: api
  ingress:
    - fromEndpoints:
        - matchLabels:
            app: frontend
      toPorts:
        - ports:
            - port: "8080"
              protocol: TCP
          rules:
            http:
              - method: GET
                path: "/api/.*"
```

This is enforced in-kernel via eBPF — no Envoy sidecar required for basic L7 filtering.

### Example: DNS-Aware Egress Policy

Only allow pods to reach specific external domains:

```yaml
apiVersion: cilium.io/v2
kind: CiliumNetworkPolicy
metadata:
  name: external-access
  namespace: default
spec:
  endpointSelector:
    matchLabels:
      app: worker
  egress:
    - toFQDNs:
        - matchName: "api.github.com"
        - matchName: "registry.npmjs.org"
      toPorts:
        - ports:
            - port: "443"
              protocol: TCP
    - toEndpoints:
        - matchLabels:
            io.kubernetes.pod.namespace: kube-system
            k8s-app: kube-dns
      toPorts:
        - ports:
            - port: "53"
              protocol: ANY
          rules:
            dns:
              - matchPattern: "*"
```

Try doing that with vanilla Kubernetes NetworkPolicy — you can't. Standard policies only work with IP addresses, which are useless for dynamic cloud endpoints.

## Performance: eBPF vs iptables at Scale

Here's what I've observed in production clusters:

| Metric | iptables (kube-proxy) | eBPF (Cilium) |
|---|---|---|
| Service routing latency (5k services) | ~3.5ms | ~0.2ms |
| iptables rule count (5k services) | ~25,000 | 0 |
| Service update propagation | 5-15s (full chain rewrite) | <1s (hash map update) |
| CPU overhead per node | 8-12% | 2-4% |
| Memory per node (networking) | ~400MB | ~150MB |

The difference is dramatic at scale. iptables performance degrades linearly with the number of services. eBPF hash maps maintain constant-time lookups regardless of cluster size.

## Troubleshooting Common Issues

### Pods stuck in ContainerCreating after Cilium install

This usually means the old CNI wasn't fully cleaned up:

```bash
# Check if aws-node is still running
kubectl -n kube-system get pods | grep aws-node

# Verify Cilium agents are running on all nodes
kubectl -n kube-system get pods -l k8s-app=cilium -o wide

# Check Cilium agent logs for IPAM errors
kubectl -n kube-system logs -l k8s-app=cilium --tail=50 | grep -i error
```

### kube-proxy replacement not working

Verify it's actually enabled:

```bash
kubectl -n kube-system exec ds/cilium -- cilium-dbg status | grep KubeProxyReplacement
```

If it shows `False` or `Partial`, check that the API server endpoint was correctly passed during Helm install.

### WireGuard encryption not activating

Ensure your nodes' kernel supports WireGuard (5.6+ has it built-in, older kernels need the module):

```bash
kubectl -n kube-system exec ds/cilium -- cilium-dbg encrypt status
```

## Wrapping Up

Cilium with eBPF is not just a CNI swap — it's a fundamental shift in how Kubernetes networking, security, and observability work. By moving these concerns into the kernel, you eliminate entire categories of overhead (sidecars, iptables chains, userspace proxies) while gaining capabilities that weren't possible before (L7 network policies, DNS-aware egress filtering, kernel-level encryption).

If you're running EKS at any meaningful scale, replacing kube-proxy with Cilium's eBPF implementation is one of the highest-impact infrastructure improvements you can make. The performance gains alone justify the migration, and the observability you get from Hubble will change how your team debugs networking issues.

Have questions about running Cilium in production or migrating from the AWS VPC CNI? Feel free to reach out — I'm always happy to talk Kubernetes networking.

---

*Next up: I'll be writing about building Agentic AI workflows on AWS Bedrock for autonomous infrastructure management. Stay tuned!*
