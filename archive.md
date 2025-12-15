---
layout: default
title: "Archive"
permalink: /archive/
---

<div style="max-width: 1000px; margin: 0 auto;">
    <header style="text-align: center; margin-bottom: 3rem;">
        <h1>Blog Archive</h1>
        <p style="font-size: 1.2rem; color: #64748b;">All posts, organized by date</p>
    </header>

    {% if site.posts.size > 0 %}
        {% assign posts_by_year = site.posts | group_by_exp: "post", "post.date | date: '%Y'" %}
        
        {% for year in posts_by_year %}
            <section style="margin-bottom: 3rem;">
                <h2 style="color: #2563eb; border-bottom: 2px solid #e2e8f0; padding-bottom: 0.5rem; margin-bottom: 2rem;">
                    {{ year.name }}
                    <span style="font-size: 0.8em; color: #64748b; font-weight: normal;">
                        ({{ year.items.size }} post{% if year.items.size != 1 %}s{% endif %})
                    </span>
                </h2>
                
                <div style="display: grid; gap: 1.5rem;">
                    {% for post in year.items %}
                        <article style="background: white; border: 1px solid #e2e8f0; border-radius: 8px; padding: 1.5rem; transition: all 0.3s ease;">
                            <div style="display: flex; flex-wrap: wrap; gap: 1rem; align-items: flex-start; justify-content: space-between;">
                                <div style="flex: 1; min-width: 250px;">
                                    <h3 style="margin: 0 0 0.5rem 0;">
                                        <a href="{{ post.url | relative_url }}" style="color: #1e293b; text-decoration: none;">
                                            {{ post.title }}
                                        </a>
                                    </h3>
                                    
                                    <div style="display: flex; flex-wrap: wrap; gap: 1rem; margin-bottom: 1rem; font-size: 0.875rem; color: #64748b;">
                                        <time datetime="{{ post.date | date_to_xmlschema }}">
                                            <i class="fas fa-calendar-alt"></i>
                                            {{ post.date | date: "%B %d, %Y" }}
                                        </time>
                                        
                                        {% if post.category %}
                                            <span>
                                                <i class="fas fa-folder"></i>
                                                {{ post.category }}
                                            </span>
                                        {% endif %}
                                        
                                        {% if post.tags.size > 0 %}
                                            <span>
                                                <i class="fas fa-tags"></i>
                                                {% for tag in post.tags limit:2 %}{{ tag }}{% unless forloop.last %}, {% endunless %}{% endfor %}{% if post.tags.size > 2 %}...{% endif %}
                                            </span>
                                        {% endif %}
                                    </div>
                                    
                                    {% if post.excerpt %}
                                        <p style="color: #64748b; margin: 0; line-height: 1.6;">
                                            {{ post.excerpt | strip_html | truncate: 200 }}
                                        </p>
                                    {% endif %}
                                </div>
                                
                                <div style="flex-shrink: 0;">
                                    <a href="{{ post.url | relative_url }}" 
                                       style="display: inline-flex; align-items: center; gap: 0.5rem; padding: 0.5rem 1rem; background: #2563eb; color: white; border-radius: 6px; text-decoration: none; font-size: 0.875rem; font-weight: 500; transition: all 0.3s ease;">
                                        Read More
                                        <i class="fas fa-arrow-right"></i>
                                    </a>
                                </div>
                            </div>
                        </article>
                    {% endfor %}
                </div>
            </section>
        {% endfor %}
        
        <div style="background: linear-gradient(135deg, #f1f5f9 0%, #e2e8f0 100%); padding: 2rem; border-radius: 8px; text-align: center; margin-top: 3rem;">
            <h3>Thanks for Reading!</h3>
            <p style="color: #64748b; margin-bottom: 1.5rem;">
                That's all {{ site.posts.size }} post{% if site.posts.size != 1 %}s{% endif %} so far. 
                {% if site.posts.size > 0 %}New content is published regularly, so check back soon!{% endif %}
            </p>
            
            <div style="display: flex; justify-content: center; gap: 1rem; flex-wrap: wrap;">
                <a href="{{ '/' | relative_url }}" 
                   style="display: inline-flex; align-items: center; gap: 0.5rem; padding: 0.75rem 1.5rem; background: #2563eb; color: white; border-radius: 8px; text-decoration: none; font-weight: 500; transition: all 0.3s ease;">
                    <i class="fas fa-home"></i>
                    Back to Home
                </a>
                
                {% if site.social.github or site.social.twitter or site.email %}
                    <a href="{{ '/about' | relative_url }}" 
                       style="display: inline-flex; align-items: center; gap: 0.5rem; padding: 0.75rem 1.5rem; background: white; color: #2563eb; border: 1px solid #2563eb; border-radius: 8px; text-decoration: none; font-weight: 500; transition: all 0.3s ease;">
                        <i class="fas fa-user"></i>
                        Get in Touch
                    </a>
                {% endif %}
            </div>
        </div>
    {% else %}
        <div style="background: #f8fafc; padding: 3rem; border-radius: 8px; text-align: center; border: 1px solid #e2e8f0;">
            <i class="fas fa-edit" style="font-size: 3rem; color: #cbd5e0; margin-bottom: 1rem;"></i>
            <h3>No posts yet!</h3>
            <p style="color: #64748b; margin-bottom: 2rem;">
                This blog is just getting started. The first posts will appear here soon!
            </p>
            <a href="{{ '/' | relative_url }}" 
               style="display: inline-flex; align-items: center; gap: 0.5rem; padding: 0.75rem 1.5rem; background: #2563eb; color: white; border-radius: 8px; text-decoration: none; font-weight: 500;">
                <i class="fas fa-home"></i>
                Back to Home
            </a>
        </div>
    {% endif %}
</div>

<style>
article:hover {
    transform: translateY(-2px);
    box-shadow: 0 10px 25px rgba(0,0,0,0.1) !important;
}

article h3 a:hover {
    color: #2563eb !important;
}

article a[href*="Read More"]:hover {
    background: #1d4ed8 !important;
    transform: translateY(-1px);
}

.archive-nav a:hover {
    background: #1d4ed8 !important;
    transform: translateY(-2px);
    text-decoration: none !important;
}

@media (max-width: 768px) {
    article div[style*="flex"] {
        flex-direction: column !important;
    }
    
    article div[style*="flex-shrink: 0"] {
        align-self: flex-start !important;
        margin-top: 1rem !important;
    }
}
</style>