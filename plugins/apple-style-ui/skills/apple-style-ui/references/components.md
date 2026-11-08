# Component recipes

Every recipe here assumes the variables from `assets/tokens.css`. The numbers are deliberate: they are the measurements of Apple's own controls, adapted to the web. Copy them, then adapt the markup to the project's framework.

## Contents
1. Navigation bar (translucent, sticky)
2. Buttons
3. Inset-grouped list (iOS Settings style)
4. Cards and tiles
5. Segmented control
6. Toggle switch
7. Text field, search field, select
8. Sidebar (macOS / iPadOS)
9. Tab bar (mobile)
10. Sheet / modal, alert
11. Menu / popover
12. Toast / banner
13. Badges, chips, status dots
14. Tables and data
15. Empty state, loading
16. Marketing: hero, feature sections, tile grid, pricing, footer
17. Liquid Glass (floating controls)
18. Icons

---

## 1. Navigation bar

Use a translucent, sticky bar with no border at the top of the page. A hairline appears once the content scrolls under it.

```html
<header class="navbar" data-scrolled="false">
  <div class="navbar__inner">
    <a class="navbar__brand" href="/">Brand</a>
    <nav class="navbar__links">
      <a href="#">Overview</a><a href="#">Features</a><a href="#">Pricing</a>
    </nav>
    <a class="btn btn--filled btn--sm" href="#">Get started</a>
  </div>
</header>
```

```css
.navbar {
  position: sticky; top: 0; z-index: 50;
  background: var(--material-regular);
  -webkit-backdrop-filter: var(--material-blur);
  backdrop-filter: var(--material-blur);
  box-shadow: 0 1px 0 transparent;
  transition: box-shadow var(--dur-base) var(--ease-standard);
}
.navbar[data-scrolled="true"] { box-shadow: 0 0.5px 0 var(--separator); }
.navbar__inner {
  max-width: var(--content-wide); margin: 0 auto;
  height: 52px; padding: 0 var(--gutter);
  display: flex; align-items: center; gap: var(--space-6);
}
.navbar__brand { font-weight: 600; font-size: 1.1875rem; letter-spacing: -0.02em; color: var(--label); }
.navbar__links { display: flex; gap: var(--space-6); margin-left: auto; }
.navbar__links a { font-size: 0.875rem; color: var(--label-secondary); }   /* nav links are small: 12–14px */
.navbar__links a:hover { color: var(--label); text-decoration: none; }
@media (max-width: 734px) { .navbar__links { display: none; } }
```

```js
// Show the hairline only after scrolling
const nav = document.querySelector('.navbar');
addEventListener('scroll', () => nav.dataset.scrolled = scrollY > 4, { passive: true });
```

In an **app**, the bar is 44px (mobile) or 52px (desktop), with the title centered on iOS and left-aligned on macOS. A **large title** (34px bold, `--tracking-title`) sits below the bar and collapses into the bar's centered 17px semibold title as the user scrolls.

---

## 2. Buttons

There are four styles. Pick by importance, not by taste. Use **one filled button per view**.

| Style | Use for | Look |
|---|---|---|
| Filled | Primary action | accent background, white text |
| Tinted | Secondary but important | accent text on `--accent-tint` |
| Gray | Neutral or secondary | label text on `--fill-tertiary` |
| Plain | Tertiary, inline | accent text, no background (marketing uses "Learn more ›") |

