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

## Architecture

### Single-page structure
All content lives in `index.html`. Sections are anchored with IDs and navigated via smooth scroll:
- `#header` — hero with Typed.js cycling effect
- `#about` — bio text + interests icon grid
- `#education` — institution cards + certifications portfolio grid (Isotope)
- `#experience` — job timeline entries
- `#portfolio` — project cards filtered by Isotope (`.filter-app` / `.filter-project`)
- `#skills` — logo images + styled tag spans
- `#links` — CV download + GitHub
- `#contacts` — location, social, email

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

### Skills section proficiency layout
The `#skills` section uses a flex column layout to group logos by proficiency tier. Each tier is a row with a colored left-border label (`Avançado` = `#12d640`, `Intermediário` = `#ffc107`, `Básico` = `#6c757d`) followed by the relevant logo images. Do not revert to a single flat `<p>` of logos — the tiered grouping is intentional.

### CSS/JS vendors (all local, no CDN for core libs)
`assets/vendor/` holds: Bootstrap 4, jQuery, Isotope, Owl Carousel, Venobox, Typed.js, Boxicons, Remixicon, Icofont, Waypoints, CounterUp, jQuery Easing. `assets/css/style.css` is the single custom stylesheet (dark theme, `#12d640` green accent, `#010e1b` background).

`assets/js/main.js` — custom JS: nav toggle, Isotope filtering, skill bar animation (Waypoints), Typed.js init.

### Certification/logo images
`assets/img/certification/` — local logos for cert cards. `assets/img/education/` — institution logos. `assets/img/project/` — project thumbnail images used in the grid cards.

## Colors (design tokens in style.css)
| Token | Value | Usage |
|---|---|---|
| Primary accent | `#12d640` | Links, highlights, badges |
| Dark accent | `#1c7d32` | Hover states |
| Gold | `#9e7f25` | Section titles underline |
| Background | `#010e1b` | Page background |
| Card background | `#09203a` | `.icon-box` cards |
