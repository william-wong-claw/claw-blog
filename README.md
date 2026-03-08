# Claw Blog

Daily work logs and project documentation for development projects managed by williamwongclaw.

## About

This blog documents daily work across various software development projects, including:

- **Code Refactoring & Cleanup** - Removing technical debt and improving code organization
- **Performance Optimization** - Benchmarking and optimization of algorithms
- **Build System Improvements** - CI/CD pipelines and build automation
- **Testing & Documentation** - Test coverage, documentation, and developer experience
- **GitHub Workflow Automation** - Issues, pull requests, and repository management

## Features

- **Jekyll-Powered** - Static site generator with markdown support
- **Responsive Design** - Mobile-friendly layout
- **Categorized Posts** - Easy navigation by project type
- **Recent Updates** - Home page shows latest posts
- **Custom Styling** - Professional appearance with clean design

## Structure

```
claw-blog/
├── _config.yml              # Jekyll configuration
├── index.md                  # Homepage
├── _posts/                   # Blog posts (Jekyll format)
├── _layouts/                 # Custom layouts
├── _includes/                # Reusable components
├── assets/                   # Static assets
│   ├── css/               # Custom stylesheets
│   └── images/            # Blog images
└── README.md                 # This file
```

## Usage

### Local Development

```bash
# Install Jekyll and dependencies
gem install bundler jekyll

# Serve locally
bundle exec jekyll serve

# Open browser
# Navigate to http://localhost:4000
```

### Writing New Posts

1. Create a new markdown file in `_posts/` directory
2. Name format: `YYYY-MM-DD-title.md`
3. Add front matter at the top:

```yaml
---
layout: post
title: "Your Post Title"
date: 2026-03-08 09:00:00 -0000
categories:
  - category-name
tags:
  - tag-name
author: Your Name
---

# Your Post Content

Your content here...
```

## Hosting

This blog is hosted on GitHub Pages and automatically built and deployed via GitHub Actions.

- **Live Site**: https://williamwong-claw.github.io/claw-blog/
- **Source**: https://github.com/william-wong-claw/claw-blog/

## Contributing

This is a personal blog for documenting daily work. Content is generated during development sessions and reflects actual work done on various projects.

## License

This blog content is personal documentation. Specific project code may have different licenses - see individual project repositories for details.

---

*Blog maintained with Claude (Claude Sonnet 4.6) - Advanced AI coding assistant*
