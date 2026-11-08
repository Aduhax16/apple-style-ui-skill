---
name: apple-style-ui
description: Design and restyle interfaces so they look and feel like Apple made them — clean hierarchy, SF-style typography, system colors, translucent materials, soft concentric corners, spring motion, perfect light/dark mode. Use this skill whenever the user builds or touches any UI (website, landing page, web app, dashboard, settings screen, mobile app, desktop app, component, form, modal) and wants it to look clean, premium, minimal, polished, modern, "like Apple", iOS, macOS, Cupertino, HIG or "less AI-generated", and also when they ask to restyle, redesign, modernize, clean up or "make prettier" an existing UI. Works for HTML/CSS, Tailwind, React, Vue, Svelte, Next.js, SwiftUI, UIKit, Flutter and React Native.
license: MIT
---

# Apple-style UI

Make any interface feel as if Apple's design team shipped it. The look is not "white with rounded corners". It comes from a small set of disciplined decisions, applied everywhere, with nothing extra:

- **Content first, chrome second.** The UI steps back so the content can lead. Controls are quiet until they are needed.
- **Hierarchy through type, not decoration.** Size, weight and spacing do the work that other designs hand to borders, colors, badges and shadows.
- **One accent color, used sparingly.** Everything else is a neutral system color.
- **Depth through layers and materials.** Translucent bars, elevated sheets and layered backgrounds replace heavy shadows.
- **Precision.** Spacing sits on a 4pt grid, corners are concentric, text is optically tracked, motion is physical and short.

If you remember one rule: **remove before you add**. When something looks "off", the fix is almost always to delete a border, a shadow, a color, a gradient or a font weight. It is rarely to add one.

## Workflow

### 1. Read the situation

Answer these before writing code. They change the right answer.

- **New UI or restyle?** For a restyle, read the existing code first. Keep its structure, logic, routes, data and copy. Change the presentation layer only. See "Restyling an existing UI" below.
- **Stack.** For web stacks, the tokens below apply directly. For **native Apple platforms** (SwiftUI/UIKit/AppKit), use the real system components, semantic colors and text styles. Do not recreate them by hand; read `references/frameworks.md`. Flutter and React Native are also covered there.
- **Surface type.** This picks the type scale and layout:
  - **Product marketing** (landing page, homepage, launch page) follows apple.com. It uses huge confident headlines, big imagery or product shots, alternating full-bleed sections, centered narrative, generous vertical rhythm and capsule CTAs.
  - **App UI** (dashboard, settings, tool, admin, mobile screen) follows iOS/macOS. It uses the 17px text-style scale, sidebars or tab bars, inset-grouped lists, translucent toolbars, dense but calm layouts, and left-aligned content.
  - Many products have both. Use the marketing scale for the marketing pages and the app scale inside the app.

### 2. Install the foundation (tokens)

Start every web project from `assets/tokens.css`. It holds the system colors (light and dark), text styles, spacing, radii, materials, shadows and motion as CSS variables. Copy it into the project's global CSS (or map it into `tailwind.config` / theme files using `references/frameworks.md`). Then build every component **only** on those variables. A hard-coded hex or a random `#e5e7eb` gray is how the result drifts away from Apple.

For a brand, change only `--accent` (and `--accent-text` / `--accent-tint`). Apple products each own one color. Keep everything else neutral.

### 3. Build with the component recipes

`references/components.md` has ready-to-use HTML/CSS for the pieces that carry most of the "Apple feel". These are the translucent navigation bar, buttons (filled, tinted, gray, plain), inset-grouped lists, cards and tiles, segmented control, toggle switch, text fields, search, sidebar, tab bar, sheet/modal, alert, menu, toast, empty state and the marketing hero. Read the sections you need before writing those components. Their exact numbers (heights, paddings, radii, weights) are the point.

### 4. Review against the checklist

Before you finish, go through `references/checklist.md`. It lists the tells that make a UI look generic or AI-generated, and the details that make it look Apple-made. If you can render the page (a browser, a preview server or screenshots), look at it in **both light and dark mode** and at **phone width**, and fix what you see. Polish lives in the details you only notice when you look.

