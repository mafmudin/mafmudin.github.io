# Portfolio Redesign Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Replace the 2019-era MDB Bootstrap look with a custom dark-mode + sage-green design, dropping ~600 KB of vendor CSS/JS in favor of one ~7 KB stylesheet and zero JavaScript.

**Architecture:** Single `css/style.css` holds design tokens, layout utilities, and components. Both HTML pages are rewritten to a semantic, framework-free structure. Mobile navigation uses a CSS-only checkbox+label pattern (no JS). Visual verification per task by serving locally with `python3 -m http.server`.

**Tech Stack:** Plain HTML5, CSS3 (custom properties, Grid, `clamp()`, `backdrop-filter`), Google Fonts (Inter + JetBrains Mono), Font Awesome 5 (already linked via CDN). No build step. GitHub Pages hosting.

**Spec reference:** `docs/superpowers/specs/2026-05-28-portfolio-redesign-design.md` — read it before starting if you haven't already.

---

## Adaptation note for static HTML/CSS

This project has no test framework. "Tests" in each task = serve the site locally and verify in a browser. The local server should be started once at the very beginning of Task 1 and left running throughout all tasks. Each task ends with a specific visual checklist plus a commit.

Pre-flight (do once before Task 1):

```bash
cd /Users/muchamad_mafmudin/project/github-io
python3 -m http.server 8000 &
# Browser: open http://localhost:8000 and http://localhost:8000/aboutme.html
# Keep both tabs open. Refresh after each task.
```

Stop the server when done with `kill %1` (or close the terminal).

---

## File-level plan

**Modified:**
- `index.html` — full rewrite of `<head>` and `<body>`
- `aboutme.html` — full rewrite of `<head>` and `<body>`
- `css/style.css` — written from empty to ~7 KB
- `CLAUDE.md` — replaced

**Created:**
- `README.md` — short replacement for `README.txt`

**Deleted (batch in Task 11):**
```
css/bootstrap.css            css/bootstrap.min.css
css/mdb.css                  css/mdb.lite.css
css/mdb.lite.min.css         css/mdb.lite.min.css.map
css/mdb.min.css              css/mdb.min.css.map
css/modules/                 (animations-extended, unused)
js/bootstrap.js              js/bootstrap.min.js
js/jquery.js                 js/jquery.min.js
js/mdb.js                    js/mdb.min.js
js/mdb.lite.min.js.map       js/mdb.min.js.map
js/popper.js                 js/popper.min.js
js/modules/                  (wow, treeview, scrolling-navbar, forms — all unused)
js/addons/                   (datatables, masonry, etc. — all unused)
scss/                        (entire MDB Sass source)
src/                         (entire MDB vendor JS source)
exclusive-templates/         (MDB freebies promo)
README.txt                   (MDB README, not user's)
Useful_Resources.pdf         (MDB freebie — confirm in audit then remove)
License.pdf                  (audit: keep if user's, remove if MDB's)
```

---

## Task 1: Strip vendor refs from both HTML heads + add Google Fonts

**Goal:** Both pages reference only `css/style.css` (still empty) plus Google Fonts and Font Awesome. No more Bootstrap/MDB/jQuery loads. Bodies stay untouched in this task — pages will look unstyled until Task 2.

**Files:**
- Modify: `index.html:1-20` (head section) and `index.html:170-184` (script block at bottom)
- Modify: `aboutme.html:1-20` (head section) and `aboutme.html:395-408` (script block at bottom)

- [ ] **Step 1: Update `<head>` of `index.html`**

Replace lines 1-20 with:

```html
<!DOCTYPE html>
<html lang="id">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <meta http-equiv="x-ua-compatible" content="ie=edge">
  <title>Muchamad Mafmudin</title>
  <link rel="icon" href="img/favicon.ico" type="image/x-icon">
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link rel="stylesheet" href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700;800&family=JetBrains+Mono:wght@500&display=swap">
  <link rel="stylesheet" href="https://use.fontawesome.com/releases/v5.11.2/css/all.css">
  <link rel="stylesheet" href="css/style.css">
</head>
```

Key changes: `lang="id"` (Indonesian), drop Bootstrap + MDB CSS, drop the old Google Fonts Roboto link.

- [ ] **Step 2: Remove `<script>` block at bottom of `index.html`**

Delete lines 170-184 (everything between `<!-- SCRIPTS -->` and `</body>` inclusive of all `<script>` tags, but keep `</body></html>`). After deletion the bottom of the file should end with:

```html
  </main>

  <!-- End your project here-->

</body>
</html>
```

- [ ] **Step 3: Update `<head>` of `aboutme.html`**

Replace lines 1-20 with the same block as Step 1 except change the title:

```html
<!DOCTYPE html>
<html lang="id">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <meta http-equiv="x-ua-compatible" content="ie=edge">
  <title>Muchamad Mafmudin · Portofolio</title>
  <link rel="icon" href="img/favicon.ico" type="image/x-icon">
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link rel="stylesheet" href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700;800&family=JetBrains+Mono:wght@500&display=swap">
  <link rel="stylesheet" href="https://use.fontawesome.com/releases/v5.11.2/css/all.css">
  <link rel="stylesheet" href="css/style.css">
</head>
```

- [ ] **Step 4: Remove `<script>` block at bottom of `aboutme.html`**

Delete the `<!-- SCRIPTS -->` block and all `<script>` tags from `aboutme.html` (around lines 395-408). End of file should be:

```html
  </main>

  <!-- End your project here-->

</body>
</html>
```

- [ ] **Step 5: Verify in browser**

Refresh both tabs. Expected: pages render unstyled (default browser styles), white background, Times New Roman or default font, hyperlinks blue, navbar unstyled. The Inter font may be loaded but not applied to anything yet (CSS file is empty). **This is correct.** If you see MDB styles still applying, something didn't get removed — re-check.

- [ ] **Step 6: Commit**

```bash
git add index.html aboutme.html
git commit -m "refactor: strip MDB/Bootstrap/jQuery from page heads

Drop all vendor CSS+JS references from index.html and aboutme.html.
Pages now reference only the (empty) css/style.css plus Google Fonts
(Inter + JetBrains Mono) and the existing Font Awesome CDN link.

Pages render unstyled until Task 2 adds design tokens and reset.

Co-Authored-By: Claude Opus 4.7 (1M context) <noreply@anthropic.com>"
```

---

## Task 2: CSS foundation — design tokens + reset

**Goal:** `css/style.css` has `:root` design tokens (color, space, radius, type, motion) and a minimal CSS reset. Pages now have a dark background, default Inter typography on body, and consistent box-sizing.

