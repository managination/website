# Setup Guide for Managination Website

## Prerequisites

- Ruby 3.1.3 or higher
- Bundler 2.4.7 or higher
- Git

## Apple Silicon (M1/M2/M3) Setup

If you're on Apple Silicon, you need to install `eventmachine` with a special flag due to OpenSSL 3 compatibility:

```bash
gem install eventmachine -v 1.2.7 -- --with-ldflags="-Wl,-undefined,dynamic_lookup"
```

## Local Development

### Build the Site

```bash
script/cibuild
```

This will:
1. Build the Jekyll site to `_site/`
2. Build the gem package

### Run Local Server

```bash
script/server
```

Then visit http://localhost:4000

The server will automatically reload when you make changes.

### Manual Build

If you prefer to build manually:

```bash
jekyll build           # Build site
jekyll serve          # Run server
```

## Project Structure

```
.
├── _config.yml              # Jekyll configuration
├── _layouts/                # Custom page layouts
│   └── default.html         # Main layout with header/footer
├── _sass/                   # Theme SASS files
├── assets/
│   ├── css/
│   │   └── style.scss       # Custom styles (imports theme + additions)
│   ├── favicon.png          # Site favicon
│   └── logo-*.png           # Logo images
├── index.md                 # Homepage content
├── contact.md               # Contact page
├── privacy-policy.md        # Privacy policy
└── docs/                    # Documentation
    └── web-site-content.md  # Source content document
```

## Deployment

The site automatically deploys to GitHub Pages when you push to `main` branch via GitHub Actions (`.github/workflows/jekyll.yml`).

No manual deployment needed!

## Troubleshooting

### eventmachine Won't Compile

On Apple Silicon with OpenSSL 3, use this command:
```bash
gem install eventmachine -v 1.2.7 -- --with-ldflags="-Wl,-undefined,dynamic_lookup"
```

### Bundler Version Conflicts

If you see bundler version errors, install the correct version:
```bash
gem install bundler:2.4.7
bundle _2.4.7_ install
```

### SASS Deprecation Warnings

The SASS deprecation warnings are expected and don't affect functionality. They come from the base Cayman theme and will be addressed in a future theme update.

## Making Content Changes

### Edit Homepage

Edit `index.md` - it uses markdown with embedded HTML for rich layouts.

### Update Styles

Edit `assets/css/style.scss` - all custom styles are at the bottom after the theme import.

### Add New Pages

1. Create a new `.md` file in the root
2. Add front matter:
   ```yaml
   ---
   layout: default
   title: Page Title
   permalink: /url-path/
   ---
   ```
3. Add your content
4. Link to it from `_layouts/default.html` or other pages

## Design System

The site uses a custom design system called "Institutional Trust meets Digital Future":

- **Display Font:** DM Serif Display (headers)
- **Body Font:** Work Sans (paragraphs)
- **Mono Font:** IBM Plex Mono (technical tags)

### Color Palette

```scss
--trust-navy: #0a1628
--trust-blue: #1e3a8a
--trust-cyan: #06b6d4
--trust-teal: #0d9488
--accent-gold: #f59e0b
--accent-violet: #7c3aed
--accent-emerald: #059669
```

All colors are defined as CSS custom properties in `assets/css/style.scss`.

## Support

For issues or questions about the codebase, see `CLAUDE.md` for AI assistant guidance.
