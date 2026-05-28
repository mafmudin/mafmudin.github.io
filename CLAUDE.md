# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this repo is

Personal GitHub Pages site (`mafmudin/mafmudin.github.io`) — a static portfolio for Mafmudin (Android developer). Content is written in **Bahasa Indonesia**; keep it that way when editing copy unless the user asks otherwise.

Two pages:

- `index.html` — landing page listing open-source libraries (ProDialog, MPopUp).
- `aboutme.html` — portfolio: Tentang Saya, Aplikasi Saya (Play Store apps), Project Saya (client work).

`mycv.svg` is the linked CV.

## Build, run, deploy

There is **no build step**. The site is plain HTML + one CSS file (`css/style.css`) + Google Fonts (Inter, JetBrains Mono) + Font Awesome via CDN. **Zero JavaScript.**

- Preview locally: `python3 -m http.server 8000` from the repo root, then open <http://localhost:8000>. Opening as `file://` works but blocks some browsers from caching fonts; serve locally for an accurate preview.
- Deploy: push to `master`. GitHub Pages serves the repo root.

## Architecture

- `css/style.css` is the single stylesheet. Layout follows seven labeled sections at the top of the file: Tokens, Reset & base, Typography, Layout utilities, Components, Page-specific, Reduced motion. Add new styles to the matching section.
- Design tokens live in `:root` (colors, spacing, radius, type, motion, layout). **Don't hardcode colors or spacing values** — reference the variables.
- The mobile menu is CSS-only (a sibling `<input type="checkbox">` toggles `~ .navbar-links` visibility). Don't add JavaScript for this — keep it dependency-free.
- Card images use `<img>` against `play-lh.googleusercontent.com` URLs (Play Store CDN). To add a new app card, copy an existing `<article class="card">` block and swap the image URL, title, description, and Play Store link.

## Where to put changes

- **Visual / design tokens** → top of `css/style.css` (`:root`).
- **New components** → `css/style.css` section 5 (Components).
- **Page content / structure** → `index.html` or `aboutme.html` directly.
- **Images** → `img/`.

## Conventions

- Bahasa Indonesia for visible copy.
- External links always include `target="_blank" rel="noopener"`.
- Use `aria-current="page"` on the active nav link.
- Section labels read `— LABEL` (em-dash + space + uppercase mono text); tags read `▸ LABEL` (triangle + space + uppercase mono text). Both styles are font-controlled via `.section-label` and `.tag`.
