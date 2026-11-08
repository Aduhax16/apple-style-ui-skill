# Framework mappings

Use the same design in every stack. Map the tokens once at the theme layer, then build components on top of them.

## Contents
- Tailwind CSS (v3 config and v4 `@theme`)
- shadcn/ui
- MUI, Chakra, Mantine, Bootstrap
- Vue, Svelte, Angular, plain CSS-in-JS
- SwiftUI
- UIKit / AppKit
- Flutter
- React Native / Expo

---

## Tailwind CSS

Keep `tokens.css` as the source of truth (import it in your global CSS), then point Tailwind at the variables. Dark mode then works automatically, because the variables change underneath.

**Tailwind v4** (`app.css`):

```css
@import "tailwindcss";
@import "./tokens.css";

@custom-variant dark (&:where([data-theme=dark], [data-theme=dark] *));

@theme inline {
  --font-sans: var(--font-text);
  --font-display: var(--font-display);
  --font-mono: var(--font-mono);

  --color-accent: var(--accent);
  --color-accent-tint: var(--accent-tint);
  --color-label: var(--label);
  --color-label-2: var(--label-secondary);
  --color-label-3: var(--label-tertiary);
  --color-bg: var(--bg);
  --color-bg-2: var(--bg-secondary);
  --color-bg-grouped: var(--bg-grouped);
  --color-surface: var(--bg-elevated);
  --color-fill: var(--fill);
  --color-fill-2: var(--fill-secondary);
  --color-fill-3: var(--fill-tertiary);
  --color-fill-4: var(--fill-quaternary);
  --color-separator: var(--separator);
  --color-sys-blue: var(--blue);   --color-sys-green: var(--green);
  --color-sys-red: var(--red);     --color-sys-orange: var(--orange);
  --color-sys-yellow: var(--yellow); --color-sys-purple: var(--purple);
  --color-sys-pink: var(--pink);   --color-sys-teal: var(--teal);
  --color-sys-indigo: var(--indigo); --color-sys-gray: var(--gray);

  --radius-sm: var(--radius-sm);
  --radius-md: var(--radius-md);
  --radius-lg: var(--radius-lg);
  --radius-xl: var(--radius-xl);

  --shadow-1: var(--shadow-1);
  --shadow-2: var(--shadow-2);
  --shadow-3: var(--shadow-3);

  --text-body: var(--text-body);
  --text-callout: var(--text-callout);
  --text-subhead: var(--text-subheadline);
  --text-footnote: var(--text-footnote);
  --text-caption: var(--text-caption1);
  --text-title3: var(--text-title3);
  --text-title2: var(--text-title2);
  --text-title1: var(--text-title1);
  --text-large-title: var(--text-large-title);
  --text-hero: var(--text-hero);
  --text-display: var(--text-display);

  --ease-spring: var(--spring-snappy);
  --ease-apple: var(--ease-out);
}
```

**Tailwind v3** (`tailwind.config.js`): use the same mapping inside `theme.extend`, as `colors: { accent: 'var(--accent)', label: { DEFAULT: 'var(--label)', 2: 'var(--label-secondary)' }, … }`, `borderRadius`, `boxShadow`, `fontFamily: { sans: ['var(--font-text)'] }` and `darkMode: ['class', '[data-theme="dark"]']`.

Class habits that keep it Apple-like:
- Use `text-label` / `text-label-2`, not `text-gray-500` / `text-slate-600`. Tailwind's grays are blue-tinted and slightly off.
- Use `bg-bg-2` sections and `bg-surface rounded-lg` cards, with no `border border-gray-200 shadow-sm` combination.
- Use `tracking-tight` (or `[letter-spacing:-0.02em]`) and `font-semibold` on headings. Avoid `font-extrabold` / `font-black`.
- Use `active:scale-[0.97] transition-transform ease-spring` on pressables.
- Use `backdrop-blur-xl backdrop-saturate-[1.8] bg-[var(--material-regular)]` for bars.
- Remove `bg-gradient-to-r from-purple-500 to-blue-500`, `shadow-xl shadow-blue-500/50`, `ring-1 ring-white/10` stacking and similar leftovers.

## shadcn/ui

shadcn reads HSL/OKLCH variables from `globals.css`. Point them at the tokens (keep the names shadcn expects):

