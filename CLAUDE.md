# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Overview

Personal portfolio site for Zain Asif (AI Engineer & Senior Full Stack Developer), served as a GitHub Pages site at https://zain-asif-dev.github.io. It is a **single-file static site with zero build tooling** — no package.json, no bundler, no tests, no linter. All markup, styling, and behavior live in `index.html` (~1,600 lines).

## Development Commands

There is no build, lint, or test step. To preview changes, serve the directory (or just open `index.html` in a browser):

```bash
python3 -m http.server 8000   # then visit http://localhost:8000
# or
npx serve .
```

**Deployment:** pushing to `master` auto-deploys via GitHub Pages from the repo root. No CI or additional configuration.

## Architecture of index.html

Everything is inline in one file; there are no separate `.css` or `.js` files. Layout of the file:

- **`<head>`** — meta/OG tags, then CDN dependencies: Tailwind CSS (via `cdn.tailwindcss.com` runtime script, *not* a compiled build), Font Awesome 6, and Google Fonts (Inter + JetBrains Mono).
- **Inline `tailwind.config`** (small `<script>` block) — extends `fontFamily` so `font-sans`/`font-mono` map to Inter/JetBrains Mono.
- **Inline `<style>` block** (~300 lines) — everything Tailwind utilities can't express: CSS variables (`--bg`, `--bg-soft`), custom scrollbar, `.glass` (glass-morphism surfaces), gradient text/accents, the companies logo marquee animation, `.fade-in`/`.visible` scroll-reveal classes, floating badge animations, and mobile menu `.open` state.
- **Body sections in order** — sticky nav, Hero, Companies marquee, Impact stats, About, Skills (`#skills`), Experience (`#experience`), Projects (`#projects`), Tools (`#tools`), Education (`#education`), Certifications (`#certifications`), Contact (`#contact`), footer. Nav links target these section `id`s, so keep ids stable when editing.
- **Inline `<script>` at the end** — footer year, mobile menu toggle, an `IntersectionObserver` that adds `.visible` to `.fade-in` elements on scroll (new sections should carry the `fade-in` class to match), and the scroll-to-top button.

## Assets

`assets/` holds all static files, referenced with relative `./assets/...` paths:

- `picture.png` (profile photo, also the favicon/OG image), `resume.pdf`
- `companies/` — employer logos used in the marquee and experience cards
- `projects/` — project screenshots
- `certificates/` — certification images
- `tools/` — ~44 SVG/PNG tech-stack icons (mostly devicon-style `*-original.svg` naming)

## Conventions

- Styling is Tailwind utility classes directly in markup, with the dark glass-morphism theme (indigo/violet gradient accents on near-black `--bg: #05060a`). Match existing patterns (`.glass` cards, gradient text spans) rather than inventing new component styles.
- The site content (roles, projects, stats) is real personal/professional data — edit it only when asked, and keep `README.md` in sync when structure or sections change (it documents the section list and project tree).
- Keep the site dependency-free beyond the existing CDN links; there is deliberately no build step.
