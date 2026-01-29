# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a Jekyll-based GitHub Pages website for managination.com, focusing on decentralization and innovation. The site uses the Cayman theme and is automatically deployed to GitHub Pages via GitHub Actions.

## Key Commands

### First Time Setup (Apple Silicon Only)

If on Apple Silicon (M1/M2/M3), install eventmachine with this flag to avoid OpenSSL 3 compilation issues:
```bash
gem install eventmachine -v 1.2.7 -- --with-ldflags="-Wl,-undefined,dynamic_lookup"
```

### Local Development
```bash
# Run local development server
script/server
# Site will be available at http://localhost:4000 with live reload

# Build the site
jekyll build

# Run CI build (build + gem package)
script/cibuild
```

### Testing
The simplified `script/cibuild` now runs:
- Jekyll build
- Gem package build

Full HTML validation and linting run automatically in GitHub Actions CI, not locally.

## Architecture

### Jekyll Structure
- **Content files**: Markdown files in root (`index.md`, `contact.md`, `privacy-policy.md`, etc.) are compiled into HTML pages
- **Layouts**: `_layouts/default.html` defines the page template with header, footer navigation, and content area
- **Includes**: `_includes/` contains reusable HTML snippets (head tags, Google Analytics)
- **Styles**: `_sass/` contains theme SCSS files; `assets/css/style.scss` imports the theme
- **Static assets**: `assets/` contains logos, favicon, and custom CSS
- **Output**: `_site/` contains generated static HTML (excluded from git, generated on build)

### Theme Customization
The site uses `jekyll-theme-cayman` with custom overrides:
- Custom layout in `_layouts/default.html` adds footer navigation (Contact, Privacy Policy)
- Favicon configured via `_includes/head-custom.html`
- Custom styles can be added to `assets/css/style.scss` after the theme import

### Configuration
- `_config.yml`: Site title, description, theme selection, and exclusions
- Ruby version: 3.1.3 (specified in GitHub Actions workflow)
- GitHub Actions workflow: `.github/workflows/jekyll.yml` handles automated deployment on push to main

### Page Front Matter
Each content page uses YAML front matter:
```yaml
---
layout: default
title: Page Title
permalink: /custom-url/
---
```

## Deployment

The site automatically deploys to GitHub Pages when changes are pushed to the `main` branch. The GitHub Actions workflow:
1. Checks out code
2. Sets up Ruby 3.1.3
3. Installs dependencies with bundler
4. Builds Jekyll site with production environment
5. Deploys to GitHub Pages

No manual deployment steps are required.

## Content Guidelines

- Create new pages as `.md` files in the root directory
- Use `layout: default` in front matter for consistent styling
- Add links to new pages in navigation by editing `_layouts/default.html`
- Logo files are in `assets/`: `logo-square.png`, `logo-transparent.png`, `favicon.png`