```css
:root {
  --background: var(--bg);
  --foreground: var(--label);
  --card: var(--bg-elevated);
  --card-foreground: var(--label);
  --popover: var(--bg-elevated);
  --popover-foreground: var(--label);
  --primary: var(--accent);
  --primary-foreground: #fff;
  --secondary: var(--fill-tertiary);
  --secondary-foreground: var(--label);
  --muted: var(--bg-secondary);
  --muted-foreground: var(--label-secondary);
  --accent: var(--fill-quaternary);          /* shadcn "accent" = hover bg, not brand */
  --accent-foreground: var(--label);
  --destructive: var(--red);
  --border: var(--separator);
  --input: var(--fill-tertiary);
  --ring: var(--blue);
  --radius: 0.75rem;
}
```

Note the name clash: shadcn's `--accent` is a hover background. Load `tokens.css` first and rename the brand variable (for example `--brand: #007aff`) so that shadcn's `--accent` does not overwrite it. Then adjust components: `Button` default height 44 (`h-11`), `rounded-xl`, `font-semibold`; `Input` with `bg-input border-0`; `Card` without `border` and with `shadow-1` or nothing; `Switch` resized to 51×31 with a green checked state.

## MUI, Chakra, Mantine, Bootstrap

Override the theme, not individual components:
- **MUI** `createTheme`: set `palette.primary.main = '#007aff'`, `background.default = '#f5f5f7'`, `background.paper = '#fff'`, `text.primary = '#1d1d1f'`, `text.secondary = 'rgba(60,60,67,.72)'` and `shape.borderRadius = 12`. In `typography`, use the SF stack, body1 17px, h1–h4 weight 600 with negative letterSpacing. In `components`, set `MuiButton` `disableElevation`, `textTransform: 'none'`, `fontWeight: 600`; `MuiPaper` `elevation={0}` and soft shadows; `MuiAppBar` translucent with backdrop-filter. Add a `palette.mode='dark'` variant using the dark tokens.
- **Chakra / Mantine**: use the same values in `extendTheme` / `createTheme`, and turn off uppercase button text and default heavy shadows.
- **Bootstrap**: override the Sass variables (`$primary`, `$border-radius`, `$font-family-sans-serif`, `$btn-font-weight: 600`, `$card-border-width: 0`, `$box-shadow`), or the CSS variables `--bs-*` → tokens.

## Vue, Svelte, Angular, CSS-in-JS

Import `tokens.css` globally (`main.ts` / `app.css` / `styles.css`) and use `var(--…)` in component styles. In styled-components or Emotion, use the variables directly (`color: var(--label)`) instead of copying hex values into a JS theme, so dark mode keeps working through CSS.

---

## SwiftUI

The most Apple-like SwiftUI is the most native SwiftUI. Use system components and semantic styles. Don't hard-code sizes, hex colors or custom fonts unless the brand requires them.

```swift
struct SettingsView: View {
    @State private var notifications = true
    var body: some View {
        NavigationStack {
            List {
                Section {
                    Toggle(isOn: $notifications) {
                        Label("Notifications", systemImage: "bell.badge.fill")
                    }
                    NavigationLink { SoundsView() } label: {
                        LabeledContent("Sounds", value: "Chime")
                    }
                } header: { Text("Alerts") } footer: { Text("You can change this anytime.") }
            }
            .listStyle(.insetGrouped)
            .navigationTitle("Settings")            // large title collapses automatically
            .toolbar { Button("Done") {} .fontWeight(.semibold) }
        }
        .tint(.blue)                                // the single accent
    }
}
```

