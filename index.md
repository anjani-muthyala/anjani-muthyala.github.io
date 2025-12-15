---
layout: default
title: "Home"
---

# Welcome to My Blog

I'm **Anjani Kumar Muthyala**, a Senior DevOps Engineer at Verizon with over 8 years of experience in cloud infrastructure, automation, and emerging AI technologies.

## About Me

Currently, I'm passionate about advancing AI-driven automation in DevOps, particularly in autonomous planning, validation, and remediation systems. I specialize in:

### 🤖 Agentic AI & Automation
- AWS Bedrock workflows for autonomous infrastructure management
- GenAI SRE assistants with RAG over process documentation  
- LangFlow pipelines for infrastructure automation

### ☁️ Cloud Infrastructure  
- AWS and GCP cloud architecture and optimization
- Kubernetes orchestration and container management
- Infrastructure as Code with Terraform and Ansible

### 🔧 DevOps Excellence
- CI/CD pipeline design and optimization
- Monitoring and observability with ElasticSearch, Splunk, SumoLogic
- Configuration management and automation

## Certifications

- 🎓 **Google Cloud Gen AI Leader**
- 🎓 **Red Hat Certified Specialist in Ansible Automation** 
- 🎓 **AWS Certified Solutions Architect - Associate**

## Recent Posts

{% if site.posts.size > 0 %}
  {% for post in site.posts limit:3 %}
  - **[{{ post.title }}]({{ post.url | relative_url }})** - {{ post.date | date: "%B %d, %Y" }}
    
    {{ post.excerpt | strip_html | truncate: 150 }}
  {% endfor %}
  
  [View all posts →]({{ '/archive' | relative_url }})
{% else %}
  More posts coming soon! This blog will focus on DevOps, AI automation, and cloud infrastructure insights.
{% endif %}

## Connect With Me

- 📧 [Email](mailto:{{ site.email }})
- 💼 [LinkedIn](https://linkedin.com/in/{{ site.social.linkedin }})
- 💻 [GitHub](https://github.com/{{ site.social.github }})
- 🐦 [Twitter](https://twitter.com/{{ site.social.twitter }})
- 📄 [Download Resume]({{ '/assets/AnjaniKumar_Muthyala_Resume.pdf' | relative_url }})

---

*Welcome to my professional blog where I share insights from the intersection of DevOps and AI!*