## Core rules (the 20% that gives 80% of the look)

**Typography**
- Use the system font stack (`--font-text` / `--font-display`). It is SF Pro on Apple devices and Inter/system elsewhere. Do not download or self-host SF Pro, because its license forbids it on the web. Inter is the best fallback if a webfont is needed.
- Body text is **17px** (apps and marketing) with line-height ~1.47. Secondary text is 15px and footnotes 13px. Avoid 14px-everywhere UIs: they read as generic SaaS.
- Headlines are **semibold (600)** or bold (700), never black (900). Tighten tracking as size grows (`-0.02em` to `-0.025em` on display sizes). Use `text-wrap: balance` on headings.
- Use at most **two weights** in any one view (regular + semibold is the classic pair) and about three sizes per screen region. Make hierarchy jumps decisive: an 80px headline above a 21px subhead, not 32px above 24px.
- Write in sentence case and keep it short. Avoid ALL-CAPS labels except tiny section headers in inset-grouped lists.

**Color**
- Text uses `--label` (near black / near white). Secondary text uses `--label-secondary`, which is a translucent gray, so it adapts to any background.
- Backgrounds are white/black plus the soft gray `--bg-secondary` (#f5f5f7 / #1c1c1e). Alternate them to separate sections instead of drawing lines.
- **Surfaces come in pairs. Use the right pair, or dark mode breaks.** For a page that *holds cards or lists* (dashboards, settings, app screens), the page is `--bg-grouped` and the cards are `--bg-grouped-secondary`. That gives #f2f2f7 → white in light and black → #1c1c1e in dark. Do not put `--bg-elevated` cards on a `--bg-secondary` page: both are #1c1c1e in dark mode, so the cards vanish. Inside a card, a nested well uses `--fill-quaternary`.
- Reserve the accent for **interactive** things and the one thing that matters most. If the accent appears on more than ~10% of a screen, it stops working.
- Status colors are the system ones (green, orange, red, yellow) at full strength for icons and dots. For badges, use the color as text on a 12–15% tint background.
- **No** decorative gradients on UI chrome, **no** gradient text, **no** neon glows, **no** colored shadows. Apple uses gradients only inside imagery or product art.
- Dark mode is a first-class design, not an inversion. The base is pure black, and elevated surfaces get *lighter* (#1c1c1e → #2c2c2e) instead of getting shadows. Accents switch to their brighter dark variants (all handled by the tokens).

**Space and layout**
- Use a 4pt grid. Typical values are 8 / 12 / 16 / 20 / 24 / 32 and then large jumps (64, 80, 120+) between marketing sections.
- Be generous with whitespace and stingy with containers. Not every group needs a card. Grouping by proximity and alignment is cleaner than boxing.
- Use a reading width of ~980px for marketing copy. On mobile, side margins are 16–20px.
- Hit targets are at least 44×44px.

**Shape**
- Corners are continuous and **concentric**: an inner element's radius = outer radius − padding. A 20px card with 8px padding holds 12px buttons. Mismatched radii are the most common reason a UI feels "almost right".
- Use capsules (`--radius-pill`) for marketing CTAs, chips and segmented controls, 10–12px for app buttons and fields, and 18–28px for cards and tiles.
- Use **either** a hairline border **or** a soft shadow **or** a background shift to separate a surface. Never use all three. In light mode, a white card on `--bg-secondary` usually needs nothing else.

**Depth and materials**
- Navigation bars, toolbars, tab bars, menus and sheets are translucent: `background: var(--material-regular); backdrop-filter: var(--material-blur);`. Add a hairline separator only once content scrolls beneath.
- **Liquid Glass** (iOS 26 / macOS Tahoe era) is the newest layer. It is a floating, highly translucent control layer with a light rim and soft lensing. Use it only for controls that float above content (tab bar, toolbar, floating buttons, player controls), never for content cards or whole pages. `references/components.md` has a recipe.
- Shadows are soft, neutral and layered (`--shadow-1/2/3`), and only on things that actually float (popovers, sheets, dragged items, the selected segment).

**Motion**
- Motion explains a change: where something came from and where it went. Durations run 160–420ms. Use the spring curves (`--spring-smooth`, `--spring-snappy`) or `--ease-out`.
- For press feedback, scale to `0.97` on `:active` with a quick spring back. Hover is subtle (a background fill or small opacity change), not a lift-and-glow.
- Respect `prefers-reduced-motion` and `prefers-reduced-transparency` (the tokens file does).

**Icons and imagery**
- Use thin-to-regular line icons with rounded caps that match the text weight, sized relative to the text (≈ 1.1–1.2× the cap height of adjacent text). On the web, use Lucide, Phosphor or Heroicons outline at stroke 1.75–2. SF Symbols may only be used in apps for Apple platforms, so use them in SwiftUI/UIKit, not on websites.
- No emoji as UI icons. No icons inside colored rounded squares on every feature. If a feature grid needs icons, keep them in the accent or label color, unboxed. (The colorful rounded-square glyph *is* correct for **settings rows**, as in iOS Settings.)
- Big, real imagery, screenshots or product renders beat illustrations. Present screenshots inside a device frame or a softly rounded container with a subtle shadow.

**Copy**
- Use short, confident, specific headlines in Apple's rhythm: a two-to-five-word headline, then one sentence of support. Cut filler ("Supercharge your workflow with our cutting-edge platform").
- Button labels are verbs ("Get started", "Buy", "Continue", "Learn more ›").

## Restyling an existing UI

The user wants the *same* product, made beautiful. Work like this:

1. **Inventory.** Skim the styles and components. Note the stack, where colors and fonts are defined (CSS variables, Tailwind config, theme object, component library), and which components exist.
2. **Replace at the source.** Swap the theme layer first: tokens, Tailwind theme, CSS variables or MUI/shadcn/Chakra theme (see `references/frameworks.md`). One change at the root restyles hundreds of components consistently.
3. **Then fix components.** Remove the leftovers that fight the new style: gradients, colored shadows, heavy borders, 14px text, mixed radii, icon-in-colored-square grids, uppercase labels and extra font weights. Rebuild key components with the recipes.
4. **Don't break behavior.** Keep the DOM hooks that scripts and tests rely on (ids, `data-*`, handler names, form field names), the routes, accessibility attributes and the copy unless asked. If you must restructure markup, keep the existing class names alongside the new ones, or update every selector that uses them.
5. **Add dark mode** if it is missing. The tokens make it nearly free. If the project has a theme toggle, wire it to `data-theme`.
6. **Show the result.** Summarize what changed. If you can, show before/after screenshots.

## Native and cross-platform

- **SwiftUI / UIKit / AppKit:** the most Apple-like code is the most *native* code. Use `NavigationStack`, `List` with `.insetGrouped`, `Form`, `.toolbar`, `TabView`, sheets with detents, SF Symbols, `.font(.body)`-style Dynamic Type, semantic colors (`.primary`, `.secondary`, `Color(.systemGroupedBackground)`) and system materials (`.regularMaterial`, `.glassEffect()` on iOS 26+). Don't hard-code sizes or colors that the system already provides.
- **Flutter:** prefer the Cupertino widgets when targeting iOS, and otherwise apply the token values in a `ThemeData`.
- **React Native:** use `PlatformColor` and native navigation (large titles, blur headers).

Details, code and mappings are in `references/frameworks.md`.

## Reference files

| File | Read when |
|---|---|
| `assets/tokens.css` | Always, for web. Copy it in; build only on its variables. |
| `references/components.md` | Building or restyling any component: nav bar, buttons, lists, cards, forms, controls, sheets, hero, Liquid Glass. |
| `references/frameworks.md` | Tailwind, shadcn/ui, MUI, Chakra, Vue/Svelte, SwiftUI, UIKit, Flutter, React Native. |
| `references/checklist.md` | Before you finish: the final quality pass, light/dark and mobile checks, and the "looks AI-generated" anti-patterns. |
