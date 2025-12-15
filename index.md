---
layout: default
title: "Welcome to Your Personal Blog"
---

<div class="hero">
    <h1>{{ site.title }}</h1>
    <p>{{ site.description }}</p>
</div>

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
    <h2>About This Blog</h2>
    <p style="font-size: 1.1rem; color: #64748b; max-width: 600px; margin: 1rem auto;">
        Welcome to my personal blog where I share thoughts, experiences, and insights on various topics. 
        This is a space for exploration, learning, and connecting with like-minded individuals.
    </p>
    <a href="{{ '/about' | relative_url }}" class="btn-secondary" style="display: inline-block; margin-top: 1rem; padding: 0.75rem 1.5rem; background: #2563eb; color: white; border-radius: 8px; text-decoration: none; transition: all 0.3s ease;">
        Learn More About Me
    </a>
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