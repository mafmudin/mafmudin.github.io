# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this repo is

Personal GitHub Pages site (`mafmudin/mafmudin.github.io`) — a static portfolio for Mafmudin (Android developer). Content is written in **Bahasa Indonesia**; keep it that way when editing copy unless the user asks otherwise.

Two pages carry the actual content:

- `index.html` — landing page listing personal libraries (ProDialog, MPopUp).
- `aboutme.html` — portfolio: Android apps on Play Store ("Aplikasi Saya") and client projects ("Project Saya"). To add an entry, copy an existing `.col-lg-4 ... <div class="card">…</div>` block in the relevant section.

`mycv.svg` is the linked CV.

## Build, run, deploy

There is **no build step and no package.json** — `css/` and `js/` ship as pre-compiled vendor files. Do not try to `npm install`, run a bundler, or expect a test/lint command; none exist. Edit HTML and CSS directly.

- Preview locally: `python3 -m http.server 8000` from the repo root, open <http://localhost:8000>. Any static server works; opening the HTML as `file://` breaks the WOW.js animation init and some relative paths.
- Deploy: push to `master`. GitHub Pages serves the repo root.

## Where to put changes

- **Custom CSS overrides** → `css/style.css` (currently empty, already linked from both HTML files after `mdb.min.css`). Do not edit `css/mdb*.css` or `css/bootstrap*.css` — those are vendored.
- **Page content / structure** → `index.html` or `aboutme.html` directly.
- **Images** → `img/`. `my-2020_Images/` is a separate older asset folder; prefer `img/` for new assets.

### About `scss/` and `src/`

`scss/` contains the upstream MDB Sass source (entry: `scss/mdb-free.scss`) with two empty hooks (`_custom-styles.scss`, `_custom-variables.scss`). **There is no Sass compiler wired up in this repo** — recompiling the MDB CSS would require setting up MDB's external build pipeline, which the user has not done here. For everyday changes, edit `css/style.css` instead of touching `scss/`. Do the same for the `src/` tree (vendor JS source).

## Versions to preserve

- MDB FREE **4.19.0** (jQuery flavor), Bootstrap 4, jQuery 3.4.1. Don't propose upgrading to MDB 5+ or Bootstrap 5 — the markup uses Bootstrap 4 conventions (`data-toggle`, `ml-2`, `waves-effect`) and an upgrade would touch every page.

## Navbar duplication gotcha

Both `index.html` and `aboutme.html` contain the navbar markup **twice** — once inside the `<button class="navbar-toggler">` (mobile collapse) and once inside `<div class="collapse navbar-collapse">` (desktop). When adding/renaming a nav link, update **both copies in both files** or the mobile and desktop menus will drift.
