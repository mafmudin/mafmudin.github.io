# Portfolio Redesign — Design Spec

**Date:** 2026-05-28
**Status:** Approved, ready for implementation planning
**Scope:** Modernize the visual look of both pages of `mafmudin.github.io`. Content stays; structure shifts slightly to give the photo a proper home and to remove vendor cruft. Implementation strips MDB Bootstrap entirely and rebuilds with a small custom CSS file — no framework, no JavaScript dependencies.

---

## 1. Goal & non-goals

**Goal.** Replace the 2019-era MDB Bootstrap look with a modern, distinctive dark-mode design that reads as a 2025 developer portfolio. Page weight drops from ~600 KB of CSS/JS to ~30 KB.

**Non-goals.**
- Not adding new portfolio entries, apps, or libraries — content is preserved verbatim, including Bahasa Indonesia copy.
- Not migrating to a static-site generator, build tool, or framework. The site stays as plain HTML files served by GitHub Pages.
- Not building a separate dark/light toggle. The site is dark-only.
- Not adding entrance animations or scroll-triggered effects. CSS hover transitions only.

---

## 2. Visual language

### Palette (dark + sage accent)

| Token | Value | Role |
|---|---|---|
| `--bg-base` | `#0c0f0d` | Page background |
| `--bg-surface` | `#131a16` | Card surface |
| `--bg-elevated` | `#1a2218` | Card hover state |
| `--border` | `rgba(163, 177, 138, 0.15)` | Card / divider borders |
| `--border-strong` | `rgba(163, 177, 138, 0.35)` | Outline buttons, hover |
| `--text-primary` | `#ffffff` | Headings, primary copy |
| `--text-secondary` | `#cbd0c8` | Body |
| `--text-muted` | `#9ca39a` | Captions, secondary descriptions |
| `--text-faint` | `#6b7268` | Footer, fine print |
| `--accent-from` | `#a3b18a` | Sage lighter — gradient start |
| `--accent-to` | `#588157` | Sage deeper — gradient end |
| `--accent-glow` | `rgba(132, 169, 140, 0.35)` | Blob glow |

Accent is always rendered as `linear-gradient(135deg, var(--accent-from), var(--accent-to))` — never used as a flat color. Glow blobs sit behind hero text and at one corner of every card.

### Typography

- **Body & headings:** Inter (Google Fonts), weights 400 / 500 / 600 / 700 / 800.
- **Labels & code:** JetBrains Mono, weight 500. Used for the small `▸ TAG` chips and `— SECTION LABEL` text above headings.

| Element | Spec |
|---|---|
| Hero name (`h1`) | Inter 800, `clamp(40px, 6vw, 72px)`, line-height 0.95, letter-spacing −0.035em |
| Section heading (`h2`) | Inter 700, 32 px, letter-spacing −0.02em |
| Card title (`h3`) | Inter 600, 22 px |
| Body (`p`) | Inter 400, 15 px, line-height 1.6 |
| Section label | JetBrains Mono 500, 11 px, letter-spacing 0.18em, uppercase |
| Tag / badge | JetBrains Mono 500, 10 px, letter-spacing 0.15em, uppercase |

Load fonts via `<link>` to `fonts.googleapis.com` with `display=swap`. Preconnect to `fonts.gstatic.com`.

### Components

- **Button (primary).** Gradient sage fill, text `#0c0f0d`, padding 10 × 18, border-radius 8, weight 600. Hover: scale 1.02 + glow shadow.
- **Button (outline).** Transparent, 1 px border at `--border-strong`, text `--text-secondary`, same padding/radius. Hover: border → solid sage, text → white.
- **Tag.** Sage at 15 % opacity background, text `--accent-from`, padding 4 × 10, border-radius 999. Prefixed with `▸`.
- **Section label.** Plain text, prefixed with `—` (em dash + space). Sits above section heading with 14 px gap.
- **Card.** `--bg-surface`, 1 px `--border`, border-radius 16, padding 28 × 24, glow blob in one corner. Hover: background → `--bg-elevated`, border → `--border-strong`, transform `translateY(-2px)`. Transition 180 ms.
- **Card image.** 16:9 aspect, border-radius 12, `object-fit: cover`. Replaces the current `<iframe>` hack — uses `<img>` against the same `play-lh.googleusercontent.com` URLs already in the HTML, since those resolve to static images.

---

## 3. Information architecture

### Pages preserved
Both `index.html` (open-source libraries) and `aboutme.html` (portfolio) stay as separate pages. The navbar moves between them.

### Content moves

