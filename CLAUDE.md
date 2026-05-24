# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Personal portfolio website for Mohammed Shahin Kamarudeen (Software Engineer at Microsoft). Static site built with **Jekyll 4.x**. The landing page (`index.html`) is hand-written HTML/CSS/JS, but the `/blog/` section uses Jekyll layouts, includes, and Markdown posts.

## Development

Jekyll build is required to preview the site correctly — Python's `http.server` will show raw YAML frontmatter on blog pages.

```bash
# Install gems (one-time)
bundle install

# Run dev server with livereload
bundle exec jekyll serve --port 8000 --livereload
```

Visit `http://localhost:8000/`.

### Local Ruby gotcha (macOS)

System Ruby (`/usr/bin/ruby`, 2.6.x) is too old for Jekyll 4. Use Homebrew Ruby:

```bash
# Put Homebrew Ruby and the user gem bin dir on PATH first, then run bundle/jekyll
PATH="/opt/homebrew/opt/ruby/bin:$HOME/.gem/ruby/4.0.0/bin:$PATH" bundle exec jekyll serve --port 8000 --livereload
```

The user-gem bin path is needed because `bundle exec` installs binstubs to `~/.gem/<ruby-version>/bin/`, which isn't on PATH by default.

## Architecture

**Hand-written landing page:**
- `index.html` - Hero, about, life, journey, contact sections
- `styles.css` - All styling, CSS custom properties for theming
- `script.js` - Custom cursor, particles, scroll animations, pun generator, Konami easter egg

**Jekyll blog:**
- `_config.yml` - Jekyll config
- `_layouts/` - Page templates (default, post, archive, etc.)
- `_includes/` - Partials
- `_posts/` - Markdown posts (filenames: `YYYY-MM-DD-slug.md`)
- `blog/index.html` - Blog index page (uses `layout: archive` frontmatter)
- `_site/` - Build output (gitignored; do not edit by hand)

### Theme System

CSS variables define multiple theme palettes in `styles.css`: Classic, Modern, Creative, Zen, Terminal. The active theme is set via `data-theme="..."` on `<body>` in `index.html`. Each theme is a `[data-theme="<name>"]` block of `--*` variable overrides; some themes also have a small selector-specific override block beneath the palette (e.g. disabling particles).

When changing theme palettes, watch out for `--gradient-primary` — it's used by `.section-title` and `.logo-text` via `background-clip: text`. If it's a multi-color gradient, headings render with that gradient, which reads as the typical AI-generated dev-site look. Flat solid colors are preferred for restrained themes.

### JavaScript Features

- **Custom cursor**: Disabled automatically on touch devices; some themes also disable it via `display: none`
- **Particle system**: Animated background particles using requestAnimationFrame
- **Scroll animations**: IntersectionObserver for fade-in effects
- **Pun generator**: Cycling programming puns in the footer
- **Konami code easter egg**: ↑↑↓↓←→←→BA triggers rainbow animation

### Responsive Breakpoints

- Desktop: default
- Tablet: 1024px
- Mobile: 768px
- Small mobile: 480px

## Deployment

Site is built by Jekyll and deployable to GitHub Pages, Netlify, or any static host. Run `bundle exec jekyll build` to produce `_site/` for upload, or let the host run the build.