```css
.btn {
  --h: 44px;
  display: inline-flex; align-items: center; justify-content: center; gap: 6px;
  height: var(--h); padding: 0 calc(var(--h) * 0.45);
  border: 0; border-radius: var(--radius-md);
  font: 600 var(--text-body)/1 var(--font-text); letter-spacing: var(--tracking-body);
  cursor: pointer; text-decoration: none; white-space: nowrap;
  transition: transform var(--dur-fast) var(--spring-snappy),
              background-color var(--dur-fast) var(--ease-standard),
              opacity var(--dur-fast) var(--ease-standard);
  -webkit-tap-highlight-color: transparent;
}
.btn:hover { text-decoration: none; }
.btn:active { transform: scale(0.97); }
.btn:disabled { opacity: 0.4; pointer-events: none; }

.btn--filled { background: var(--accent); color: var(--on-accent); }
.btn--filled:hover { background: color-mix(in srgb, var(--accent) 88%, black); }
.btn--tinted { background: var(--accent-tint); color: var(--accent); }
.btn--tinted:hover { background: color-mix(in srgb, var(--accent-tint) 70%, var(--accent) 12%); }
.btn--gray { background: var(--fill-tertiary); color: var(--label); }
.btn--gray:hover { background: var(--fill-secondary); }
.btn--plain { background: none; color: var(--accent-text); padding: 0 4px; }
.btn--plain:hover { text-decoration: underline; text-underline-offset: 0.18em; }
.btn--destructive { color: var(--red); }

.btn--sm { --h: 32px; font-size: 0.875rem; }
.btn--lg { --h: 52px; font-size: 1.0625rem; }
.btn--pill { border-radius: var(--radius-pill); }   /* marketing CTAs, apple.com style */
.btn--block { width: 100%; }
```

Rules:
- Marketing CTAs are **capsules**: filled `Buy` / `Get started`, plus a plain `Learn more ›` link next to it.
- Never put a gradient, glow or drop shadow on a button. Never use an outlined/bordered button as the main CTA.
- Icon-only buttons are a circle (`border-radius: 50%`) or a rounded square. The icon is 17–20px, the hit target 44px, and the background `--fill-tertiary` or none.

---

## 3. Inset-grouped list

This is the signature iOS Settings pattern. Use it for settings, account pages, forms and any list of options.

```html
<section class="group">
  <h3 class="group__header">Notifications</h3>
  <ul class="list">
    <li class="row">
      <span class="row__icon" style="--icon-bg: var(--red)"><!-- svg --></span>
      <span class="row__title">Allow notifications</span>
      <label class="switch"><input type="checkbox" checked><span></span></label>
    </li>
    <li class="row row--link">
      <span class="row__icon" style="--icon-bg: var(--blue)"><!-- svg --></span>
      <span class="row__title">Sounds</span>
      <span class="row__value">Chime</span>
      <svg class="row__chevron" viewBox="0 0 8 14" width="8" height="14"><path d="M1 1l6 6-6 6" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/></svg>
    </li>
  </ul>
  <p class="group__footer">You can change this any time in Settings.</p>
</section>
```

```css
.group { margin: 0 0 var(--space-8); }
.group__header {
  font: 400 var(--text-footnote)/1.3 var(--font-text);
  text-transform: uppercase; letter-spacing: 0.02em;
  color: var(--label-secondary); margin: 0 var(--space-4) var(--space-2);
}
.group__footer { font-size: var(--text-footnote); color: var(--label-secondary); margin: var(--space-2) var(--space-4) 0; }
.list {
  list-style: none; margin: 0; padding: 0;
  background: var(--bg-grouped-secondary);
  border-radius: var(--radius-md); overflow: hidden;
}
.row {
  position: relative; display: flex; align-items: center; gap: var(--space-3);
  min-height: var(--touch-min); padding: 0 var(--space-4);
  font-size: var(--text-body);
}
/* Inset separator: starts where the text starts, not at the edge */
.row + .row::before {
  content: ""; position: absolute; top: 0; right: 0;
  left: var(--sep-inset, var(--space-4)); height: 0.5px; background: var(--separator);
}
.list:has(.row__icon) { --sep-inset: 60px; }
.row__icon {
  flex: none; width: 29px; height: 29px; border-radius: 7px;
  display: grid; place-items: center; color: #fff; background: var(--icon-bg, var(--gray));
}
.row__icon svg { width: 18px; height: 18px; }
.row__title { flex: 1; min-width: 0; padding: 11px 0; }
.row__value { color: var(--label-secondary); }
.row__chevron { color: var(--label-tertiary); flex: none; }
.row--link { cursor: pointer; }
.row--link:active { background: var(--fill-quaternary); }
```