| Element | Before | After |
|---|---|---|
| `img/poto.jpeg` (avatar) | Centered in hero, 100 px circle | New "Tentang Saya" section between hero and "Aplikasi Saya", left-aligned 96 px rounded-square |
| Footer | None | New: 3-column footer with social links + copyright |
| `exclusive-templates/` link | Linked from MDB freebie banner | Removed |
| MDB README and freebies | Present at repo root and in subdir | Removed |
| `<hr class="my-5">` dividers | Between every section | Replaced by `<section>` padding (96–120 px) + section label |
| Navbar social icons | 4 inline icons | Kept but smaller (16 px), with 1 px sage border on the GitHub button only |

### `aboutme.html` outline (in order)

1. **Hero.** Typographic. `— PORTFOLIO · 2026` label, "Halo, aku **Mafmudin**." (name styled with sage gradient text-fill), role line "Android Developer · Membuat aplikasi yang mudah digunakan.", two CTAs: primary "Lihat CV →" (links to `mycv.svg`), outline "GitHub ↗" (links to `https://github.com/Mafmudin`). Glow blob bottom-left.
2. **Tentang Saya.** Section label + small 96 px rounded-square photo on the left, 2–3 line bio on the right (Indonesian, written during implementation — see §6).
3. **Aplikasi Saya.** Section label `— APLIKASI` + `<h2>` "Aplikasi Saya" + 3-column responsive grid (1 col mobile, 2 col md, 3 col lg). Cards for GamaPro, EDayaK, Mobile City.
4. **Project Saya.** Same grid layout. Cards for Get Indonesia Customer, Desa Apps, and the third entry currently in the file (to be confirmed during implementation by reading the file).
5. **Footer.**

### `index.html` outline (in order)

1. **Hero (compact).** Smaller than `aboutme.html`. `— OPEN SOURCE` label, "Libraries Android.", short tagline.
2. **Grid 2-col.** Cards for ProDialog, MPopUp. Badge `▸ OPEN SOURCE`, title, description (Indonesian, copied from current file), button "Buka GitHub ↗".
3. **Footer** (identical to `aboutme.html`).

### Navbar

Fixed-top, full width, height 64 px, background `rgba(12, 15, 13, 0.8)` with `backdrop-filter: blur(12px)` and 1 px bottom border at `--border`.

- **Left:** "Mafmudin" wordmark, Inter 600, with sage dot before it (a 6 × 6 px gradient circle).
- **Center:** "Beranda" / "Portofolio" links. Active page underlined with sage gradient (2 px, 12 px wide, centered below text).
- **Right:** social icons (Facebook, Twitter, Instagram) at 16 px in `--text-muted`, with the GitHub link styled as a small pill button with 1 px sage border.

On mobile (< 768 px) the center links collapse into a single icon menu (CSS-only with a checkbox + label pattern — no JS).

### Footer

3-column on desktop, stacks on mobile.

- **Left:** "Mafmudin" wordmark + 1-line bio.
- **Center:** quick links (Beranda, Portofolio, CV).
- **Right:** social icons row.
- **Bottom row:** `© 2026 Muchamad Mafmudin · Made with care.`

---

## 4. Layout details

### Spacing scale (CSS custom properties)

```
--space-1: 4px   --space-2: 8px   --space-3: 12px
--space-4: 16px  --space-5: 24px  --space-6: 32px
--space-7: 48px  --space-8: 64px  --space-9: 96px
--space-10: 128px
```

Section vertical padding: `--space-9` mobile, `--space-10` desktop.

### Grid

CSS Grid, not Bootstrap. Cards section:

```css
.grid {
  display: grid;
  grid-template-columns: 1fr;
  gap: var(--space-5);
}
@media (min-width: 640px)  { .grid-2 { grid-template-columns: repeat(2, 1fr); } }
@media (min-width: 1024px) { .grid-3 { grid-template-columns: repeat(3, 1fr); } }
```

Container max-width 1100 px, centered, horizontal padding 24 px mobile / 48 px desktop.

### Glow blobs

Each card has one `::before` pseudo-element absolutely positioned, 160 × 160 px, `radial-gradient(circle, var(--accent-glow) 0%, transparent 70%)`, `filter: blur(20px)`. Positioned in the top-right corner (`top: -40px; right: -40px;`). Hero has a larger 280 × 280 px version positioned bottom-left.

Cards use `overflow: hidden` to clip the blob to the card boundary.

---

## 5. Tech approach

### Files removed

```
css/bootstrap.css                  css/mdb.lite.min.css.map
css/bootstrap.min.css              css/mdb.min.css
css/mdb.css                        css/mdb.min.css.map
css/mdb.lite.css                   js/bootstrap.js
css/mdb.lite.min.css               js/bootstrap.min.js
js/jquery.js                       js/popper.js
js/jquery.min.js                   js/popper.min.js
js/mdb.js                          js/mdb.lite.min.js.map
js/mdb.min.js                      js/mdb.min.js.map
scss/                              (entire directory)
src/                               (entire directory)
css/modules/                       (animations-extended, unused after redesign)
js/modules/                        (wow, treeview, scrolling-navbar, forms — all unused)
js/addons/                         (datatables, masonry, etc. — all unused)
exclusive-templates/               (MDB promo)
README.txt                         (MDB README, not user's)
License.pdf                        (audit during implementation — keep if user's own; remove if MDB's)
Useful_Resources.pdf               (MDB freebie — confirm and remove)
```

