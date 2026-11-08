# Final quality pass

Do this before handing the work back. Fix every "no" you can. If you can render the UI, check it visually. Many of these problems are invisible in code.

## 1. Remove the "generic / AI-made" tells

Search the code and the screen for these and delete or replace them:

- [ ] Purple-to-blue (or any) **gradient** backgrounds, gradient buttons, **gradient text**
- [ ] **Glows**, colored shadows (`shadow-blue-500/50`), neon borders, `ring` + `border` + `shadow` stacked on one element
- [ ] **Emoji** used as icons or bullet points (🚀 ✨ ⚡)
- [ ] Every feature icon sitting in a **colored rounded square**, in six different colors
- [ ] A pill badge above every headline ("✨ New: AI-powered")
- [ ] Filler copy: "Supercharge", "Unlock", "Revolutionize", "Seamless", "Cutting-edge", "Elevate your workflow"
- [ ] More than **two font weights** in one view; any `font-black` / 800–900 weights
- [ ] Tiny gray 14px body text everywhere; text in Tailwind's blue-tinted grays (`slate`, `gray-500`) instead of `--label-secondary`
- [ ] Cards with `border` **and** `shadow` **and** a different background
- [ ] Mixed corner radii that are not concentric (a 16px card holding 12px buttons with 4px padding)
- [ ] UPPERCASE letter-spaced labels everywhere (OK only for small inset-list section headers)
- [ ] Glassmorphism used on content cards or everything at once
- [ ] Bouncy hover lifts with big shadows on every card (`hover:-translate-y-2 hover:shadow-2xl`)
- [ ] Multiple accent colors competing (blue buttons, purple links, green badges, orange icons)
- [ ] Outlined/ghost button as the main call to action
- [ ] Hamburger menu inside an iOS-style app (use a tab bar or sidebar)

## 2. Confirm the Apple details

**Type**
- [ ] The system font stack is applied to `body` (`--font-text`) and to inputs/buttons (they don't inherit by default: `font: inherit`)
- [ ] Body text is 17px, secondary 15px and footnote 13px, with line-height ≈ 1.47 on body copy
- [ ] Headlines are 600/700 with negative tracking and `text-wrap: balance`, and the size jump between levels is decisive
- [ ] Numbers in tables, stats and prices use `font-variant-numeric: tabular-nums`

**Color**
- [ ] Every color comes from the tokens, and there is exactly **one** accent color
- [ ] Secondary text uses `--label-secondary`, not a fixed gray
- [ ] Link-colored text on white uses `--accent-text` (contrast ≥ 4.5:1)

**Layout and shape**
- [ ] Spacing values are on the 4pt grid, and sections breathe (marketing ≥ 80px vertical padding)
- [ ] Inner radius = outer radius − padding (concentric)
- [ ] Separators are hairlines (0.5px / `--separator`), and list separators are inset to the text start
- [ ] Hit targets are ≥ 44px, and buttons share consistent heights per size

**Depth and motion**
- [ ] The nav/tool/tab bars are translucent (`--material-*` + blur), with the hairline shown only on scroll
- [ ] Shadows appear only on floating elements and are soft and neutral
- [ ] Pressables scale to 0.97 on `:active`, transitions run 160–420ms with spring/ease-out curves, and hover states are subtle
- [ ] `prefers-reduced-motion` and `prefers-reduced-transparency` are honored

**Dark mode**
- [ ] The UI looks designed in dark mode, not inverted: the base is black, elevated surfaces are lighter grays, accents are brighter variants
- [ ] In dark mode every card, list, sidebar and toolbar is still distinguishable from the page. Check that you used the `--bg-grouped` / `--bg-grouped-secondary` pair. Cards on `--bg-secondary` disappear in dark.
- [ ] There are no hard-coded `#fff` / `#000` / `white` / `black` in components, except text on accent buttons and the switch knob
- [ ] Images and screenshots look right on black (no white boxes around transparent PNGs)
- [ ] `color-scheme: light dark` is set so form controls and scrollbars match

**Responsive**
- [ ] At 375px wide there is no horizontal scroll, the side margins are 16–20px and the hero headline still fits (fluid `clamp()` sizes)
- [ ] The desktop navigation collapses gracefully, and app UIs switch from a sidebar to a tab bar or stacked navigation

**Accessibility**
- [ ] Text contrast is ≥ 4.5:1 (3:1 for ≥ 24px text). Check the secondary text on `--bg-secondary`
- [ ] A `:focus-visible` ring is present and visible on every control
- [ ] Switches use `role="switch"`, segmented controls use `role="tablist"`, dialogs use `<dialog>` or `aria-modal`, and icon-only buttons have `aria-label`

## 3. The squint test

Squint at the screen (or blur a screenshot):
- Can you tell the **one** most important thing and the **one** primary action within a second?
- Is there a calm, even rhythm of space, or are there cramped and empty patches?
- Does anything shout that does not deserve to (a colored badge, an icon, a border)? Quiet it.

If a screen still feels "busy", remove one more thing.