- **Type:** `.font(.largeTitle / .title / .headline / .body / .subheadline / .footnote / .caption)` supports Dynamic Type for free. Use `.fontWeight(.semibold)` for emphasis and `.fontDesign(.rounded)` for friendly numerals.
- **Color:** `.primary`, `.secondary`, `.tint`, `Color(.systemBackground)`, `Color(.secondarySystemGroupedBackground)`, `.foregroundStyle(.secondary)`. Asset-catalog colors need Any/Dark variants.
- **Materials:** `.background(.regularMaterial)` / `.ultraThinMaterial`. On iOS 26+ / macOS 26+, use `.glassEffect()` (and `GlassEffectContainer` for groups) on floating controls only; the system bars adopt Liquid Glass automatically. Guard with `if #available(iOS 26, *)`.
- **Shape:** `RoundedRectangle(cornerRadius: 20, style: .continuous)`. Always use `.continuous`. For concentric corners, use `ContainerRelativeShape()` / `.containerShape(...)`.
- **Buttons:** `.buttonStyle(.borderedProminent)` (filled), `.bordered` (tinted/gray), `.borderless` (plain), plus `.controlSize(.large)` and `.buttonBorderShape(.capsule)`.
- **Motion:** `withAnimation(.snappy)`, `.smooth`, `.bouncy`, `.spring(duration: 0.35, bounce: 0.15)`. Use `.contentTransition(.numericText())` for numbers, `.symbolEffect(.bounce)` for icons and `.sensoryFeedback(.success, trigger:)` for haptics.
- **Layout:** `.padding()` (16pt default), `Form`, `LabeledContent`, `ContentUnavailableView` for empty states, and `.sheet` with `.presentationDetents([.medium, .large])`.
- **Icons:** SF Symbols via `Label` / `Image(systemName:)`. Use `.symbolRenderingMode(.hierarchical)` for depth.

## UIKit / AppKit

- UIKit: `UICollectionViewCompositionalLayout.list(using: .init(appearance: .insetGrouped))`, `UIListContentConfiguration`, `UIButton.Configuration.filled()/.tinted()/.gray()/.plain()`, `navigationBar.prefersLargeTitles = true`, `UIFont.preferredFont(forTextStyle:)`, `.label`/`.secondaryLabel`/`.systemGroupedBackground`, `UIVisualEffectView(effect: UIBlurEffect(style: .systemMaterial))`, `layer.cornerCurve = .continuous`, `UISheetPresentationController` detents.
- AppKit: `NSSplitViewController` with a sidebar (`NSVisualEffectView` `.sidebar` material), source-list outline views, `NSToolbar` unified style, SF Symbols in toolbar items and `controlAccentColor`.

## Flutter

For iOS-first apps, use the Cupertino widgets: `CupertinoApp`, `CupertinoPageScaffold`, `CupertinoSliverNavigationBar` (large title), `CupertinoListSection.insetGrouped` + `CupertinoListTile`, `CupertinoSwitch`, `CupertinoSlidingSegmentedControl`, `CupertinoButton.filled`, `CupertinoSearchTextField`, `showCupertinoModalPopup`, `CupertinoActionSheet` and `CupertinoColors.systemBlue` / `.label` / `.secondaryLabel` / `.systemGroupedBackground` (`.resolveFrom(context)` for dark mode).

For a Material app that should *look* Apple: set `ThemeData(useMaterial3: true, colorScheme: ColorScheme.fromSeed(seedColor: Color(0xFF007AFF)).copyWith(surface: Colors.white))`, use `fontFamily` `.SF Pro Text` on Apple platforms with an Inter fallback, set `elevation: 0` everywhere, add `BackdropFilter(ImageFilter.blur(sigmaX: 20, sigmaY: 20))` behind app bars, use `ContinuousRectangleBorder` (or a radius ≈1.6× larger to match visually) and set `splashFactory: NoSplash.splashFactory` (iOS has no ink ripples; use opacity/scale feedback instead).

## React Native / Expo

- Colors: `PlatformColor('label')`, `PlatformColor('secondaryLabel')`, `PlatformColor('systemGroupedBackground')`, `PlatformColor('systemBlue')` on iOS, with a tokens object for Android, plus `useColorScheme()`.
- Navigation: native stack (`@react-navigation/native-stack` or Expo Router) with `headerLargeTitle: true`, `headerTransparent: true`, `headerBlurEffect: 'systemMaterial'`, and native bottom tabs.
- Blur: `expo-blur` `<BlurView intensity={80} tint="systemMaterial" />`. For iOS 26 glass, use `expo-glass-effect` where available, and otherwise fall back to BlurView.
- Text: `fontSize: 17`, `letterSpacing: -0.4` (≈ SF Pro Text tracking at 17pt), `fontWeight: '600'` for titles, and `allowFontScaling` on.
- Corners: `borderCurve: 'continuous'` (iOS) with `borderRadius`.
- Press feedback: `Pressable` with a scale-0.97 spring (Reanimated) and `expo-haptics` `selectionAsync()` on toggles.
- Icons: `expo-symbols` (`SymbolView`) for SF Symbols on iOS, with a Lucide fallback elsewhere.
