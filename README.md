# Personal Blog

A modern, responsive personal blog built with Jekyll and designed for GitHub Pages deployment. This blog features a clean, minimalist design with a focus on readability and performance.

## 🌟 Features

- **Modern Design**: Clean, responsive layout that works on all devices
- **Jekyll-Powered**: Static site generation for fast loading and security
- **GitHub Pages Ready**: Optimized for easy deployment on GitHub Pages
- **SEO Optimized**: Built-in SEO features and social media integration
- **Customizable**: Easy to personalize with your own content and branding
- **Performance Focused**: Minimal CSS and optimized assets for fast loading
- **Accessible**: Designed with accessibility best practices in mind

## 🚀 Quick Start

### Option 1: Deploy to GitHub Pages (Recommended)

1. **Fork or download this repository**
2. **Rename the repository** to `your-username.github.io` (replace `your-username` with your GitHub username)
3. **Edit the configuration** in `_config.yml` with your information
4. **Customize the content** (about page, first blog post, etc.)
5. **Enable GitHub Pages** in repository settings
6. **Visit your live blog** at `https://your-username.github.io`

### Option 2: Local Development

1. **Clone the repository**
   ```bash
   git clone https://github.com/your-username/personal-blog.git
   cd personal-blog
   ```

2. **Install dependencies**
   ```bash
   bundle install
   ```

3. **Serve locally**
   ```bash
   bundle exec jekyll serve
   ```

4. **Visit** `http://localhost:4000` to see your blog

## ⚙️ Configuration

### Essential Settings

Edit `_config.yml` to customize your blog:

```yaml
# Basic Information
title: "Your Blog Title"
subtitle: "Your blog subtitle"
description: "A brief description of your blog"
author: "Your Name"
email: "your.email@example.com"
url: "https://yourusername.github.io"
baseurl: "" # Leave empty if using username.github.io

# Social Media
social:
  github: yourusername
  twitter: yourusername
  linkedin: yourusername
```

### Important Files to Customize

- **`_config.yml`** - Main site configuration
- **`about.md`** - Your about page
- **`_posts/`** - Your blog posts (you can delete the sample posts)
- **`assets/css/main.css`** - Styling (optional customization)

## ✍️ Writing Posts

Create new blog posts in the `_posts` directory using this naming convention:
`YYYY-MM-DD-your-post-title.md`

### Post Template

```markdown
---
layout: post
title: "Your Post Title"
date: 2024-12-15 10:00:00 -0500
category: "Technology"
tags: [tag1, tag2, tag3]
excerpt: "A brief summary of your post that appears in previews"
---

Your post content goes here in Markdown format.

## Subheading

You can use all standard Markdown syntax:

- Lists
- **Bold text**
- *Italic text*
- [Links](https://example.com)
- Code blocks
- Images

And much more!
```

## 🎨 Customization

### Colors and Styling

The main colors are defined in CSS custom properties at the top of `assets/css/main.css`:

```css
:root {
    --primary-color: #2563eb;      /* Main accent color */
    --secondary-color: #64748b;    /* Secondary text color */
    --background-color: #ffffff;   /* Background */
    --text-color: #1e293b;        /* Main text color */
}
```

### Adding Your Own Pages

Create new `.md` files in the root directory. For example, create `portfolio.md`:

```markdown
---
layout: default
title: "Portfolio"
permalink: /portfolio/
---

# My Portfolio

Your portfolio content here...
```

Then add a link to the navigation in `_layouts/default.html`.

## 📁 Project Structure

```
personal-blog/
├── _config.yml          # Site configuration
├── _layouts/            # HTML templates
│   ├── default.html     # Base layout
│   └── post.html        # Blog post layout
├── _posts/              # Blog posts
│   ├── 2024-12-15-welcome-to-my-blog.md
│   ├── 2024-12-15-building-this-blog.md
│   └── 2024-12-10-five-lessons-learned.md
├── assets/
│   └── css/
│       └── main.css     # Custom styles
├── index.md             # Homepage
├── about.md             # About page
├── archive.md           # Blog archive
├── Gemfile              # Ruby dependencies
└── README.md            # This file
```

