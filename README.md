# SpoknLabs — website

Static marketing site for **SpoknLabs** and its apps. Hosts the home page, app pages, Privacy Policy, Terms of Service, Support, and FAQ. Built as plain HTML/CSS/JS — no build step, no framework, no dependencies.

Design language is borrowed from the Spokn iOS and macOS apps: dark-first, signal-red accent (`#FF5A4A`), system fonts with tight negative kerning, continuous rounded corners, soft radial glows.

## Project structure

```
spokn-web/
├── index.html                  # Home
├── 404.html                    # Not found
├── apps/
│   └── spokn/index.html        # Spokn app page (template pattern for more apps)
├── privacy/index.html          # Privacy Policy (scoped to Spokn iOS)
├── terms/index.html            # Terms of Service (scoped to Spokn iOS)
├── support/index.html          # Support / Contact
├── faq/index.html              # FAQ
├── assets/
│   ├── css/styles.css          # All styles + design tokens
│   ├── js/main.js              # Theme toggle (light / dark / system)
│   └── images/
│       ├── favicon.svg         # Tab icon (also used as apple-touch-icon)
│       ├── spokn-glyph.svg     # Standalone brand glyph
│       └── og-default.svg      # Shared Open Graph image (1200×630)
├── robots.txt
├── sitemap.xml
├── .nojekyll                   # Disable Jekyll on GitHub Pages
└── spokn-app-legal-content.md  # Source content for privacy/terms
└── website-build-prompt.md     # Original build brief
```

## Run locally

No build, no install. Just serve the folder from any static server:

```bash
cd spokn-web
python3 -m http.server 4000
# open http://localhost:4000
```

Or with Node:

```bash
npx serve -p 4000 .
```

Opening `index.html` directly with `file://` mostly works but root-relative links (`/assets/...`, `/privacy/`) won't resolve — always use a local server.

## Deploy to GitHub Pages

The site is configured for `https://spoknlabs.github.io/` — a GitHub organization (or user) page. All internal links use root-relative paths (`/privacy/`, `/assets/...`), which is what GitHub Pages expects at the root of that hostname.

**One-time setup:**

1. Create a repository on GitHub named **exactly** `spoknlabs.github.io` under the `spoknlabs` org.
2. Push this folder's contents to that repository's `main` branch:
   ```bash
   git init
   git remote add origin git@github.com:spoknlabs/spoknlabs.github.io.git
   git add .
   git commit -m "Initial site"
   git branch -M main
   git push -u origin main
   ```
3. In the repo settings → **Pages**, set *Source* to **Deploy from a branch**, branch `main`, folder `/ (root)`. Save.
4. First deploy takes a minute or two. The site comes up at `https://spoknlabs.github.io/`.

**If you'd rather host it at a project URL** (`https://spoknlabs.github.io/spokn-web/`), the root-relative paths will break. Two fixes:
- Add a custom domain (see below), or
- Do a global find-replace of `href="/` and `src="/` with `href="./` and `src="./` (and update the equivalent references in sub-pages to `../...`). Easier to use the user/org repo name.

**Custom domain (optional):**

1. Add a `CNAME` file at the repo root containing your apex or `www` hostname, e.g. `spoknlabs.com`.
2. In your DNS provider, add the four GitHub Pages `A` records (or a `CNAME` to `spoknlabs.github.io` for a subdomain).
3. In repo settings → Pages → enforce HTTPS once the certificate is issued.
4. Update the `<link rel="canonical">`, Open Graph URLs, and `sitemap.xml` / `robots.txt` to the new domain.

## How to edit content

**Text / copy.** All text lives inline in the HTML files. Open the relevant page and edit — there's no CMS, no templating, no partials.

- Home hero, apps grid, values → [index.html](index.html)
- Spokn app page → [apps/spokn/index.html](apps/spokn/index.html)
- Privacy Policy → [privacy/index.html](privacy/index.html)
- Terms of Service → [terms/index.html](terms/index.html)
- Support → [support/index.html](support/index.html)
- FAQ → [faq/index.html](faq/index.html)

Remember to update the **"Last updated"** date at the top of `privacy/` and `terms/` when you change their content.

**Contact email.** Default is `hello@spoknlabs.com`. Search for it across files and replace if yours differs.

**Design tokens** (colors, radii, motion). All CSS custom properties are declared at the top of [assets/css/styles.css](assets/css/styles.css). Change them there once; the rest of the site follows.

**Adding a new app.** Duplicate `apps/spokn/` to `apps/<new-app>/`. Then:
1. Rename/replace hero copy, features, download section.
2. Add a card for it on the home page (`index.html` → the `#apps` section grid).
3. Add a row to `sitemap.xml`.
4. If the app has its own privacy/terms, create `apps/<new-app>/privacy/` and `apps/<new-app>/terms/` — this keeps URLs stable as the portfolio grows.

**Adding a FAQ entry.** In `faq/index.html`, copy a `<details class="faq-item">…</details>` block and edit.

**Open Graph image.** `assets/images/og-default.svg` is used everywhere. Some social platforms (notably older Twitter cards) prefer PNG over SVG. If you notice OG previews missing, export `og-default.svg` to a 1200×630 PNG and update the `og:image` URLs to point at it.

**Favicon.** `assets/images/favicon.svg` is the SVG favicon. For maximum browser compatibility you may also want to add a `favicon.ico` and a 180×180 `apple-touch-icon.png` derived from it — optional.

## Wiring Spokn app → Privacy/Terms links

The Spokn iOS app (v3.0+) adds Settings rows that deep-link to these pages. The stable URLs are:

- Privacy: `https://spoknlabs.github.io/privacy/`
- Terms:   `https://spoknlabs.github.io/terms/`

If you add a custom domain, update the Settings rows in the Spokn repo to match.

## Accessibility & SEO notes

- Semantic HTML (`<header>`, `<nav>`, `<main>`, `<section>`, `<article>`, `<footer>`).
- Focus states are visible — `outline: 2px solid var(--signal)` on all interactive elements.
- Meta tags (title, description, canonical, OG/Twitter) are set per page.
- `robots.txt` allows everything; `sitemap.xml` lists all public URLs.
- Respects `prefers-reduced-motion` (disables animations/smooth scroll when set).
- Respects `prefers-color-scheme` (system-aware dark/light; override via the nav toggle).
- `theme-color` meta tag matches the current scheme.

## Why these choices

**Plain HTML over a framework.** The site is ~8 pages of static content with no interactivity to speak of. A framework adds a build step, dependencies, and version maintenance for no visible win.

**Directory URLs (`/privacy/` not `/privacy.html`).** Cleaner to type, cleaner in share cards, and easier to migrate later (e.g. to `/apps/spokn/privacy/`) with server rewrites.

**`.nojekyll`.** GitHub Pages runs Jekyll by default, which ignores files/directories starting with `_`. We don't use Jekyll here, so we disable it to avoid surprises.
