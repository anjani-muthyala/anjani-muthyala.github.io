---
layout: default
title: "Welcome to Your Personal Blog"
---

<div class="hero">
    <h1>{{ site.title }}</h1>
    <p>Senior DevOps Engineer at Verizon | AI Enthusiast | Cloud Infrastructure Specialist</p>
    <p style="margin-top: 1rem;">{{ site.description }}</p>
</div>

<section class="about-preview" style="background: white; padding: 2rem; border-radius: 8px; border: 1px solid #e2e8f0; margin-bottom: 3rem;">
    <h2>About Me</h2>
    <p>I'm a Senior DevOps Engineer at Verizon with over 8 years of experience in cloud infrastructure, automation, and emerging AI technologies. Currently, I'm passionate about advancing AI-driven automation in DevOps, particularly in autonomous planning, validation, and remediation systems.</p>
    
    <div style="display: grid; grid-template-columns: repeat(auto-fit, minmax(200px, 1fr)); gap: 1rem; margin: 1.5rem 0;">
        <div style="background: #f8fafc; padding: 1rem; border-radius: 6px; text-align: center;">
            <i class="fas fa-robot" style="color: #2563eb; font-size: 1.5rem; margin-bottom: 0.5rem;"></i>
            <h4 style="margin: 0 0 0.5rem 0;">Agentic AI</h4>
            <p style="margin: 0; font-size: 0.875rem; color: #64748b;">AWS Bedrock workflows for autonomous infrastructure</p>
        </div>
        <div style="background: #f8fafc; padding: 1rem; border-radius: 6px; text-align: center;">
            <i class="fas fa-cloud" style="color: #2563eb; font-size: 1.5rem; margin-bottom: 0.5rem;"></i>
            <h4 style="margin: 0 0 0.5rem 0;">Cloud Infrastructure</h4>
            <p style="margin: 0; font-size: 0.875rem; color: #64748b;">AWS, GCP, Kubernetes, Docker orchestration</p>
        </div>
        <div style="background: #f8fafc; padding: 1rem; border-radius: 6px; text-align: center;">
            <i class="fas fa-cogs" style="color: #2563eb; font-size: 1.5rem; margin-bottom: 0.5rem;"></i>
            <h4 style="margin: 0 0 0.5rem 0;">DevOps Automation</h4>
            <p style="margin: 0; font-size: 0.875rem; color: #64748b;">CI/CD, Ansible, Terraform, Jenkins</p>
        </div>
    </div>
    
    <div style="text-align: center; margin-top: 1.5rem;">
        <h4>Certifications</h4>
        <div style="display: flex; justify-content: center; gap: 0.5rem; margin-top: 0.5rem; flex-wrap: wrap;">
            <span style="background: #e2e8f0; padding: 0.25rem 0.75rem; border-radius: 15px; font-size: 0.75rem;">Google Cloud Gen AI Leader</span>
            <span style="background: #e2e8f0; padding: 0.25rem 0.75rem; border-radius: 15px; font-size: 0.75rem;">Red Hat Ansible Specialist</span>
            <span style="background: #e2e8f0; padding: 0.25rem 0.75rem; border-radius: 15px; font-size: 0.75rem;">AWS Solutions Architect</span>
        </div>
    </div>
    
    <div style="text-align: center; margin-top: 1.5rem;">
        <a href="{{ '/about' | relative_url }}" class="btn-secondary">
            Learn More About Me <i class="fas fa-arrow-right"></i>
        </a>
    </div>
</section>