### Files added or modified

- **`css/style.css`** — currently 0 bytes. Becomes the single stylesheet, ~7 KB, structured as:
  1. `:root` design tokens (color, space, radius, transition)
  2. Reset + base elements (`html`, `body`, `h1-h3`, `p`, `a`, `img`)
  3. Layout utilities (`.container`, `.grid`, `.grid-2`, `.grid-3`, `.section`)
  4. Components (`.btn`, `.btn-primary`, `.btn-outline`, `.tag`, `.section-label`, `.card`, `.navbar`, `.footer`, `.glow`)
  5. Page-specific tweaks (`.hero`, `.hero-compact`, `.about-grid`, `.nav-mobile-toggle`)
  6. Media queries grouped at the bottom by breakpoint

- **`index.html`** — body rewritten. Head adds Google Fonts links, drops Bootstrap/MDB CSS and all JS scripts (no more `<script>` tags at all).

- **`aboutme.html`** — body rewritten with the 5-section outline above. Head changes identical to `index.html`.

- **`CLAUDE.md`** — updates:
  - Remove the "Versions to preserve / Don't upgrade MDB" section (no longer applies).
  - Remove the "scss/ and src/ vendor source" note.
  - Remove the "navbar duplication gotcha" note (single source of truth in the new navbar — no `navbar-toggler` duplicate).
  - Add: site is now plain HTML + a single `css/style.css`. Design tokens live in `:root`. No JS.
  - Add: typographic conventions (Inter + JetBrains Mono via Google Fonts, sage accent gradient).

- **`README.md` (new, optional)** — short replacement for `README.txt`. One paragraph. Not required for the redesign to ship; created only if it falls out naturally.

### Browser support

- CSS custom properties, CSS Grid, `clamp()`, `backdrop-filter` — all baseline since 2020.
- No polyfills needed. No JavaScript at all.
- `prefers-reduced-motion: reduce` disables the 180 ms card hover transitions.

### Deployment

No change. Push to `master` → GitHub Pages serves the new files. The cleanup commits will be large (deleting hundreds of vendor files) but the deployed footprint shrinks dramatically.

---

## 6. Content notes for implementation

- **Bio for "Tentang Saya".** Write a 2–3 line bio in Bahasa Indonesia during implementation. Tone: friendly-professional. Draft to propose: *"Android developer dengan fokus pada UI yang ringan dan mudah dipakai. Saat ini membangun aplikasi Android di Astra dan tetap aktif merilis library kecil di GitHub."* — confirm or rewrite with the user before committing.
- **Photo presentation.** `img/poto.jpeg` is rendered at 96 × 96 with `object-fit: cover` and `border-radius: 14px` (rounded square, not circle). Border 1 px at `--border-strong`.
- **App thumbnails.** Switch from `<iframe>` embeds to `<img src="...">` using the existing `play-lh.googleusercontent.com` URLs already in the HTML. Wrap each in a div with the 16:9 aspect and apply `object-fit: cover`. Add `alt` text per app.
- **CTA copy preserved.** "Unduh Gratis" → kept on app cards; "Buka GitHub" → kept on library cards. Add a small `↗` glyph to external links.
- **Active nav state.** "Beranda" is active on `index.html`; "Portofolio" is active on `aboutme.html`. Mark with `aria-current="page"` and the underline style.

---

## 7. Out of scope (explicitly deferred)

- Adding a blog or articles section.
- Adding analytics (Plausible, Google Analytics).
- Adding contact form (use mailto or social links).
- Adding RSS / sitemap.
- Open Graph / Twitter Card meta tags — could be added later but not required for the visual refresh.
- Translating the site to English — copy stays in Bahasa Indonesia.

---

## 8. Acceptance criteria

The redesign is done when:

1. `index.html` and `aboutme.html` load with no `<script>` tags and reference only `css/style.css` plus Google Fonts.
2. All vendor files listed in §5 are removed from the repo.
3. Both pages render correctly at 360 px, 768 px, and 1280 px widths.
4. Lighthouse Performance score ≥ 95 on mobile; Accessibility ≥ 95.
5. All current content (apps, libraries, projects, CV link, social links, photo) is present and links to the same destinations.
6. `CLAUDE.md` reflects the new architecture (no MDB references).
7. The site looks consistent with the approved mockups: sage accent gradient, dark surface, typographic hero, glow blobs, card hover lift.