The page behind the list is `--bg-grouped` (#f2f2f7 light, black dark). On desktop, cap the list width (~680px) and center it.

---

## 4. Cards and tiles

```css
/* The page behind cards: */
.page { background: var(--bg-grouped); }          /* #f2f2f7 light · #000 dark */
.card {
  background: var(--bg-grouped-secondary);        /* #fff light · #1c1c1e dark */
  border-radius: var(--radius-lg);
  padding: var(--space-6);
}
/* Separation, pick ONE:
   a) card on a --bg-grouped page   → needs nothing more, in both modes
   b) card on white page           → box-shadow: var(--shadow-1) + border: 0.5px solid var(--separator)
   c) floating / hover             → box-shadow: var(--shadow-2) */
.card--interactive { transition: transform var(--dur-base) var(--spring-smooth), box-shadow var(--dur-base) var(--ease-standard); }
.card--interactive:hover { box-shadow: var(--shadow-2); }
.card--interactive:active { transform: scale(0.985); }

.card__eyebrow { font-size: var(--text-footnote); font-weight: 600; color: var(--label-secondary); }
.card__title { font: 600 var(--text-title3)/1.2 var(--font-display); letter-spacing: var(--tracking-title); margin: 4px 0 6px; }
.card__body { color: var(--label-secondary); font-size: var(--text-subheadline); }
```

**Stat / metric card (dashboards).** A small secondary label sits above a big number (28–34px, semibold, `font-variant-numeric: tabular-nums`). Under it goes a delta in green/red, small, with an arrow glyph. Do not color the whole card.

---

## 5. Segmented control

```html
<div class="segmented" role="tablist">
  <button role="tab" aria-selected="true">Day</button>
  <button role="tab" aria-selected="false">Week</button>
  <button role="tab" aria-selected="false">Month</button>
</div>
```

```css
.segmented {
  display: inline-grid; grid-auto-flow: column; grid-auto-columns: 1fr;
  padding: 2px; gap: 2px; border-radius: 9px; background: var(--fill-tertiary);
}
.segmented button {
  height: 28px; padding: 0 14px; border: 0; border-radius: 7px; /* 9 - 2 = 7: concentric */
  background: transparent; color: var(--label);
  font: 500 0.8125rem/1 var(--font-text); cursor: pointer;
  transition: background-color var(--dur-fast) var(--ease-standard);
}
.segmented button[aria-selected="true"] {
  background: var(--bg-elevated); box-shadow: var(--shadow-control); font-weight: 600;
}
:root[data-theme="dark"] .segmented button[aria-selected="true"] { background: #636366; }
@media (prefers-color-scheme: dark) { :root:not([data-theme="light"]) .segmented button[aria-selected="true"] { background: #636366; } }
```

For a smooth sliding thumb, animate one absolutely positioned indicator with `transform: translateX()` and `--spring-snappy`.

---

## 6. Toggle switch

```html
<label class="switch"><input type="checkbox" role="switch"><span></span></label>
```

```css
.switch { position: relative; display: inline-block; width: 51px; height: 31px; flex: none; }
.switch input { position: absolute; opacity: 0; inset: 0; margin: 0; cursor: pointer; z-index: 1; }
.switch span {
  position: absolute; inset: 0; border-radius: 999px; background: var(--fill);
  transition: background-color var(--dur-base) var(--ease-standard);
}
.switch span::after {
  content: ""; position: absolute; top: 2px; left: 2px;
  width: 27px; height: 27px; border-radius: 50%; background: #fff;
  box-shadow: 0 3px 8px rgb(0 0 0 / 0.15), 0 3px 1px rgb(0 0 0 / 0.06);
  transition: transform var(--dur-base) var(--spring-snappy), width var(--dur-fast) var(--ease-standard);
}
.switch input:checked + span { background: var(--green); }
.switch input:checked + span::after { transform: translateX(20px); }
.switch input:active + span::after { width: 33px; }               /* iOS stretch on press */
.switch input:checked:active + span::after { transform: translateX(14px); }
.switch input:focus-visible + span { box-shadow: var(--focus-ring); }
```

The "on" color is **green** by default (iOS). The accent color is fine if the brand prefers it.

---

## 7. Text field, search, select

```css
.field {
  width: 100%; height: 44px; padding: 0 var(--space-3);
  border: 0; border-radius: var(--radius-md);
  background: var(--fill-tertiary); color: var(--label);
  font: 400 var(--text-body)/1.2 var(--font-text); letter-spacing: var(--tracking-body);
  transition: box-shadow var(--dur-fast) var(--ease-standard), background-color var(--dur-fast);
}
.field::placeholder { color: var(--label-tertiary); }
.field:focus { outline: none; background: var(--bg-elevated); box-shadow: 0 0 0 1px var(--accent), var(--focus-ring); }
.field[aria-invalid="true"] { box-shadow: 0 0 0 1px var(--red), 0 0 0 4px rgb(255 59 48 / 0.2); }
textarea.field { height: auto; min-height: 96px; padding: var(--space-3); resize: vertical; }

.label { display: block; font-size: var(--text-subheadline); font-weight: 500; margin: 0 0 6px 2px; }
.help  { font-size: var(--text-footnote); color: var(--label-secondary); margin: 6px 0 0 2px; }

/* Search: pill-ish, magnifier inside, 36px tall */
.search { position: relative; }
.search .field { height: 36px; padding-left: 32px; border-radius: 10px; font-size: var(--text-body); }
.search svg { position: absolute; left: 10px; top: 50%; translate: 0 -50%; width: 15px; color: var(--label-secondary); }

/* Native select with custom chevron */
select.field {
  appearance: none; padding-right: 32px;
  background-image: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' width='12' height='8' viewBox='0 0 12 8'%3E%3Cpath d='M1 1.5l5 5 5-5' fill='none' stroke='%238e8e93' stroke-width='1.8' stroke-linecap='round' stroke-linejoin='round'/%3E%3C/svg%3E");
  background-repeat: no-repeat; background-position: right 12px center;
}
```

Forms: one column, with labels above fields, 16–20px between fields and a single filled button at the end. On mobile, put grouped forms inside an inset-grouped list (section 3), with the fields as rows and placeholders as labels.

---

## 8. Sidebar (macOS / iPadOS)

```css
.app { display: grid; grid-template-columns: 240px 1fr; min-height: 100dvh; }
.sidebar {
  background: var(--bg-secondary);   /* or var(--material-thin) + blur over a wallpaper */
  padding: var(--space-3) var(--space-2);
  border-right: 0.5px solid var(--separator);
}
.sidebar__section { font-size: 0.6875rem; font-weight: 600; color: var(--label-tertiary); padding: var(--space-4) var(--space-3) var(--space-1); }
.sidebar__item {
  display: flex; align-items: center; gap: 8px; height: 30px; padding: 0 var(--space-3);
  border-radius: var(--radius-sm); color: var(--label); font-size: 0.875rem;
}
.sidebar__item svg { width: 16px; height: 16px; color: var(--accent); }
.sidebar__item:hover { background: var(--fill-quaternary); text-decoration: none; }
.sidebar__item[aria-current="page"] { background: var(--fill-secondary); font-weight: 500; }
.sidebar__count { margin-left: auto; color: var(--label-secondary); font-size: 0.8125rem; font-variant-numeric: tabular-nums; }
@media (max-width: 900px) { .app { grid-template-columns: 1fr; } .sidebar { display: none; } }
```

Desktop app density is a little tighter than iOS: 13–14px text, 28–32px rows.

---

## 9. Tab bar (mobile)

```css
.tabbar {
  position: fixed; inset: auto 0 0 0; z-index: 50;
  display: grid; grid-auto-flow: column; grid-auto-columns: 1fr;
  padding: 6px 8px calc(6px + env(safe-area-inset-bottom));
  background: var(--material-regular);
  -webkit-backdrop-filter: var(--material-blur); backdrop-filter: var(--material-blur);
  box-shadow: 0 -0.5px 0 var(--separator);
}
.tabbar a {
  display: grid; justify-items: center; gap: 2px; padding-top: 2px;
  font-size: 0.625rem; font-weight: 500; color: var(--gray);
}
.tabbar svg { width: 24px; height: 24px; }
.tabbar a[aria-current="page"] { color: var(--accent); }
```

Use 3–5 tabs, each with an icon and a label. Never use a hamburger menu in an iOS-style app. For the iOS 26 floating glass tab bar, see section 17.

---

## 10. Sheet / modal, alert

```html
<dialog class="sheet">
  <div class="sheet__grabber"></div>
  <header class="sheet__header">
    <button class="btn btn--plain">Cancel</button>
    <h2>New Event</h2>
    <button class="btn btn--plain" style="font-weight:600">Add</button>
  </header>
  <div class="sheet__body">…</div>
</dialog>
```

```css
.sheet {
  border: 0; padding: 0; color: var(--label);
  width: min(560px, 100vw); max-height: min(88dvh, 820px);
  border-radius: var(--radius-xl); background: var(--bg-elevated);
  box-shadow: var(--shadow-3);
}
.sheet::backdrop { background: rgb(0 0 0 / 0.35); }
.sheet[open] { animation: sheet-in var(--dur-slow) var(--ease-sheet); }
@keyframes sheet-in { from { transform: translateY(24px) scale(0.98); opacity: 0; } }
.sheet__grabber { width: 36px; height: 5px; border-radius: 3px; background: var(--fill); margin: 6px auto 0; }
.sheet__header { display: grid; grid-template-columns: 1fr auto 1fr; align-items: center; height: 52px; padding: 0 var(--space-2); }
.sheet__header h2 { font-size: var(--text-headline); font-weight: 600; letter-spacing: var(--tracking-body); }
.sheet__header > :last-child { justify-self: end; }
.sheet__body { padding: var(--space-4) var(--space-5) var(--space-6); overflow: auto; }

/* Mobile: sheet docks to the bottom */
@media (max-width: 600px) {
  .sheet { margin: auto 0 0; width: 100vw; max-width: 100vw; border-radius: var(--radius-xl) var(--radius-xl) 0 0; }
  .sheet[open] { animation: sheet-up var(--dur-slow) var(--ease-sheet); }
  @keyframes sheet-up { from { transform: translateY(100%); } }
}
```

**Alert** (destructive confirmation): ~280px wide and centered on `--material-thick` + blur. It has a 17px semibold title, a 13px message, and full-width stacked or side-by-side plain buttons separated by hairlines. Make the destructive button red, and the default action semibold.

---

## 11. Menu / popover

```css
.menu {
  min-width: 200px; padding: 6px; border-radius: var(--radius-md);
  background: var(--material-thick);
  -webkit-backdrop-filter: var(--material-blur); backdrop-filter: var(--material-blur);
  box-shadow: var(--shadow-3), 0 0 0 0.5px var(--separator);
  transform-origin: top; animation: menu-in var(--dur-fast) var(--ease-out);
}
@keyframes menu-in { from { opacity: 0; transform: scale(0.96); } }
.menu__item { display: flex; align-items: center; gap: 10px; height: 32px; padding: 0 10px; border-radius: 6px; font-size: 0.875rem; }
.menu__item:hover { background: var(--accent); color: var(--on-accent); }  /* macOS highlight */
.menu__sep { height: 0.5px; margin: 5px 10px; background: var(--separator); }
```

---

## 12. Toast / banner

```css
.toast {
  position: fixed; left: 50%; top: 16px; translate: -50% 0; z-index: 100;
  display: flex; align-items: center; gap: 10px;
  padding: 10px 18px 10px 12px; border-radius: var(--radius-pill);
  background: var(--material-thick);
  -webkit-backdrop-filter: var(--material-blur); backdrop-filter: var(--material-blur);
  box-shadow: var(--shadow-2), 0 0 0 0.5px var(--separator);
  font-size: var(--text-subheadline); font-weight: 500;
  animation: toast-in var(--dur-slow) var(--spring-snappy);
}
@keyframes toast-in { from { opacity: 0; transform: translateY(-16px) scale(0.96); } }
```

Use it for short messages with an optional icon in a system color, like the Dynamic Island / HUD style.

---

## 13. Badges, chips, status dots

```css
.badge { display: inline-flex; align-items: center; gap: 4px; height: 22px; padding: 0 8px;
  border-radius: var(--radius-pill); font-size: 0.75rem; font-weight: 600;
  color: var(--c, var(--accent)); background: color-mix(in srgb, var(--c, var(--accent)) 14%, transparent); }
.dot { width: 8px; height: 8px; border-radius: 50%; background: var(--c, var(--green)); display: inline-block; }
.chip { height: 32px; padding: 0 14px; border-radius: var(--radius-pill); border: 0;
  background: var(--fill-tertiary); color: var(--label); font-size: 0.875rem; font-weight: 500; }
.chip[aria-pressed="true"] { background: var(--label); color: var(--bg); }
```

Usage: `<span class="badge" style="--c: var(--green)">Active</span>`. Use at most one badge per row, and never a badge above every headline.

---

## 14. Tables and data

```css
.table { width: 100%; border-collapse: collapse; font-size: 0.875rem; font-variant-numeric: tabular-nums; }
.table th { text-align: left; font-weight: 500; font-size: 0.75rem; color: var(--label-secondary);
  padding: 8px 12px; border-bottom: 0.5px solid var(--separator); }
.table td { padding: 12px; border-bottom: 0.5px solid var(--separator); }
.table tr:last-child td { border-bottom: 0; }
.table tbody tr:hover { background: var(--fill-quaternary); }
.table .num { text-align: right; }
```

Tables have no zebra stripes, no vertical lines and no heavy header background. Right-align numbers and use tabular figures.

**Charts:** draw thin lines (2px) with rounded caps and joins. Use the accent for the main series and gray or system colors for the rest. Gridlines are very faint (`--separator`), or show only a baseline. Label directly instead of using a legend where you can. Use no 3D, no gradients in bars, and at most a subtle area fill (accent at 10–15% fading to 0).

---

## 15. Empty state, loading

- **Empty state:** center it with a large SF-style line icon (48px, `--label-tertiary`), a 20px semibold title, one line of `--label-secondary` text and one tinted or filled button. No illustrations of people with laptops.
- **Loading:** a small spinner (activity indicator, 8–12 fading spokes, `--gray`) or skeletons that pulse gently between `--fill-quaternary` and `--fill-tertiary`. No shimmering rainbow.

```css
.skeleton { background: var(--fill-tertiary); border-radius: var(--radius-sm); animation: pulse 1.4s var(--ease-standard) infinite alternate; }
@keyframes pulse { to { background: var(--fill-quaternary); } }
```

---

## 16. Marketing pages (apple.com language)

**Hero**: centered, with a huge headline and very little else.

```html
<section class="hero">
  <p class="hero__eyebrow">New</p>
  <h1 class="hero__title">Focus. Finally.</h1>
  <p class="hero__lead">The calm planner that organizes your day before you do.</p>
  <div class="hero__ctas">
    <a class="btn btn--filled btn--pill" href="#">Download free</a>
    <a class="btn btn--plain" href="#">Learn more ›</a>
  </div>
  <img class="hero__media" src="screenshot.png" alt="">
</section>
```

```css
.hero { text-align: center; padding: var(--section-pad) var(--gutter) 0; overflow: hidden; }
.hero__eyebrow { color: var(--orange); font-weight: 600; font-size: var(--text-lead); margin: 0 0 8px; }
.hero__title { font-size: var(--text-hero); line-height: var(--leading-tight); letter-spacing: var(--tracking-hero); font-weight: 700; }
.hero__lead { font-size: var(--text-lead); line-height: 1.38; color: var(--label-secondary); max-width: 34ch; margin: 16px auto 0; }
.hero__ctas { display: flex; gap: var(--space-5); justify-content: center; align-items: center; margin-top: var(--space-8); flex-wrap: wrap; }
.hero__media { display: block; width: min(1000px, 100%); margin: var(--space-16) auto 0; border-radius: var(--radius-xl); box-shadow: var(--shadow-3); }
```

**Section rhythm**
- Alternate `--bg` and `--bg-secondary` (or a full black section in light mode) to divide the story. Do not use lines.
- Each section is a headline (`--text-display`, 600–700) plus one or two lines of lead, then one large visual or a tile grid.
- The eyebrow is a short colored word above the headline. Use it rarely, at most once or twice per page.
- **Stat highlight**: a giant number in the accent or a single gradient-free color (e.g. "2×", "18 hrs") with a small caption below.

**Tile grid (bento)**: tiles in `--bg-secondary` (or white on gray) with radius 28px, padding 32–40px, a title at the top-left (24–28px semibold) and a visual bleeding off the bottom or edge. Use mixed spans (2:1, 1:1) with a 16–20px gap. No borders, no shadows on tiles in light mode.

```css
.bento { display: grid; grid-template-columns: repeat(6, 1fr); gap: 20px; max-width: var(--content-wide); margin: 0 auto; padding: 0 var(--gutter); }
.tile { grid-column: span 3; background: var(--bg-secondary); border-radius: var(--radius-xl); padding: 36px; min-height: 360px; overflow: hidden; position: relative; }
.tile--wide { grid-column: span 6; } .tile--third { grid-column: span 2; }
.tile h3 { font-size: clamp(1.5rem, 1.2rem + 1vw, 1.75rem); font-weight: 600; letter-spacing: -0.02em; }
.tile p { color: var(--label-secondary); margin-top: 8px; }
@media (max-width: 734px) { .bento { grid-template-columns: 1fr; } .tile, .tile--wide, .tile--third { grid-column: auto; } }
```

**Pricing**: 2–3 plans, side by side, as cards on `--bg-secondary`. The plan name is 20px semibold, the price huge (48px, 600, tight tracking) with "/mo" small and secondary. Features are a plain list with small check glyphs in the accent color, and one filled pill button per card. Highlight the recommended plan with a thin accent ring (`box-shadow: 0 0 0 2px var(--accent)`) and a small "Most popular" badge. Do not scale the card up or give it a gradient.

**Footer**: 12px text in `--label-secondary` on `--bg-secondary`, with columns of links (heading 12px semibold `--label`) and a hairline on top. Make it dense and quiet.

---

## 17. Liquid Glass (floating controls)

Apple's 2025+ material is for controls that **float over content**: tab bars, toolbars, floating action buttons, media controls and segmented pickers over imagery. The control shows the content behind it with heavy blur and saturation, plus a bright specular rim. Use it sparingly. Never use it for cards, text blocks, or the page background.

```css
.glass {
  background: color-mix(in srgb, var(--bg) 42%, transparent);
  -webkit-backdrop-filter: blur(24px) saturate(190%) brightness(1.05);
  backdrop-filter: blur(24px) saturate(190%) brightness(1.05);
  border-radius: var(--radius-pill);
  box-shadow:
    inset 0 1px 0 var(--glass-edge),          /* top specular highlight */
    inset 0 -1px 0 rgb(255 255 255 / 0.08),
    inset 0 0 0 0.5px rgb(255 255 255 / 0.25),
    0 8px 32px rgb(0 0 0 / 0.12);
}
/* Floating tab bar (iOS 26 style) */
.glass-tabbar {
  position: fixed; left: 50%; bottom: calc(16px + env(safe-area-inset-bottom)); translate: -50% 0;
  display: flex; gap: 4px; padding: 6px;
}
.glass-tabbar a { display: grid; justify-items: center; gap: 1px; padding: 6px 16px; border-radius: var(--radius-pill); font-size: 0.625rem; font-weight: 600; color: var(--label); }
.glass-tabbar a[aria-current="page"] { background: var(--fill-tertiary); color: var(--accent); }
@media (prefers-reduced-transparency: reduce) { .glass { background: var(--bg-elevated); backdrop-filter: none; } }
```

Glass only reads as glass when there is varied content behind it, such as photos, color or scrolling content. Over a flat white page it just looks gray, so use the plain material there instead.

---

## 18. Icons

- **Web:** use Lucide (`stroke-width: 1.75`), Phosphor (Regular) or Heroicons (outline), with `stroke-linecap: round` and `stroke-linejoin: round`. Size icons to the text: 17px text → 20px icon; 13px → 16px.
- **Apple platforms:** use SF Symbols, which match the font's weight and scale automatically (`Image(systemName:)` with `.font(.body)` / `.imageScale`).
- Color icons with `currentColor` so they inherit `--label`, `--label-secondary` or `--accent`.
- For simple inline icons (chevron, check, plus, xmark), hand-drawn inline SVG with round caps is fine and avoids a dependency.
