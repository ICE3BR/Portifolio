# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Static portfolio website hosted on GitHub Pages at `me.gaqtech.dev`. No build step, no package manager, no test suite — changes to `index.html` and files under `assets/` and `projects/` are deployed directly via `git push` to the `master` branch.

## Deployment

```bash
git add .
git commit -m "message"
git push origin master
```

GitHub Pages serves from the root of `master`. The custom domain is configured in `CNAME` (`me.gaqtech.dev`).

## Hard Constraints

- **Bootstrap 4** — do NOT migrate to Bootstrap 5
- **No build step** — static HTML/CSS/JS only; no npm, no bundler
- **Profile photo** (`profile_2026.jpeg`) must NOT have `loading="lazy"` — it is above the fold
- **GA/GTM** — only a commented-out placeholder in `index.html`; do NOT insert real IDs
- **OpenAI logo** stays local at `assets/img/openai-vector.svg` — NOT on CDN
- **AOS** is vendored locally at `assets/vendor/aos/` — do NOT switch back to unpkg CDN

## Architecture

### Single-page structure

All content lives in `index.html`. Sections are anchored with IDs and navigated via smooth scroll:

- `#header` — hero with Typed.js cycling effect + 2-column layout (code block on right at ≥1200px)
- `#about` — bio text + interests icon grid
- `#education` — institution cards + certifications portfolio grid (Isotope)
- `#experience` — job timeline entries
- `#portfolio` — project cards filtered by Isotope (`.filter-app` / `.filter-project`)
- `#skills` — tiered logo rows (devicons CDN) + styled tag spans for metodologias/idiomas
- `#links` — CV download + GitHub
- `#contacts` — location, social, email

### Hero — 2-column layout (desktop ≥1200px)

The hero wraps content in two elements inside `.container`:
- `.hero-content` — h1 + nav (takes the left side, `flex: 1 1 auto`)
- `.hero-decorative` — decorative code block (positioned `absolute`, `right: 0`, `pointer-events: none`)

**Sticky navbar (`header-top` class):** uses `display: contents` on `.hero-content` to make h1 and nav direct children of `.container` again, restoring original single-row layout. The decorative block is hidden (`display: none !important`).

Do not use `flex-wrap` on `#header.header-top` — it breaks the sticky nav layout.

### Project detail pages (`projects/*.html`)

Each project opens as an iframe lightbox via **Venobox** (`data-vbtype="iframe"`). All detail pages share the same structure:
- `tech-badge` spans for the stack
- A commented-out screenshots block (uncomment + add images to `assets/img/project/details/<project-name>/`)
- A commented-out YouTube/Loom video embed block
- A "Sobre o Projeto" narrative section

To add a new project:
1. Add a card in `index.html` `#portfolio` with class `filter-app` or `filter-project`
2. Create `projects/<slug>.html` following the pattern of `projects/chatbotai.html`
3. Create `assets/img/project/details/<slug>/` and drop screenshots there, then uncomment the screenshot block

### Private projects

Use a badge inline in the card title and a locked-info box at the bottom of the detail page. See `projects/legalhub.html` for the pattern.

### Skills section — proficiency tiers + devicons

The `#skills` section groups logos by proficiency tier. Each `.skill-tier` row has:
- A colored label (`.skill-tier-label`): `adv` = `#12d640`, `int` = `#ffc107`, `bas` = `#6c757d`
- A `.skill-tier-logos` div with `<img>` tags from devicons CDN (`cdn.jsdelivr.net/gh/devicons/devicon@latest`)

**Logo visibility rules on dark background (`#010e1b`):**
- Logos with dark colors (e.g. Django `plain-wordmark`, Linux Tux) need `style="filter: brightness(0) invert(1);"` to appear white
- Next.js uses `style="filter: invert(1);"` (inverts black to white)
- Wordmark variants (e.g. `python-original-wordmark`, `nodejs-plain-wordmark`) are preferred over icon-only for readability
- Do not restore the old single flat `<p>` of logos — the tiered grouping is intentional

### CSS/JS vendors (all local, no CDN for core libs)

`assets/vendor/` holds: Bootstrap 4, jQuery, Isotope, Owl Carousel, Venobox, Typed.js, Boxicons, Remixicon, Icofont, Waypoints, CounterUp, jQuery Easing, **AOS 2.3.4**.

`assets/css/style.css` — single custom stylesheet. Key additions from visual renovation:
- Dark theme: `#12d640` green accent, `#010e1b` background
- Navbar blur (`backdrop-filter: blur(10px)`) when `.header-top`
- Section titles: `border-left: 3px solid #12d640; padding-left: 10px` on `.section-title h2`
- Card hover: `translateY(-6px)` + green box-shadow on `.portfolio-item .portfolio-wrap:hover`
- Scroll-to-top button: fixed bottom-right, appears after 300px, `#12d640` background
- Certification visual differentiation: `.cert-azure` (blue border), `.cert-db` (orange), `.cert-ml` (green)
- Skill tiers: `.skill-tier.adv/int/bas` with colored backgrounds and borders

`assets/js/main.js` — custom JS: nav toggle, Isotope filtering, skill bar animation (Waypoints), Typed.js init.

### Certification/logo images

`assets/img/certification/` — local logos for cert cards. `assets/img/education/` — institution logos. `assets/img/project/` — project thumbnail images used in the grid cards.

## Colors (design tokens in style.css)

| Token | Value | Usage |
|---|---|---|
| Primary accent | `#12d640` | Links, highlights, badges, adv tier |
| Dark accent | `#1c7d32` | Hover states |
| Gold | `#9e7f25` | Section titles underline |
| Background | `#010e1b` | Page background |
| Card background | `#09203a` | `.icon-box` cards |
| Int tier | `#ffc107` | Intermediário label/border |
| Bas tier | `#6c757d` | Básico label/border |