## 🚀 Deployment Options

### GitHub Pages (Free & Easy)

1. Push your code to a GitHub repository
2. Go to Settings > Pages in your repository
3. Select "Deploy from a branch" and choose "main"
4. Your blog will be available at `https://username.github.io/repository-name`

### Custom Domain

To use a custom domain with GitHub Pages:

1. Add a `CNAME` file to your repository root with your domain name
2. Configure your domain's DNS settings to point to GitHub Pages
3. Enable "Enforce HTTPS" in repository settings

### Other Hosting Options

This Jekyll blog can also be deployed to:
- **Netlify**: Drag and drop the `_site` folder after running `bundle exec jekyll build`
- **Vercel**: Connect your GitHub repository for automatic deployments
- **Your own server**: Upload the `_site` folder contents to your web server

## 🛠️ Development

### Prerequisites

- Ruby (2.7 or higher)
- Bundler gem (`gem install bundler`)

### Local Development Workflow

1. **Make changes** to your content or code
2. **Test locally** with `bundle exec jekyll serve`
3. **Preview** at `http://localhost:4000`
4. **Commit and push** to deploy (if using GitHub Pages)

### Building for Production

```bash
bundle exec jekyll build
```

This generates the static site in the `_site` directory.

## 🎯 SEO & Performance

### Built-in SEO Features

- Semantic HTML structure
- Meta tags and Open Graph tags
- XML sitemap generation (`/sitemap.xml`)
- RSS feed (`/feed.xml`)
- Structured data for blog posts

### Performance Optimizations

- Minimal, custom CSS (no frameworks)
- Optimized web fonts with fallbacks
- Responsive images
- Clean, semantic HTML
- Fast-loading static site

## 📝 Content Ideas

Need inspiration for blog posts? Consider writing about:

- **Technical tutorials** - Share your programming knowledge
- **Project walkthroughs** - Document your latest projects
- **Learning journeys** - What new skills are you developing?
- **Industry insights** - Your thoughts on trends and news
- **Personal experiences** - Travel, hobbies, life lessons
- **Tool reviews** - Software, apps, or services you recommend
- **Book reviews** - Share insights from your reading
- **Career advice** - Lessons learned from your professional journey

## 🤝 Contributing

If you find bugs or have suggestions for improvements:

1. **Fork the repository**
2. **Create a feature branch** (`git checkout -b feature/amazing-feature`)
3. **Commit your changes** (`git commit -m 'Add amazing feature'`)
4. **Push to the branch** (`git push origin feature/amazing-feature`)
5. **Open a Pull Request**

## 📄 License

This project is open source and available under the [MIT License](LICENSE).

## 🆘 Support

Having trouble getting started? Here are some helpful resources:

- **Jekyll Documentation**: [https://jekyllrb.com/docs/](https://jekyllrb.com/docs/)
- **GitHub Pages Guide**: [https://pages.github.com/](https://pages.github.com/)
- **Markdown Guide**: [https://www.markdownguide.org/](https://www.markdownguide.org/)

### Common Issues

**Q: My site isn't updating after pushing changes**
A: GitHub Pages can take a few minutes to build and deploy. Check the Actions tab in your repository for build status.

**Q: My custom domain isn't working**
A: Make sure your DNS settings are correct and that you've added a CNAME file to your repository.

**Q: Images aren't displaying**
A: Make sure image paths are correct and start with `/assets/` for images in the assets folder.

---

## 🎉 Getting Started Checklist

- [ ] Fork or download this repository
- [ ] Update `_config.yml` with your information
- [ ] Customize the About page
- [ ] Write your first blog post
- [ ] Delete the sample posts (or keep them as examples)
- [ ] Deploy to GitHub Pages
- [ ] Share your new blog with the world!

**Happy blogging!** 🚀

---

*This blog template was created with ❤️ for the developer community. If you use it for your blog, I'd love to see what you create!*