**Files:**
- Modify: `css/style.css` (currently empty)

- [ ] **Step 1: Write tokens + reset to `css/style.css`**

```css
/* ============================================================
   Portfolio · Mafmudin
   Single stylesheet. No framework. Dark + sage accent.
   Sections:
     1. Tokens
     2. Reset & base
     3. Typography
     4. Layout utilities
     5. Components (navbar, card, button, tag, footer)
     6. Page-specific (hero, about)
     7. Reduced motion
   ============================================================ */

/* ---------- 1. Tokens ---------- */

:root {
  /* Colors */
  --bg-base: #0c0f0d;
  --bg-surface: #131a16;
  --bg-elevated: #1a2218;
  --border: rgba(163, 177, 138, 0.15);
  --border-strong: rgba(163, 177, 138, 0.35);
  --text-primary: #ffffff;
  --text-secondary: #cbd0c8;
  --text-muted: #9ca39a;
  --text-faint: #6b7268;
  --accent-from: #a3b18a;
  --accent-to: #588157;
  --accent-glow: rgba(132, 169, 140, 0.35);

  /* Spacing */
  --space-1: 4px;
  --space-2: 8px;
  --space-3: 12px;
  --space-4: 16px;
  --space-5: 24px;
  --space-6: 32px;
  --space-7: 48px;
  --space-8: 64px;
  --space-9: 96px;
  --space-10: 128px;

  /* Radius */
  --radius-sm: 8px;
  --radius-md: 12px;
  --radius-lg: 16px;
  --radius-pill: 999px;

  /* Type */
  --font-sans: 'Inter', -apple-system, system-ui, sans-serif;
  --font-mono: 'JetBrains Mono', ui-monospace, monospace;

  /* Motion */
  --transition: 180ms ease;

  /* Layout */
  --nav-height: 64px;
  --container-max: 1100px;
}

/* ---------- 2. Reset & base ---------- */

*, *::before, *::after { box-sizing: border-box; }
* { margin: 0; }

html { -webkit-text-size-adjust: 100%; scroll-behavior: smooth; }

body {
  font-family: var(--font-sans);
  font-size: 15px;
  line-height: 1.6;
  color: var(--text-secondary);
  background: var(--bg-base);
  min-height: 100vh;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}

img, svg, video { display: block; max-width: 100%; height: auto; }

a { color: inherit; text-decoration: none; }

button { font: inherit; cursor: pointer; background: none; border: 0; color: inherit; padding: 0; }

ul { list-style: none; padding: 0; }
```

- [ ] **Step 2: Verify in browser**