<section class="recent-posts">
    <h2>Recent Posts</h2>
    
    {% if site.posts.size > 0 %}
        <div class="posts-grid">
            {% for post in site.posts limit:6 %}
                <article class="post-card">
                    <h3><a href="{{ post.url | relative_url }}">{{ post.title }}</a></h3>
                    
                    <div class="post-meta">
                        <time datetime="{{ post.date | date_to_xmlschema }}">
                            <i class="fas fa-calendar-alt"></i>
                            {{ post.date | date: "%B %d, %Y" }}
                        </time>
                        
                        {% if post.author %}
                            <span class="post-author">
                                <i class="fas fa-user"></i>
                                {{ post.author }}
                            </span>
                        {% endif %}
                        
                        {% if post.category %}
                            <span class="post-category">
                                <i class="fas fa-folder"></i>
                                {{ post.category }}
                            </span>
                        {% endif %}
                    </div>
                    
                    {% if post.excerpt %}
                        <div class="post-excerpt">
                            {{ post.excerpt | strip_html | truncate: 150 }}
                        </div>
                    {% endif %}
                    
                    {% if post.tags.size > 0 %}
                        <div class="post-tags">
                            {% for tag in post.tags limit:3 %}
                                <span class="tag">{{ tag }}</span>
                            {% endfor %}
                        </div>
                    {% endif %}
                    
                    <a href="{{ post.url | relative_url }}" class="read-more">
                        Read more <i class="fas fa-arrow-right"></i>
                    </a>
                </article>
            {% endfor %}
        </div>
        
        {% if site.posts.size > 6 %}
            <div style="text-align: center; margin-top: 2rem;">
                <a href="{{ '/archive' | relative_url }}" class="btn-primary">
                    View All Posts <i class="fas fa-arrow-right"></i>
                </a>
            </div>
        {% endif %}
    {% else %}
        <div style="text-align: center; padding: 3rem; background: #f8fafc; border-radius: 8px; margin: 2rem 0;">
            <h3>No posts yet!</h3>
            <p>This blog is just getting started. Check back soon for new content!</p>
        </div>
    {% endif %}
</section>

<section class="about-preview" style="background: linear-gradient(135deg, #f1f5f9 0%, #e2e8f0 100%); padding: 3rem; border-radius: 8px; margin: 3rem 0; text-align: center;">
    <h2>What You'll Find Here</h2>
    <div style="display: grid; grid-template-columns: repeat(auto-fit, minmax(250px, 1fr)); gap: 2rem; margin-top: 2rem;">
        <div>
            <i class="fas fa-brain" style="color: #2563eb; font-size: 2rem; margin-bottom: 1rem;"></i>
            <h3>AI & Automation</h3>
            <p style="color: #64748b;">Exploring AI applications in infrastructure, GenAI workflows, and autonomous system management</p>
        </div>
        <div>
            <i class="fas fa-code" style="color: #2563eb; font-size: 2rem; margin-bottom: 1rem;"></i>
            <h3>DevOps Insights</h3>
            <p style="color: #64748b;">Real-world DevOps solutions, best practices, and lessons learned from production environments</p>
        </div>
        <div>
            <i class="fas fa-tools" style="color: #2563eb; font-size: 2rem; margin-bottom: 1rem;"></i>
            <h3>Tools & Technologies</h3>
            <p style="color: #64748b;">Hands-on guides, tool reviews, and technical tutorials for modern infrastructure</p>
        </div>
    </div>
</section>

<style>
.read-more {
    display: inline-flex;
    align-items: center;
    gap: 0.5rem;
    margin-top: 1rem;
    font-weight: 500;
    color: var(--primary-color);
    text-decoration: none;
    transition: all 0.3s ease;
}

.read-more:hover {
    color: #1d4ed8;
    text-decoration: none;
}

.read-more i {
    transition: transform 0.3s ease;
}

.read-more:hover i {
    transform: translateX(4px);
}

.btn-primary, .btn-secondary {
    display: inline-block;
    padding: 0.75rem 1.5rem;
    background: var(--primary-color);
    color: white;
    border-radius: 8px;
    text-decoration: none;
    font-weight: 500;
    transition: all 0.3s ease;
}

.btn-primary:hover, .btn-secondary:hover {
    background: #1d4ed8;
    color: white;
    text-decoration: none;
    transform: translateY(-2px);
}
</style>