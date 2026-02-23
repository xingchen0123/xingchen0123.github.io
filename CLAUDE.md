# CLAUDE.md

This file provides guidance for AI assistants working with this repository.

## Project Overview

This is the personal academic homepage of **Xing Chen (陈醒)**, Assistant Professor at the School of International Relations and Public Affairs, Fudan University. The site is built with **Jekyll** and hosted on **GitHub Pages**.

The repository also doubles as a reusable Jekyll theme called **Minimal Light**, distributed as a Ruby Gem under the CC0 1.0 license.

**Live site**: Deployed automatically from the `master` branch via GitHub Pages.

---

## Repository Structure

```
.
├── _layouts/
│   └── homepage.html        # Single HTML template for all pages
├── _sass/
│   └── minimal-light.scss   # Core theme stylesheet (SCSS)
├── assets/
│   ├── css/
│   │   └── style.scss       # Custom CSS overrides (imports minimal-light)
│   ├── img/                 # Profile photo and favicons
│   │   ├── photo4.jpg       # Profile photo (used as avatar)
│   │   ├── 2.2_meitu_cut.jpg# Favicon image (shared for light/dark)
│   │   ├── favicon.png      # Light mode favicon (alternative)
│   │   └── favicon-dark.png # Dark mode favicon (alternative)
│   └── js/
│       ├── favicon-switcher.js  # Dynamically switches favicon on OS theme change
│       └── scale.fix.js         # Responsive viewport scaling fix
├── script/
│   ├── bootstrap            # Installs gem dependencies
│   ├── cibuild              # Full CI build + lint + validate
│   ├── release              # Tags and publishes gem to RubyGems
│   └── validate-html        # Custom HTML validation script
├── .github/
│   ├── workflows/ci.yaml    # GitHub Actions CI pipeline
│   ├── settings.yml         # Branch protection and label config (probot)
│   ├── config.yml           # BehaviorBot automated PR/issue responses
│   ├── no-response.yml      # Auto-close stale issues
│   └── stale.yml            # Stale PR handling
├── _config.yml              # Jekyll site configuration (site metadata)
├── index.md                 # PRIMARY CONTENT FILE — all CV/bio content lives here
├── Gemfile                  # Ruby dependencies (delegates to gemspec)
├── minimal-light.gemspec    # Gem specification and dependency versions
├── .rubocop.yml             # Ruby linter configuration
├── .travis.yml              # Legacy Travis CI config (superseded by GitHub Actions)
├── CNAME                    # Custom domain (currently empty)
├── README.md                # Theme usage documentation (English)
├── README_de.md             # German translation
├── README_zh_Hans.md        # Simplified Chinese translation
├── README_zh_Hant.md        # Traditional Chinese translation
└── LICENSE                  # CC0 1.0 Universal
```

---

## Technology Stack