Refresh both pages. Expected: dark background `#0c0f0d`, light-gray Inter text everywhere, no list bullets, no underlines on links (links look like plain text, that's fine for now). The page chrome from the current HTML markup will look stripped — that's intentional, components come in later tasks.

- [ ] **Step 3: Commit**

```bash
git add css/style.css
git commit -m "feat(css): add design tokens and base reset

Establish color/spacing/radius/type/motion variables in :root,
plus a minimal reset so subsequent components compose cleanly.

Co-Authored-By: Claude Opus 4.7 (1M context) <noreply@anthropic.com>"
```

---

## Task 3: Typography rules

**Goal:** Headings, paragraphs, and inline labels render with the spec's type scale. Existing HTML headings instantly look better even before the rewrite.

**Files:**
- Modify: `css/style.css` (append section 3)

- [ ] **Step 1: Append typography to `css/style.css`**

```css

/* ---------- 3. Typography ---------- */

h1, h2, h3, h4 {
  color: var(--text-primary);
  font-weight: 700;
  line-height: 1.15;
  letter-spacing: -0.02em;
}

h1 {
  font-weight: 800;
  font-size: clamp(40px, 6vw, 72px);
  line-height: 0.95;
  letter-spacing: -0.035em;
}

h2 { font-size: 32px; }
h3 { font-size: 22px; font-weight: 600; letter-spacing: -0.01em; }
h4 { font-size: 14px; font-weight: 600; letter-spacing: 0; color: var(--text-secondary); }

p { color: var(--text-secondary); }

.accent {
  background: linear-gradient(135deg, var(--accent-from), var(--accent-to));
  -webkit-background-clip: text;
  background-clip: text;
  -webkit-text-fill-color: transparent;
  color: transparent;
}
```

- [ ] **Step 2: Verify in browser**

Refresh both pages. Expected: existing `<h1>` "ProDialog" and "MPopUp" on index.html now render very large, white, weight 800. The h3 "Android Developer" on aboutme.html is now mid-size, weight 600, white. Section h2 "Aplikasi Saya" is 32px white.

- [ ] **Step 3: Commit**

```bash
git add css/style.css
git commit -m "feat(css): add typography scale

Heading hierarchy (h1 hero / h2 section / h3 card / h4 label) plus
.accent text-gradient utility for highlighted name fragments.

Co-Authored-By: Claude Opus 4.7 (1M context) <noreply@anthropic.com>"
```

---

## Task 4: Layout utilities — container, section, grid

**Goal:** `.container`, `.section`, `.grid`, `.grid-2`, `.grid-3` utilities exist. Spacing between sections becomes consistent.

**Files:**
- Modify: `css/style.css` (append section 4)

- [ ] **Step 1: Append layout utilities to `css/style.css`**

```css

/* ---------- 4. Layout utilities ---------- */

.container {
  width: 100%;
  max-width: var(--container-max);
  margin: 0 auto;
  padding: 0 24px;
}
@media (min-width: 1024px) {
  .container { padding: 0 48px; }
}

.section {
  padding-top: var(--space-9);
  padding-bottom: var(--space-9);
}
@media (min-width: 1024px) {
  .section {
    padding-top: var(--space-10);
    padding-bottom: var(--space-10);
  }
}

.section-label {
  font-family: var(--font-mono);
  font-size: 11px;
  font-weight: 500;
  letter-spacing: 0.18em;
  text-transform: uppercase;
  color: var(--text-muted);
  margin-bottom: var(--space-4);
}

.grid {
  display: grid;
  gap: var(--space-5);
  grid-template-columns: 1fr;
}
@media (min-width: 640px) {
  .grid-2 { grid-template-columns: repeat(2, 1fr); }
}
@media (min-width: 1024px) {
  .grid-3 { grid-template-columns: repeat(3, 1fr); }
}
```

- [ ] **Step 2: Verify in browser**

Refresh. No visible change yet (no element has these classes), but no regression either. The grid utilities will be applied when bodies are rewritten in later tasks.

- [ ] **Step 3: Commit**

```bash
git add css/style.css
git commit -m "feat(css): add container, section, and grid utilities

CSS Grid replaces Bootstrap's row/col system. .grid-2 and .grid-3
provide responsive 2- and 3-column layouts at 640px and 1024px
breakpoints.

Co-Authored-By: Claude Opus 4.7 (1M context) <noreply@anthropic.com>"
```

---

## Task 5: Components — buttons, tags

**Goal:** `.btn`, `.btn-primary`, `.btn-outline`, `.tag` render per spec.

**Files:**
- Modify: `css/style.css` (append section 5a)

- [ ] **Step 1: Append button + tag CSS**

```css

/* ---------- 5. Components ---------- */

/* Buttons */
.btn {
  display: inline-flex;
  align-items: center;
  gap: 6px;
  font-family: var(--font-sans);
  font-size: 14px;
  font-weight: 600;
  line-height: 1;
  padding: 11px 18px;
  border-radius: var(--radius-sm);
  border: 1px solid transparent;
  transition: transform var(--transition),
              box-shadow var(--transition),
              border-color var(--transition),
              color var(--transition),
              background var(--transition);
  white-space: nowrap;
}

.btn-primary {
  color: var(--bg-base);
  background: linear-gradient(135deg, var(--accent-from), var(--accent-to));
}
.btn-primary:hover {
  transform: translateY(-1px);
  box-shadow: 0 12px 28px var(--accent-glow);
}

.btn-outline {
  color: var(--text-secondary);
  border-color: var(--border-strong);
}
.btn-outline:hover {
  border-color: var(--accent-from);
  color: var(--text-primary);
}

/* Tag (▸ chip) */
.tag {
  display: inline-block;
  font-family: var(--font-mono);
  font-size: 10px;
  font-weight: 500;
  letter-spacing: 0.15em;
  text-transform: uppercase;
  color: var(--accent-from);
  background: rgba(163, 177, 138, 0.15);
  padding: 4px 10px;
  border-radius: var(--radius-pill);
  margin-bottom: var(--space-4);
}
```

- [ ] **Step 2: Verify in browser**

Refresh. No visible buttons yet (existing markup uses `.btn-primary` from MDB-with-`waves-effect` which we may partially intersect — they should now look like our new sage gradient buttons, which is fine as a preview). The `.tag` doesn't exist anywhere in old markup, so nothing to see yet.

- [ ] **Step 3: Commit**

```bash
git add css/style.css
git commit -m "feat(css): add button (primary + outline) and tag components

Primary button uses 135deg sage gradient with dark text; outline
button is transparent with strong sage border. Tag is a pill-shaped
mono-font chip used as the small ▸ category label on cards.

Co-Authored-By: Claude Opus 4.7 (1M context) <noreply@anthropic.com>"
```

---

## Task 6: Component — card with glow blob

**Goal:** `.card`, `.card-image`, `.card-title`, `.card-desc` styled. Each card has a sage glow blob in the top-right corner and lifts on hover.

**Files:**
- Modify: `css/style.css` (append section 5b)

- [ ] **Step 1: Append card CSS**

```css

/* Card */
.card {
  position: relative;
  background: var(--bg-surface);
  border: 1px solid var(--border);
  border-radius: var(--radius-lg);
  padding: 24px;
  overflow: hidden;
  transition: background var(--transition),
              border-color var(--transition),
              transform var(--transition);
  display: flex;
  flex-direction: column;
}

.card::before {
  content: '';
  position: absolute;
  top: -40px;
  right: -40px;
  width: 160px;
  height: 160px;
  background: radial-gradient(circle, var(--accent-glow) 0%, transparent 70%);
  filter: blur(20px);
  pointer-events: none;
  z-index: 0;
}

.card > * { position: relative; z-index: 1; }

.card:hover {
  background: var(--bg-elevated);
  border-color: var(--border-strong);
  transform: translateY(-2px);
}

.card-image {
  margin: -24px -24px 20px;
  aspect-ratio: 16 / 9;
  overflow: hidden;
  background: var(--bg-elevated);
  border-bottom: 1px solid var(--border);
}
.card-image img {
  width: 100%;
  height: 100%;
  object-fit: cover;
}

.card-title { margin-bottom: var(--space-2); }
.card-desc {
  font-size: 14px;
  color: var(--text-muted);
  margin-bottom: var(--space-5);
  flex: 1;
}
.card .btn { align-self: flex-start; }
```

- [ ] **Step 2: Verify in browser**

Refresh. Old MDB `.card` elements now render with our new dark surface, sage glow, and hover lift. Image areas will look broken because the old markup uses `<iframe>` inside `.embed-responsive` containers — fine, those get replaced in Tasks 9-10.

- [ ] **Step 3: Commit**

```bash
git add css/style.css
git commit -m "feat(css): add card component with glow blob and hover lift

Cards have dark surface, 1px sage border at low opacity, 16px radius,
and a radial sage glow blob positioned in the top-right. Hover state
elevates the background, brightens the border, and lifts the card 2px.

Co-Authored-By: Claude Opus 4.7 (1M context) <noreply@anthropic.com>"
```

---

## Task 7: Component — navbar with CSS-only mobile menu

**Goal:** Navbar styled with backdrop blur, brand wordmark, active link underline, social cluster. Mobile menu toggles via checkbox+label (no JS).

**Files:**
- Modify: `css/style.css` (append section 5c)

- [ ] **Step 1: Append navbar CSS**

```css

/* Navbar */
.navbar {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  z-index: 50;
  height: var(--nav-height);
  display: flex;
  align-items: center;
  background: rgba(12, 15, 13, 0.8);
  backdrop-filter: blur(12px);
  -webkit-backdrop-filter: blur(12px);
  border-bottom: 1px solid var(--border);
}

.navbar-inner {
  width: 100%;
  max-width: var(--container-max);
  margin: 0 auto;
  padding: 0 24px;
  display: flex;
  align-items: center;
  gap: var(--space-6);
}
@media (min-width: 1024px) {
  .navbar-inner { padding: 0 48px; }
}

.navbar-brand {
  display: inline-flex;
  align-items: center;
  gap: 10px;
  font-family: var(--font-sans);
  font-weight: 600;
  font-size: 15px;
  color: var(--text-primary);
}
.navbar-brand::before {
  content: '';
  width: 8px;
  height: 8px;
  border-radius: 50%;
  background: linear-gradient(135deg, var(--accent-from), var(--accent-to));
  box-shadow: 0 0 12px var(--accent-glow);
}

.navbar-links {
  display: flex;
  gap: var(--space-5);
  align-items: center;
}
.navbar-links a {
  position: relative;
  font-size: 14px;
  color: var(--text-muted);
  transition: color var(--transition);
}
.navbar-links a:hover { color: var(--text-primary); }
.navbar-links a[aria-current="page"] { color: var(--text-primary); }
.navbar-links a[aria-current="page"]::after {
  content: '';
  position: absolute;
  bottom: -10px;
  left: 50%;
  transform: translateX(-50%);
  width: 16px;
  height: 2px;
  background: linear-gradient(135deg, var(--accent-from), var(--accent-to));
  border-radius: 1px;
}

.navbar-social {
  margin-left: auto;
  display: flex;
  gap: var(--space-4);
  align-items: center;
}
.navbar-social a {
  color: var(--text-muted);
  font-size: 15px;
  transition: color var(--transition);
}
.navbar-social a:hover { color: var(--text-primary); }
.navbar-social .github {
  display: inline-flex;
  align-items: center;
  gap: 6px;
  font-size: 13px;
  font-weight: 500;
  padding: 7px 12px;
  border: 1px solid var(--border-strong);
  border-radius: var(--radius-sm);
  color: var(--text-secondary);
}
.navbar-social .github:hover {
  border-color: var(--accent-from);
  color: var(--text-primary);
}

/* Mobile menu — CSS-only checkbox + label */
.nav-toggle { display: none; }
.nav-toggle-label { display: none; }

@media (max-width: 767px) {
  .nav-toggle-label {
    display: inline-flex;
    align-items: center;
    justify-content: center;
    margin-left: auto;
    width: 32px;
    height: 32px;
    cursor: pointer;
    color: var(--text-primary);
    font-size: 18px;
  }
  .nav-toggle-label::before { content: '☰'; }
  .nav-toggle:checked ~ .nav-toggle-label::before { content: '✕'; font-size: 16px; }

  .navbar-links {
    position: absolute;
    top: var(--nav-height);
    left: 0;
    right: 0;
    background: var(--bg-base);
    border-bottom: 1px solid var(--border);
    padding: var(--space-5);
    flex-direction: column;
    align-items: flex-start;
    gap: var(--space-4);
    display: none;
  }
  .nav-toggle:checked ~ .navbar-links { display: flex; }

  /* On mobile, social cluster collapses to nothing — those links live in the footer. */
  .navbar-social { display: none; }
}
```

- [ ] **Step 2: Verify in browser**

Refresh. The old MDB navbar markup will look broken because our selectors `.navbar-inner`, `.navbar-brand`, `.navbar-links`, `.navbar-social` don't match the old markup. **This is expected** — Task 9 rewrites the navbar HTML to use the new class names. For now you can verify by checking DevTools that the CSS rules are present and would apply to elements with these classes.

- [ ] **Step 3: Commit**

```bash
git add css/style.css
git commit -m "feat(css): add fixed navbar with backdrop blur and mobile menu

Navbar uses position:fixed, 80% opaque dark background with 12px
backdrop blur, and a sage gradient dot before the brand wordmark.
Active page link gets a small sage underline. Mobile menu uses a
CSS-only checkbox+label toggle — no JavaScript.

Co-Authored-By: Claude Opus 4.7 (1M context) <noreply@anthropic.com>"
```

---

## Task 8: Component — footer + hero + about-grid + reduced-motion

**Goal:** Final CSS sections. `.footer`, `.hero`, `.hero-compact`, `.about-grid`, `.about-photo` styles plus `prefers-reduced-motion`. After this task `css/style.css` is complete (~7 KB).

**Files:**
- Modify: `css/style.css` (append sections 5d + 6 + 7)

- [ ] **Step 1: Append footer + page-specific CSS**

```css

/* Footer */
.footer {
  border-top: 1px solid var(--border);
  padding: var(--space-8) 0 var(--space-6);
  margin-top: var(--space-9);
  color: var(--text-faint);
  font-size: 13px;
}
.footer-inner {
  display: grid;
  gap: var(--space-6);
  grid-template-columns: 1fr;
}
@media (min-width: 768px) {
  .footer-inner { grid-template-columns: 2fr 1fr 1fr; gap: var(--space-8); }
}
.footer h4 {
  font-size: 13px;
  font-weight: 600;
  margin-bottom: var(--space-3);
  color: var(--text-secondary);
}
.footer ul { display: flex; flex-direction: column; gap: var(--space-2); }
.footer a { color: var(--text-faint); transition: color var(--transition); }
.footer a:hover { color: var(--text-primary); }
.footer-social { flex-direction: row !important; gap: var(--space-4) !important; }
.footer-social a { font-size: 16px; }
.footer-bottom {
  margin-top: var(--space-7);
  padding-top: var(--space-5);
  border-top: 1px solid var(--border);
  text-align: center;
  font-size: 12px;
  color: var(--text-faint);
}

/* ---------- 6. Page-specific ---------- */

/* Hero — typographic, used on aboutme.html */
.hero {
  position: relative;
  padding-top: calc(var(--nav-height) + var(--space-9));
  padding-bottom: var(--space-9);
  overflow: hidden;
}
.hero::before {
  content: '';
  position: absolute;
  bottom: -160px;
  left: -120px;
  width: 480px;
  height: 480px;
  background: radial-gradient(circle, var(--accent-glow) 0%, transparent 70%);
  filter: blur(50px);
  pointer-events: none;
  z-index: 0;
}
.hero > * { position: relative; z-index: 1; }
.hero-label {
  font-family: var(--font-mono);
  font-size: 11px;
  font-weight: 500;
  letter-spacing: 0.18em;
  text-transform: uppercase;
  color: var(--accent-from);
  margin-bottom: var(--space-4);
}
.hero h1 {
  margin-bottom: var(--space-4);
  max-width: 14ch;
}
.hero-lead {
  font-size: 17px;
  color: var(--text-secondary);
  max-width: 48ch;
  margin-bottom: var(--space-6);
  line-height: 1.55;
}
.hero-ctas {
  display: flex;
  gap: var(--space-3);
  flex-wrap: wrap;
}

/* Hero compact — used on index.html */
.hero-compact { padding-bottom: var(--space-7); }
.hero-compact h1 {
  font-size: clamp(32px, 4.5vw, 52px);
  max-width: 20ch;
}

/* About (Tentang Saya) */
.about-grid {
  display: grid;
  gap: var(--space-5);
  align-items: center;
  grid-template-columns: 1fr;
}
@media (min-width: 640px) {
  .about-grid {
    grid-template-columns: 96px 1fr;
    gap: var(--space-6);
  }
}
.about-photo {
  width: 96px;
  height: 96px;
  border-radius: 14px;
  border: 1px solid var(--border-strong);
  object-fit: cover;
}

/* ---------- 7. Reduced motion ---------- */
@media (prefers-reduced-motion: reduce) {
  *, *::before, *::after {
    transition-duration: 0.001ms !important;
    animation-duration: 0.001ms !important;
    scroll-behavior: auto !important;
  }
}
```

- [ ] **Step 2: Verify in browser**

Refresh. Still nothing visible from these new styles because the HTML doesn't yet use these classes — that happens in Task 9 (aboutme.html) and Task 10 (index.html).

Quick sanity check: open DevTools, select `body`, computed style should show `background-color: rgb(12, 15, 13)`. If yes, CSS is loading. If no, check the `<link>` in HTML.

- [ ] **Step 3: Commit**

```bash
git add css/style.css
git commit -m "feat(css): add footer, hero, about-grid, and reduced-motion

Complete the stylesheet. Footer is a 3-column grid (stacked on mobile)
with a copyright row. Hero has a large bottom-left sage glow blob.
About-grid pairs the small photo with a bio paragraph at 640px+.
Reduced-motion media query disables all transitions/animations.

Co-Authored-By: Claude Opus 4.7 (1M context) <noreply@anthropic.com>"
```

---

## Task 9: Rewrite `aboutme.html` body

**Goal:** Replace the entire `<body>` of `aboutme.html` with the new semantic structure: navbar + hero + Tentang Saya + Aplikasi Saya + Project Saya + footer. Page should look like the approved mockups.

**Files:**
- Modify: `aboutme.html` — replace everything between `<body>` and `</body>`

- [ ] **Step 1: Replace body content of `aboutme.html`**

Replace everything between (and including) `<body>` and `</body>` with:

```html
<body>

<nav class="navbar">
  <div class="navbar-inner">
    <a class="navbar-brand" href="index.html">Mafmudin</a>
    <input type="checkbox" id="nav-toggle" class="nav-toggle">
    <label for="nav-toggle" class="nav-toggle-label" aria-label="Toggle navigation"></label>
    <ul class="navbar-links">
      <li><a href="index.html">Beranda</a></li>
      <li><a href="aboutme.html" aria-current="page">Portofolio</a></li>
    </ul>
    <ul class="navbar-social">
      <li><a href="https://www.facebook.com/mafmudin/" target="_blank" rel="noopener" aria-label="Facebook"><i class="fab fa-facebook-f"></i></a></li>
      <li><a href="https://twitter.com/mafmudin" target="_blank" rel="noopener" aria-label="Twitter"><i class="fab fa-twitter"></i></a></li>
      <li><a href="https://www.instagram.com/muchama_m/" target="_blank" rel="noopener" aria-label="Instagram"><i class="fab fa-instagram"></i></a></li>
      <li><a class="github" href="https://github.com/Mafmudin" target="_blank" rel="noopener"><i class="fab fa-github"></i> GitHub</a></li>
    </ul>
  </div>
</nav>

<main>

  <section class="hero">
    <div class="container">
      <div class="hero-label">— Portofolio · 2026</div>
      <h1>Halo, aku <span class="accent">Mafmudin</span>.</h1>
      <p class="hero-lead">Android developer. Membuat aplikasi yang mudah digunakan. Lihat aplikasi & project di bawah.</p>
      <div class="hero-ctas">
        <a class="btn btn-primary" href="mycv.svg" target="_blank" rel="noopener">Lihat CV →</a>
        <a class="btn btn-outline" href="https://github.com/Mafmudin" target="_blank" rel="noopener">GitHub ↗</a>
      </div>
    </div>
  </section>

  <section class="section">
    <div class="container">
      <div class="section-label">— Tentang Saya</div>
      <div class="about-grid">
        <img class="about-photo" src="img/poto.jpeg" alt="Foto Muchamad Mafmudin">
        <p>Android developer dengan fokus pada UI yang ringan dan mudah dipakai. Aktif merilis library kecil di GitHub seperti ProDialog dan MPopUp.</p>
      </div>
    </div>
  </section>

  <section class="section">
    <div class="container">
      <div class="section-label">— Aplikasi</div>
      <h2>Aplikasi Saya</h2>
      <div class="grid grid-3" style="margin-top: 32px;">

        <article class="card">
          <div class="card-image">
            <img src="https://play-lh.googleusercontent.com/vH0PTPgfw6IkVCZHGTWLWykaoW4lqV0O7_WI_ftU7JfuRh7dewjEMe3D5RIgbjXz4uY=s180-rw" alt="Ikon aplikasi GamaPro">
          </div>
          <div class="tag">▸ Android App</div>
          <h3 class="card-title">GamaPro</h3>
          <p class="card-desc">Lihat GamaPro di Play Store.</p>
          <a class="btn btn-primary" href="https://play.google.com/store/apps/details?id=dev.fordewe.gamapro" target="_blank" rel="noopener">Unduh Gratis ↗</a>
        </article>

        <article class="card">
          <div class="card-image">
            <img src="https://play-lh.googleusercontent.com/gdlBfaWgYBU528qc26NJ43qkYH29JlrWBYFu4oAoyPFFUBWo1bZtttouCAc0-Iotag=s180-rw" alt="Ikon aplikasi EDayaK">
          </div>
          <div class="tag">▸ Android App</div>
          <h3 class="card-title">EDayaK</h3>
          <p class="card-desc">Lihat EDayaK di Play Store.</p>
          <a class="btn btn-primary" href="https://play.google.com/store/apps/details?id=dev.fordewe.smartvilage" target="_blank" rel="noopener">Unduh Gratis ↗</a>
        </article>

        <article class="card">
          <div class="card-image">
            <img src="https://play-lh.googleusercontent.com/5MbkwUSpngKql0Vg34fIYLtaQ97NzDuouEc4LlTkyd3Btq3h3ATDt_sY8U_WQcvMdF1z=s180-rw" alt="Ikon aplikasi Mobile City Yogyakarta">
          </div>
          <div class="tag">▸ Android App</div>
          <h3 class="card-title">Mobile City (Yogyakarta)</h3>
          <p class="card-desc">Lihat Mobile City di Play Store.</p>
          <a class="btn btn-primary" href="https://play.google.com/store/apps/details?id=dev.fordewe.mobilecity" target="_blank" rel="noopener">Unduh Gratis ↗</a>
        </article>

      </div>
    </div>
  </section>

  <section class="section">
    <div class="container">
      <div class="section-label">— Project</div>
      <h2>Project Saya</h2>
      <div class="grid grid-2" style="margin-top: 32px;">

        <article class="card">
          <div class="card-image">
            <img src="https://play-lh.googleusercontent.com/pGpoivL4nU-_aGcwXExt8gavi6ZrO_uCuchhvBeJuau-U_IpzKE_irHKgekafBA8IOg=s180-rw" alt="Ikon Get Indonesia Customer">
          </div>
          <div class="tag">▸ Project Klien</div>
          <h3 class="card-title">Get Indonesia Customer</h3>
          <p class="card-desc">Lihat Get Indonesia Customer di Play Store.</p>
          <a class="btn btn-primary" href="https://play.google.com/store/apps/details?id=com.getindonesia.customer" target="_blank" rel="noopener">Buka di Play Store ↗</a>
        </article>

        <article class="card">
          <div class="card-image">
            <img src="https://play-lh.googleusercontent.com/TVtEAVJnHj9Cg0WIC0R5UAz4kQ3IcYXOQQdkCIYSw8lFfQyYUsuv0Er1tlZ2Mnsz9Q=s180-rw" alt="Ikon Desa Apps">
          </div>
          <div class="tag">▸ Project Klien</div>
          <h3 class="card-title">Desa Apps</h3>
          <p class="card-desc">Lihat Desa Apps di Play Store.</p>
          <a class="btn btn-primary" href="https://play.google.com/store/apps/details?id=com.gamatechno.desaapps" target="_blank" rel="noopener">Buka di Play Store ↗</a>
        </article>

        <article class="card">
          <div class="card-image">
            <img src="https://play-lh.googleusercontent.com/xTGF8drfPp6Ay4QH98-gu3NaynaTKcYoKbZon6sgw46LuOWU7CSy38P9k2ABIBXOM3k9=s180-rw" alt="Ikon Cared+">
          </div>
          <div class="tag">▸ Project Klien</div>
          <h3 class="card-title">Cared+</h3>
          <p class="card-desc">Lihat Cared+ di Play Store.</p>
          <a class="btn btn-primary" href="https://play.google.com/store/apps/details?id=com.gamatechno.caredplus" target="_blank" rel="noopener">Buka di Play Store ↗</a>
        </article>

        <article class="card">
          <div class="card-image">
            <img src="https://play-lh.googleusercontent.com/PpVDX07bzOauy6hO4EGSm3GzBFk4kBbiLvyf0KdB6hqIKsCpe3VN83bJpmETQ7yRIg=s180-rw" alt="Ikon Hangout Deals">
          </div>
          <div class="tag">▸ Project Klien</div>
          <h3 class="card-title">Hangout Deals</h3>
          <p class="card-desc">Lihat Hangout Deals di Play Store.</p>
          <a class="btn btn-primary" href="https://play.google.com/store/apps/details?id=com.luffycode.hangout" target="_blank" rel="noopener">Buka di Play Store ↗</a>
        </article>

      </div>
    </div>
  </section>

</main>

<footer class="footer">
  <div class="container">
    <div class="footer-inner">
      <div>
        <h4>Mafmudin</h4>
        <p>Android developer. Membuat aplikasi yang mudah digunakan.</p>
      </div>
      <div>
        <h4>Tautan</h4>
        <ul>
          <li><a href="index.html">Beranda</a></li>
          <li><a href="aboutme.html">Portofolio</a></li>
          <li><a href="mycv.svg" target="_blank" rel="noopener">CV</a></li>
        </ul>
      </div>
      <div>
        <h4>Sosial</h4>
        <ul class="footer-social">
          <li><a href="https://www.facebook.com/mafmudin/" target="_blank" rel="noopener" aria-label="Facebook"><i class="fab fa-facebook-f"></i></a></li>
          <li><a href="https://twitter.com/mafmudin" target="_blank" rel="noopener" aria-label="Twitter"><i class="fab fa-twitter"></i></a></li>
          <li><a href="https://www.instagram.com/muchama_m/" target="_blank" rel="noopener" aria-label="Instagram"><i class="fab fa-instagram"></i></a></li>
          <li><a href="https://github.com/Mafmudin" target="_blank" rel="noopener" aria-label="GitHub"><i class="fab fa-github"></i></a></li>
        </ul>
      </div>
    </div>
    <div class="footer-bottom">© 2026 Muchamad Mafmudin · Made with care.</div>
  </div>
</footer>

</body>
```

- [ ] **Step 2: Verify in browser at 3 widths**

Refresh `http://localhost:8000/aboutme.html`. Check:

1. **Desktop (1280px):** Hero shows "Halo, aku **Mafmudin**." with the name in sage gradient. Big sage glow blob in bottom-left of hero. Two CTAs side by side. Aplikasi Saya = 3 cards in a row. Project Saya = 2×2 grid. Each card has glow blob top-right, hover lifts the card. Navbar fixed top with active "Portofolio" underline. Footer has 3 columns + copyright.
2. **Tablet (768px):** Aplikasi Saya stays 3 columns (1024px breakpoint not yet hit — actually it switches to 1 column below 1024). Project Saya is 2 columns. Footer stacks to 3 cols still (768px+ breakpoint hits).
3. **Mobile (360px):** Navbar collapses to brand + ☰. Click ☰ → menu drops down with the two main links only (Beranda / Portofolio); ☰ becomes ✕. Social icons are hidden in the navbar on mobile — they still appear in the footer. All grids stack to 1 column. Hero text scales down via `clamp()`. About photo + bio stack vertically.

If any visual is off (broken alignment, missing glow, wrong color), inspect with DevTools and fix the offending CSS rule before committing.

- [ ] **Step 3: Verify external links still work**

Click through each Play Store link, CV link, GitHub link, social icons in nav + footer. All should open in new tabs and lead to the correct destinations (no 404s). External targets are unchanged from the original `aboutme.html`.

- [ ] **Step 4: Commit**

```bash
git add aboutme.html
git commit -m "feat(html): rewrite aboutme.html with semantic dark-mode structure

Replace the MDB markup with: typographic hero (name in sage gradient,
two CTAs), Tentang Saya section, Aplikasi Saya 3-col grid (3 apps),
Project Saya 2x2 grid (4 client projects), and a 3-column footer.
Card images switch from <iframe> embeds to <img> tags pointing at
the same Play Store CDN URLs.

Co-Authored-By: Claude Opus 4.7 (1M context) <noreply@anthropic.com>"
```

---

## Task 10: Rewrite `index.html` body

**Goal:** Replace the body of `index.html` with the compact hero + 2-col library grid + footer.

**Files:**
- Modify: `index.html` — replace everything between `<body>` and `</body>`

- [ ] **Step 1: Replace body content of `index.html`**

Replace everything between (and including) `<body>` and `</body>` with:

```html
<body>

<nav class="navbar">
  <div class="navbar-inner">
    <a class="navbar-brand" href="index.html">Mafmudin</a>
    <input type="checkbox" id="nav-toggle" class="nav-toggle">
    <label for="nav-toggle" class="nav-toggle-label" aria-label="Toggle navigation"></label>
    <ul class="navbar-links">
      <li><a href="index.html" aria-current="page">Beranda</a></li>
      <li><a href="aboutme.html">Portofolio</a></li>
    </ul>
    <ul class="navbar-social">
      <li><a href="https://www.facebook.com/mafmudin/" target="_blank" rel="noopener" aria-label="Facebook"><i class="fab fa-facebook-f"></i></a></li>
      <li><a href="https://twitter.com/mafmudin" target="_blank" rel="noopener" aria-label="Twitter"><i class="fab fa-twitter"></i></a></li>
      <li><a href="https://www.instagram.com/muchama_m/" target="_blank" rel="noopener" aria-label="Instagram"><i class="fab fa-instagram"></i></a></li>
      <li><a class="github" href="https://github.com/Mafmudin" target="_blank" rel="noopener"><i class="fab fa-github"></i> GitHub</a></li>
    </ul>
  </div>
</nav>

<main>

  <section class="hero hero-compact">
    <div class="container">
      <div class="hero-label">— Open Source</div>
      <h1>Library <span class="accent">Android.</span></h1>
      <p class="hero-lead">Beberapa library kecil yang saya rilis di GitHub untuk komunitas Android.</p>
    </div>
  </section>

  <section class="section">
    <div class="container">
      <div class="grid grid-2">

        <article class="card">
          <div class="tag">▸ Open Source · Library</div>
          <h3 class="card-title">ProDialog</h3>
          <p class="card-desc">Library Android untuk menampilkan progress dialog dengan gambar bergerak (SVG &amp; GIF). Ditampilkan selama proses pemuatan data berlangsung, supaya view tidak menampilkan data yang belum siap.</p>
          <a class="btn btn-primary" href="https://mafmudin.github.io/progress-sg/" target="_blank" rel="noopener">Buka GitHub ↗</a>
        </article>

        <article class="card">
          <div class="tag">▸ Open Source · Library</div>
          <h3 class="card-title">MPopUp</h3>
          <p class="card-desc">Permudah pembuatan pop up di Android dengan MPopUp :-)</p>
          <a class="btn btn-primary" href="https://mafmudin.github.io/MPopUp/" target="_blank" rel="noopener">Buka GitHub ↗</a>
        </article>

      </div>
    </div>
  </section>

</main>

<footer class="footer">
  <div class="container">
    <div class="footer-inner">
      <div>
        <h4>Mafmudin</h4>
        <p>Android developer. Membuat aplikasi yang mudah digunakan.</p>
      </div>
      <div>
        <h4>Tautan</h4>
        <ul>
          <li><a href="index.html">Beranda</a></li>
          <li><a href="aboutme.html">Portofolio</a></li>
          <li><a href="mycv.svg" target="_blank" rel="noopener">CV</a></li>
        </ul>
      </div>
      <div>
        <h4>Sosial</h4>
        <ul class="footer-social">
          <li><a href="https://www.facebook.com/mafmudin/" target="_blank" rel="noopener" aria-label="Facebook"><i class="fab fa-facebook-f"></i></a></li>
          <li><a href="https://twitter.com/mafmudin" target="_blank" rel="noopener" aria-label="Twitter"><i class="fab fa-twitter"></i></a></li>
          <li><a href="https://www.instagram.com/muchama_m/" target="_blank" rel="noopener" aria-label="Instagram"><i class="fab fa-instagram"></i></a></li>
          <li><a href="https://github.com/Mafmudin" target="_blank" rel="noopener" aria-label="GitHub"><i class="fab fa-github"></i></a></li>
        </ul>
      </div>
    </div>
    <div class="footer-bottom">© 2026 Muchamad Mafmudin · Made with care.</div>
  </div>
</footer>

</body>
```

- [ ] **Step 2: Verify in browser at 3 widths**

Refresh `http://localhost:8000`. Check:

1. **Desktop (1280px):** Hero shows "Library **Android.**" with "Android." in sage gradient. Smaller bottom-left glow. Two library cards in 2-col grid below. Active nav link is "Beranda" with sage underline.
2. **Tablet (768px):** 2-col grid still 2 cols (640px breakpoint).
3. **Mobile (360px):** Grid stacks. Hamburger menu works.

- [ ] **Step 3: Commit**

```bash
git add index.html
git commit -m "feat(html): rewrite index.html with compact hero + library grid

Compact typographic hero ('Library Android.') sits above a 2-column
grid of library cards (ProDialog, MPopUp). Same navbar + footer as
aboutme.html. Pages now share a consistent visual system.

Co-Authored-By: Claude Opus 4.7 (1M context) <noreply@anthropic.com>"
```

---

## Task 11: Delete vendor files

**Goal:** Repo size drops from ~5 MB of vendor cruft to a lean static site. All MDB/Bootstrap/jQuery files and SCSS sources are gone.

**Files:** Many. See list below.

- [ ] **Step 1: Audit License.pdf and Useful_Resources.pdf**

Run from repo root:

```bash
pdfinfo License.pdf 2>/dev/null | head -5
pdfinfo Useful_Resources.pdf 2>/dev/null | head -5
```

Or `open License.pdf` and `open Useful_Resources.pdf` and read the first page. Both come from the MDB Bootstrap zip and are not the user's own content. Expected: License.pdf is the MDB license, Useful_Resources.pdf is the MDB resources list. Both safe to remove. If `pdfinfo` is missing, install with `brew install poppler` or just inspect visually.

- [ ] **Step 2: Delete vendor CSS files**

```bash
git rm css/bootstrap.css css/bootstrap.min.css
git rm css/mdb.css css/mdb.lite.css css/mdb.lite.min.css
git rm css/mdb.lite.min.css.map css/mdb.min.css css/mdb.min.css.map
git rm -r css/modules
```

- [ ] **Step 3: Delete vendor JS files**

```bash
git rm js/bootstrap.js js/bootstrap.min.js
git rm js/jquery.js js/jquery.min.js
git rm js/mdb.js js/mdb.min.js
git rm js/mdb.lite.min.js.map js/mdb.min.js.map
git rm js/popper.js js/popper.min.js
git rm -r js/modules js/addons
```

- [ ] **Step 4: Delete SCSS, vendor src, MDB templates, MDB readme**

```bash
git rm -r scss src exclusive-templates
git rm README.txt License.pdf Useful_Resources.pdf
```

- [ ] **Step 5: Audit `my-2020_Images/` and `img/`**

```bash
ls my-2020_Images/
ls img/
```

`my-2020_Images/` is an older asset folder. Check whether any of its files are referenced from the new HTML:

```bash
grep -rE "my-2020_Images/[^\"'>]*" index.html aboutme.html
```

Expected: no matches (the new HTML only references `img/poto.jpeg` and `img/favicon.ico`). If grep finds no matches, the folder is unused:

```bash
git rm -r my-2020_Images
```

If grep finds matches, leave the folder alone for now and proceed.

- [ ] **Step 6: Verify both pages still render correctly**

Refresh both tabs. Everything should still look identical to Task 9/10 results (the deleted files weren't referenced anyway). If anything broke, restore the file with `git checkout HEAD -- <file>` and investigate.

- [ ] **Step 7: Check final repo size**

```bash
du -sh .
git ls-files | wc -l
```

Expected: under 2 MB total, fewer than 20 tracked files (mostly img/ + mycv.svg + the two HTML + style.css + docs/).

- [ ] **Step 8: Commit**

```bash
git commit -m "chore: remove MDB/Bootstrap/jQuery vendor files

Strip ~450 KB of CSS and ~440 KB of JS that no longer have any
references in the rewritten pages. Also remove the MDB Sass source
(scss/, src/), the MDB freebies template (exclusive-templates/),
and the MDB-shipped README.txt, License.pdf, and Useful_Resources.pdf.
The 'my-2020_Images/' folder is dropped if unreferenced after audit.

Co-Authored-By: Claude Opus 4.7 (1M context) <noreply@anthropic.com>"
```

---

## Task 12: Update `CLAUDE.md` and replace `README.txt` with `README.md`

**Goal:** Documentation reflects the new architecture so future agents (and humans) don't get confused trying to apply MDB-era guidance.

**Files:**
- Modify: `CLAUDE.md` (replace contents)
- Create: `README.md`

- [ ] **Step 1: Replace `CLAUDE.md` contents**

Overwrite the entire file with:

```markdown
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
```

- [ ] **Step 2: Create `README.md`**

```markdown
# mafmudin.github.io

Personal portfolio site for Muchamad Mafmudin — Android developer.

- **Live:** <https://mafmudin.github.io>
- **Stack:** Static HTML + one CSS file. No build step, no JavaScript.
- **Preview locally:** `python3 -m http.server 8000` from repo root, open <http://localhost:8000>.
- **Deploy:** push to `master`. GitHub Pages serves the repo root.

See `CLAUDE.md` for architecture notes.
```

- [ ] **Step 3: Commit**

```bash
git add CLAUDE.md README.md
git commit -m "docs: rewrite CLAUDE.md and replace README for new architecture

CLAUDE.md no longer references MDB, Bootstrap, jQuery, or the
navbar duplication gotcha (none of which apply anymore). It now
describes the single-stylesheet structure, the CSS-only mobile
menu, and where to add new cards/components.

README.md replaces the old MDB-shipped README.txt with a short
project blurb.

Co-Authored-By: Claude Opus 4.7 (1M context) <noreply@anthropic.com>"
```

---

## Task 13: Final verification — Lighthouse + accessibility + link audit

**Goal:** The site meets the spec's acceptance criteria.

**Files:** None modified — just verification.

- [ ] **Step 1: Verify all original content is present**

Run from repo root:

```bash
grep -E "GamaPro|EDayaK|Mobile City|Get Indonesia Customer|Desa Apps|Cared|Hangout Deals|ProDialog|MPopUp" index.html aboutme.html
```

Expected: 9 matches (2 in index.html for ProDialog/MPopUp, 7 in aboutme.html for the apps and projects).

- [ ] **Step 2: Verify all external destinations**

```bash
grep -oE 'href="https?://[^"]+"' index.html aboutme.html | sort -u
```

Visually scan the list — there should be entries for:
- Each Play Store app URL (7 distinct apps)
- `https://github.com/Mafmudin`
- `https://mafmudin.github.io/progress-sg/` (ProDialog)
- `https://mafmudin.github.io/MPopUp/` (MPopUp)
- Facebook, Twitter, Instagram profiles
- Google Fonts + Font Awesome CDN links in `<head>`

If any expected URL is missing, the corresponding card lost its link during the rewrite — restore it.

- [ ] **Step 3: Lighthouse audit (Chrome DevTools)**

Open `http://localhost:8000/aboutme.html` in Chrome. DevTools → Lighthouse panel → check Performance + Accessibility + Best Practices + SEO → Mobile → Analyze.

Expected scores:
- Performance ≥ 95
- Accessibility ≥ 95
- Best Practices ≥ 95
- SEO ≥ 90

If Accessibility < 95, the most likely cause is missing `alt` on an image or missing `aria-label` on an icon-only link. Fix and re-run.

Repeat for `http://localhost:8000`.

- [ ] **Step 4: Manual responsive sweep**

DevTools device toolbar (Cmd+Shift+M). Cycle through:
- iPhone SE (375 × 667)
- iPad (768 × 1024)
- Laptop (1280 × 800)
- Desktop (1920 × 1080)

For each, verify:
- Navbar is readable and not overflowing
- Hero text fits without horizontal scroll
- Cards stack correctly at each breakpoint
- Mobile hamburger menu opens/closes by clicking ☰ / ✕
- Footer columns stack on mobile, 3-col on tablet+

- [ ] **Step 5: Stop the local server**

```bash
kill %1
```

(or close the terminal that's running `python3 -m http.server`)

- [ ] **Step 6: Final commit (if Lighthouse fixes were needed) and push**

If any fixes happened in Steps 1-4:

```bash
git add -A
git commit -m "fix: address Lighthouse and responsive sweep findings

Co-Authored-By: Claude Opus 4.7 (1M context) <noreply@anthropic.com>"
```

Then ask the user whether to push to `master` (which deploys to GitHub Pages). Do NOT push without explicit confirmation, since this affects the live site.

---

## Done criteria

- [ ] Both pages serve from `python3 -m http.server` and render correctly at 360 / 768 / 1280 px widths.
- [ ] Lighthouse Performance ≥ 95, Accessibility ≥ 95 on both pages.
- [ ] No `<script>` tags in either HTML file.
- [ ] All vendor MDB/Bootstrap/jQuery files removed from the repo.
- [ ] All original content (apps, libraries, projects, social links, CV) preserved and linking correctly.
- [ ] `CLAUDE.md` and `README.md` reflect the new architecture.
- [ ] Each task above committed separately with a descriptive message.
