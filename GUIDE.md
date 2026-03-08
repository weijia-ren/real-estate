# Summit Realty Group — Site Maintenance Guide

## Quick Reference

| Task | Command |
|------|---------|
| Local dev server | `hugo server -D` |
| Build for production | `hugo --minify` |
| Create a new property | Copy an existing `.md` in `content/properties/` |
| Deploy | Push to `main` — GitHub Actions handles the rest |

---

## Project Structure

```
real-estate-site/
├── hugo.toml                 # Site config (title, params, menus)
├── static/css/style.css      # All styles
├── static/images/            # Logo and any images you add
├── layouts/                  # HTML templates
│   ├── index.html            # Homepage
│   ├── properties/           # Property list + detail pages
│   ├── _default/             # Generic single/list pages
│   ├── partials/             # Header, footer
│   └── shortcodes/           # Reusable components
├── content/                  # Markdown content (pages, properties, services)
└── .github/workflows/        # GitHub Actions deployment
```

## Common Tasks

### Add a New Property

1. Create a new `.md` file in `content/properties/`:

```md
---
title: "Your Property Title"
address: "123 Any Street, Austin, TX 78701"
price: "350,000"
beds: 3
baths: 2
sqft: "1,800"
type: "For Sale"          # Options: "For Sale", "For Rent", "Investment"
status: "Active"
features:
  - "Feature one"
  - "Feature two"
weight: 4                 # Controls sort order (lower = first)
---

Property description goes here in regular markdown.
```

2. The property automatically appears on the Properties page and (if in the first 3 by weight) the homepage.

### Edit Site Info

All business details are in `hugo.toml` under `[params]`:
- Phone, email, address, office hours
- Stats (properties managed, years experience, clients served)
- Site title and tagline

### Edit Navigation

Menu items are in `hugo.toml` under `[menu]`. Add/remove/reorder by adjusting `weight`.

### Edit a Service Page

Service pages are in `content/services/`. Edit the markdown directly. The `weight` field controls the order on the Services list page.

### Add Images

1. Place images in `static/images/`
2. Reference them in templates as `{{ "images/your-image.jpg" | relURL }}`
3. To replace the SVG placeholder in property cards, edit `layouts/properties/list.html` and `layouts/index.html`

### Customize Styles

All CSS is in `static/css/style.css`. Key CSS variables are at the top:
- `--navy`, `--gold` — primary colors
- `--radius` — border radius
- `--shadow-*` — shadow levels

### Use the Callout Shortcode

In any markdown file:

```
{{< callout type="info" >}}
This is an informational callout.
{{< /callout >}}
```

Types: `info` (blue), `success` (green), `warning` (yellow).

## Deployment

The site deploys automatically via GitHub Actions when you push to `main`.

### First-Time Setup

1. Create a GitHub repo named `real-estate`
2. In repo Settings → Pages, set Source to **GitHub Actions**
3. Push your code:
   ```bash
   git remote add origin git@github.com:weijia-ren/real-estate.git
   git push -u origin main
   ```

### Manual Build

```bash
hugo --minify
# Output is in the public/ directory
```

## Local Development

```bash
hugo server -D
# Open http://localhost:1313/real-estate/
```

The `-D` flag includes draft content. The server live-reloads on file changes.