| Layer | Technology |
|---|---|
| Static site generator | Jekyll (>3.5, <5.0) |
| Hosting | GitHub Pages (auto-deploy from `master`) |
| Content format | Markdown (`index.md`) |
| Templating | Liquid (Jekyll's default) |
| Styling | SCSS compiled by Jekyll |
| Icons | Font Awesome 5.11.2 + Academicons 1.8.6 (CDN) |
| Fonts | Crimson Pro (serif), Ubuntu Mono (CDN via Google Fonts) |
| Ruby version | 2.4+ required; CI uses 2.7 |

---

## Key Configuration File: `_config.yml`

All site-level metadata is controlled here:

```yaml
title: Xing CHEN
position: Assistant Professor
affiliation: Fudan University
email: xingc[at]fudan.edu.cn
keywords: Xing Chen
google_scholar: https://scholar.google.com/...
cv_link: assets/CV.pdf
avatar: ./assets/img/photo4.jpg
favicon: ./assets/img/2.2_meitu_cut.jpg
favicon_dark: ./assets/img/2.2_meitu_cut.jpg
```

**Optional fields** that can be added to `_config.yml` (rendered automatically in `homepage.html`):
- `linkedin` — LinkedIn profile URL
- `github_link` — GitHub profile URL
- `google_analytics` — GA tracking ID
- `canonical` — canonical URL for SEO
- `description` — meta description
- `lang` — page language (defaults to `en-US`)

---

## Primary Content File: `index.md`

**This is the only file that needs editing for CV/bio updates.** It uses the `homepage` layout and pure Markdown.

Current sections (in order):
1. About Me
2. Research Interests
3. Employment
4. Education
5. Publications
6. Working Papers
7. Awards
8. Book and Book Chapters
9. Fundings and Projects
10. Conference and Invited Talks
11. Manuscript Referee
12. Professional Activities
13. Teaching (with `<details>/<summary>` collapsible course descriptions)
14. PhD Program Placements of Research Assistants
15. Additional Information

**Conventions used in `index.md`**:
- `**Bold**` for author names and journal titles
- `***Bold Italic***` for journal names inline in publication entries
- `*\* denotes corresponding author.*` footnote pattern
- HTML `<details><summary>...</summary>...</details>` for collapsible course descriptions
- External links use `[[link]](url)`, `[[slides]](url)`, `[[中文简介]](url)` patterns

---

## Layout & Styling

### `_layouts/homepage.html`

The single page layout. Key features:
- Renders a fixed left sidebar (header) with avatar, name, position, affiliation, email, and social icon links
- Renders main content (the Markdown from `index.md`) in a right-floating section
- Includes `favicon-switcher.js` for dynamic OS-aware favicon switching
- Loads Font Awesome and Academicons from CDN with SRI integrity hashes
- Supports optional Google Analytics via Liquid conditional
- `onContextMenu="return false"` on avatar image (prevents right-click save)

### `_sass/minimal-light.scss`

Core stylesheet. Key design decisions:
- **Light mode**: `background-color: #fff`, `color: #595959`, headings `color: #043361`
- **Dark mode** (`@media (prefers-color-scheme: dark)`): `background-color: #20212b`, `color: #dadbdf`, headings `color: rgb(62, 183, 240)`
- Layout: fixed 220px left sidebar, 650px right content section, 960px total wrapper
- Custom HTML elements used as styling hooks: `<autocolor>`, `<lightonly>`, `<darkonly>`, `<papertitle>`, `<education>`
- Social icons: 3rem circles, `#495057` background, black on hover

### `assets/css/style.scss`

Minimal custom overrides on top of the theme. Currently only adjusts list item margins. Import order:
```scss
@import "minimal-light";
// custom rules below
```

---

## Development Workflow

### Prerequisites

- Ruby 2.4+
- Bundler

### Local Setup

```bash
script/bootstrap      # gem install bundler && bundle install
bundle exec jekyll serve --livereload   # serves at http://localhost:4000
```

### Full CI Build (runs lint + HTML validation)

```bash
script/cibuild
# Equivalent to:
bundle exec jekyll build
bundle exec htmlproofer ./_site --check-html --check-sri
bundle exec rubocop -D
bundle exec script/validate-html
```

### Running Individual Checks

```bash
bundle exec jekyll build          # build to _site/
bundle exec htmlproofer ./_site   # HTML link + SRI check
bundle exec rubocop               # Ruby lint
```

---

## CI/CD Pipeline

**GitHub Actions** (`.github/workflows/ci.yaml`):
- Triggers on every `push`
- Runs on `ubuntu-latest` with Ruby 2.7
- Steps: `script/bootstrap` → `script/cibuild`

**GitHub Pages**:
- Auto-deploys from the `master` branch
- No manual deployment step needed — merging to `master` publishes the site

**Legacy**: `.travis.yml` exists but is superseded by GitHub Actions.

---

## Assets: Images

| File | Usage |
|---|---|
| `assets/img/photo4.jpg` | Profile avatar (2412×2412px, SONY ILCE-7M4) |
| `assets/img/2.2_meitu_cut.jpg` | Favicon (both light + dark mode currently) |
| `assets/img/favicon.png` | Alternative light favicon |
| `assets/img/favicon-dark.png` | Alternative dark favicon |

To update the profile photo, replace `assets/img/photo4.jpg` and update `avatar` in `_config.yml` if the filename changes.

---

## Gem Release (Theme Maintainer Only)

```bash
script/release    # must be on master, tags and pushes to RubyGems
```

Current gem version: `1.0.0` (in `minimal-light.gemspec`).

---

## What AI Assistants Should Know

### Most Common Tasks

1. **Updating CV content** → edit only `index.md`
2. **Changing site metadata** (name, email, links) → edit `_config.yml`
3. **Styling changes** → edit `_sass/minimal-light.scss` or `assets/css/style.scss`
4. **Template changes** → edit `_layouts/homepage.html`
5. **Adding a new profile photo** → drop image in `assets/img/`, update `_config.yml`

### Do Not

- Do not edit files inside `_site/` — this is a generated build artifact and is git-ignored
- Do not commit `Gemfile.lock` — it is git-ignored
- Do not push to `master` directly without going through the PR workflow if branch protection is active
- Do not modify CDN integrity hashes in `homepage.html` without updating them to match the actual file hashes

### Branch Protection

Per `.github/settings.yml`, the `master` branch requires:
- Passing CI status checks
- Code owner review for PRs

### Ignored Files

```
_site/
.sass-cache/
Gemfile.lock
*.gem
.DS_Store
.jekyll-cache/
git-update.sh
```
