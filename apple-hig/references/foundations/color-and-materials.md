# Color, Dark Mode & Materials

Source: HIG › Foundations › [Color](https://developer.apple.com/design/human-interface-guidelines/color), [Dark Mode](https://developer.apple.com/design/human-interface-guidelines/dark-mode), [Materials](https://developer.apple.com/design/human-interface-guidelines/materials)

Read this when picking colors, building a theme/token layer, implementing dark mode, or reaching for blur/glass effects.

## Contents

- [Rules that matter most](#rules-that-matter-most)
- [Semantic color system](#semantic-color-system)
- [Reference values](#reference-values)
- [Dark Mode](#dark-mode)
- [Materials: Liquid Glass vs. standard](#materials-liquid-glass-vs-standard)
- [Platform differences](#platform-differences)
- [React Native mapping](#react-native-mapping)
- [Review checklist](#review-checklist)

## Rules that matter most

**Never hard-code a system color value.** Apple changes the actual RGB values between releases based on environmental variables. Documented values are for design-time reference only; resolve them at runtime. In RN this means `PlatformColor()` on iOS/macOS, not a hex constant. Hard-coded hex is the single most common way an RN app stops looking native after an OS update.

**Color must never be the only carrier of meaning.** Anyone with color blindness, anyone in bright sunlight, and anyone using Increase Contrast needs a second signal — a label, a symbol shape, a position, a state change. If you remove all color from a screenshot and it becomes ambiguous, the design is not done.

**Don't reuse one color for two meanings.** If your brand color signals "this borderless text is tappable," then non-interactive text must not use it. Consistency is what lets people learn your interface once instead of re-reading it on every screen.

**Every color needs light, dark, and increased-contrast variants — even in a single-appearance app.** Liquid Glass adapts to the content behind it, so it will pull your colors into contexts you didn't plan for. Supplying only one variant means the material has nothing correct to fall back to.

**Don't redefine what a semantic color means.** `separator` is not a text color; `secondaryLabel` is not a background. The system tunes each one for its stated role across appearances, contrast settings, and vibrancy — using it elsewhere means it will drift wrong exactly when a setting changes.

**Test in real conditions.** Bright sun mutes and darkens colors; dim rooms make them look saturated and bright. On visionOS, the color of a physical wall behind the window changes how your palette reads. On tvOS, test on several TV brands and display settings — TV calibration varies enormously.

## Semantic color system

Apple's dynamic colors are named for **purpose**, not appearance. Pick by role and the system handles light/dark/contrast/vibrancy for you.

### Foreground (iOS/iPadOS)

| Role | Use for | UIKit name |
|---|---|---|
| Label | Primary text | `label` |
| Secondary label | Supporting text | `secondaryLabel` |
| Tertiary label | De-emphasized text | `tertiaryLabel` |
| Quaternary label | Watermark-level text | `quaternaryLabel` |
| Placeholder text | Empty field hints | `placeholderText` |
| Separator | Hairline that lets content show through | `separator` |
| Opaque separator | Hairline that fully blocks content | `opaqueSeparator` |
| Link | Text that navigates | `link` |

### Background (iOS/iPadOS)

Two parallel sets, each with three depth levels. Choose the set by context, then the level by nesting depth:

- **Grouped** (`systemGroupedBackground` → `secondary…` → `tertiary…`) — use when the screen is a grouped list/form. This is the inset-card look of Settings.
- **System** (`systemBackground` → `secondary…` → `tertiary…`) — use for everything else.

Level maps to hierarchy: primary = the overall view, secondary = a group inside it, tertiary = a group inside that.

Note the inversion between sets: in a grouped context the *page* is gray and the *cards* are white; in a system context the *page* is white. Mixing the two sets is what produces the "cards that don't read as cards" bug.

### Fills

`systemFill` → `secondarySystemFill` → `tertiarySystemFill` → `quaternarySystemFill`. These are translucent grays for filling shapes behind content — segmented control tracks, unstyled buttons, chips. They're translucent by design so they work on any background.

## Reference values

Design-time reference only — resolve at runtime via `PlatformColor`. These are the widely-published iOS values and are accurate enough for building an Android-side palette or a design mock; treat any exact match against a current OS build as coincidence rather than contract.

### System colors

| Name | Light | Dark |
|---|---|---|
| Red | `#FF3B30` | `#FF453A` |
| Orange | `#FF9500` | `#FF9F0A` |
| Yellow | `#FFCC00` | `#FFD60A` |
| Green | `#34C759` | `#30D158` |
| Mint | `#00C7BE` | `#63E6E2` |
| Teal | `#30B0C7` | `#40C8E0` |
| Cyan | `#32ADE6` | `#64D2FF` |
| Blue | `#007AFF` | `#0A84FF` |
| Indigo | `#5856D6` | `#5E5CE6` |
| Purple | `#AF52DE` | `#BF5AF2` |
| Pink | `#FF2D55` | `#FF375F` |
| Brown | `#A2845E` | `#AC8E68` |

visionOS uses the dark values in all cases.

### Grays

| Name | Light | Dark |
|---|---|---|
| systemGray | `#8E8E93` | `#8E8E93` |
| systemGray2 | `#AEAEB2` | `#636366` |
| systemGray3 | `#C7C7CC` | `#48484A` |
| systemGray4 | `#D1D1D6` | `#3A3A3C` |
| systemGray5 | `#E5E5EA` | `#2C2C2E` |
| systemGray6 | `#F2F2F7` | `#1C1C1E` |

Gray 1 → 6 moves *away* from the content in light mode and *toward* the background in dark mode, which is why the pairs are not inversions of each other.

### Labels, separators, backgrounds

| Role | Light | Dark |
|---|---|---|
| label | `#000000` | `#FFFFFF` |
| secondaryLabel | `rgba(60,60,67,0.60)` | `rgba(235,235,245,0.60)` |
| tertiaryLabel | `rgba(60,60,67,0.30)` | `rgba(235,235,245,0.30)` |
| quaternaryLabel | `rgba(60,60,67,0.18)` | `rgba(235,235,245,0.16)` |
| separator | `rgba(60,60,67,0.29)` | `rgba(84,84,88,0.65)` |
| opaqueSeparator | `#C6C6C8` | `#38383A` |
| systemBackground | `#FFFFFF` | `#000000` |
| secondarySystemBackground | `#F2F2F7` | `#1C1C1E` |
| tertiarySystemBackground | `#FFFFFF` | `#2C2C2E` |
| systemGroupedBackground | `#F2F2F7` | `#000000` |
| secondarySystemGroupedBackground | `#FFFFFF` | `#1C1C1E` |
| tertiarySystemGroupedBackground | `#F2F2F7` | `#2C2C2E` |

Labels and separators are **translucent**, not solid. That's deliberate — they blend into whatever is behind them. If you flatten them to opaque hex you lose that, and text over an image or material will look wrong.

### Contrast targets

- Absolute minimum: **4.5:1** for any text.
- Aim for **7:1** on custom foreground/background pairs, especially small text.
- Increased contrast mode should widen the gap *significantly*, not marginally. A variant that's 5% different isn't serving the setting's purpose.

## Dark Mode

Supported on iOS, iPadOS, macOS, tvOS. **Not a thing on visionOS or watchOS** — visionOS glass adapts to background luminance automatically; watchOS is always dark.

**Don't ship an in-app appearance toggle.** People set appearance system-wide and expect apps to honor it. An app-level setting means two places to change one thing, and users read a non-responding app as broken. (Following the system also means handling Auto, which flips appearance *while your app is running* — so appearance must be reactive state, never read once at startup.)

**Dark colors are not inverted light colors.** Some invert, many don't. Derive nothing by inversion; define each variant.

**iOS layers two dark background sets: base and elevated.** Base is dimmer and recedes; elevated is brighter and advances. The system swaps base→elevated automatically when a view comes forward as a sheet or popover, and uses elevated to separate windows in multitasking. Using a custom background color opts you out of this depth cue, which is why custom-themed dark sheets often look flat or detached.

**Soften white-background imagery in dark mode.** A photo with a white background will glow against a dark UI. Darken it slightly.

**Design separate icon assets when shape alone doesn't survive the flip.** A full-moon glyph needs an outline on light and none on dark. SF Symbols handle this for you; hand-drawn art often doesn't.

**Test the ugly combinations.** Dark Mode + Increase Contrast + Reduce Transparency, each alone and all together. Dark-on-dark text is the classic failure, and Increase Contrast can counterintuitively *reduce* the gap between dark text and a dark background.

On macOS, when the accent color is set to graphite, window backgrounds pick up color from the desktop picture (*desktop tinting*). Give custom component backgrounds slight transparency so they participate — but only for neutral states, never for states that carry their own color, or the color will visibly shift as the desktop changes.

## Materials: Liquid Glass vs. standard

Two distinct material families with non-overlapping jobs.

### Liquid Glass — the functional layer

Liquid Glass is the material for **controls and navigation** — tab bars, toolbars, sidebars — forming a layer that floats *above* content. Content scrolls and peeks through beneath it. That's what produces the depth and the sense that chrome and content are different kinds of thing.

**Don't put Liquid Glass in the content layer.** Its whole purpose is separating interactive chrome from content; using it inside content collapses that distinction and produces a muddled hierarchy. Use standard materials for app backgrounds and in-content surfaces instead.

The one exception: an in-content control with a transient interactive element — a slider thumb, a toggle knob — takes on a glass appearance *while being manipulated*, to signal live interactivity.

**Use it sparingly on custom controls.** System components adopt it automatically. Every custom glass surface you add competes with content for attention, which is the opposite of the point. Limit it to your app's most important functional elements.

Two variants:

- **regular** — blurs and adjusts background luminosity to protect legibility. Most system components use this. Choose it when the background could hurt legibility, or when the component carries significant text: alerts, sidebars, popovers.
- **clear** — highly translucent, keeps rich background content prominent. For components floating over photos and video, where immersion matters more than chrome.

With **clear**, decide on a dimming layer: add a **35% dark dimming layer** if the underlying content is bright; skip it if the content is already dark, or if you're using standard AVKit playback controls (they bring their own).

Both variants shift appearance in response to the user's preferred Liquid Glass look and to Reduce Transparency / Increase Contrast. Don't pick a variant for the color it appears to produce.

**Color on glass:** glass is colorless by default and takes color from content behind it. Apply color only where emphasis is genuinely earned — a primary call to action, a status indicator. For a primary action, color the **background**, not the label or symbol. Don't color multiple control backgrounds in one view.

On small elements (toolbars, tab bars) the system flips glass between light and dark appearance based on underlying content, and labels/symbols default to monochrome — darkening over light content, lightening over dark. Larger elements like sidebars render **more opaque** to stay legible over complex backgrounds.

**If your app is colorful, keep chrome monochrome.** Colorful content plus colorful control labels is unreadable. Conversely, in a mostly monochrome app, a brand accent color is an effective and safe identity move.

**Check the resting state, not just the scrolled state.** Content will intermittently scroll under chrome — that's fine. What must be legible is the default position, e.g. the top of the scroll.

### Standard materials — the content layer

Standard materials (blur + vibrancy) convey structure *within* content, beneath the glass layer. iOS/iPadOS provide four thicknesses: `ultraThin`, `thin`, `regular` (default), `thick`.

Pick by **semantic role and recommended usage, not apparent color** — system settings change how each one looks. The tradeoff is stable, though:

- Thicker = more opaque = better contrast for text and fine detail.
- Thinner = more translucent = better at preserving context about what's behind.

**Always use vibrant colors on top of a material.** System vibrancy values are pre-tuned so nothing reads as too dark, bright, saturated, or washed out in any context.

iOS vibrancy levels for labels: `label` (default, highest contrast) → `secondaryLabel` → `tertiaryLabel` → `quaternaryLabel` (lowest). Avoid `quaternaryLabel` on `thin` and `ultraThin` — the contrast collapses. Fills: `fill` → `secondaryFill` → `tertiaryFill`. Separators have a single value that works everywhere.

## Platform differences

| Platform | What changes |
|---|---|
| **iOS / iPadOS** | Two background sets (system vs. grouped) × three levels. Base/elevated dark backgrounds. Four standard material thicknesses plus per-material vibrancy values. |
| **macOS** | Much larger semantic palette tied to control states (`controlAccentColor`, `selectedContentBackgroundColor`, `keyboardFocusIndicatorColor`, `unemphasizedSelected…` for non-key windows, etc.). App accent color applies when the user's setting is *multicolor*; any other choice overrides yours — except fixed-color sidebar icons, which carry meaning and are left alone. Desktop tinting under the graphite accent. Two blending modes: behind-window and within-window. |
| **tvOS** | Limited palette coordinated with your logo; let content dominate. **Never indicate focus with color alone** — scaling and responsive motion are the primary focus signals. Liquid Glass runs through navigation and system surfaces; image views and buttons adopt glass *on focus*. |
| **visionOS** | No Dark Mode; system colors use dark values. Windows use a non-customizable system *glass* material. Prefer translucency over opacity — opaque areas block people's view of their surroundings and feel constricting. Use color sparingly and prefer it in **bold text and large areas**; color in light or small text is hard to read through glass. In full immersion, keep brightness balanced — a bright object on a near-black field causes real discomfort, especially if it moves or flashes. Three vibrancy levels: `label`, `secondaryLabel`, `tertiaryLabel` (inactive only). |
| **watchOS** | Always dark; no Dark Mode setting. Use background color to *communicate* (Activity's ring-matched backgrounds), not as decoration. Avoid full-screen background color on long-lived screens like workouts or audio players — it costs battery on OLED and fatigues the eye. Users may prefer tinted over full-color graphic complications. Keep the default material backgrounds on modal sheets; they're what orients people in a full-screen modal. |

## React Native mapping

### Resolve system colors at runtime

RN ships two APIs that map directly onto the "never hard-code" rule:

```js
import { PlatformColor, DynamicColorIOS, Platform, StyleSheet } from 'react-native';

// PlatformColor: look up a named OS color. iOS/macOS use UIKit/AppKit names,
// Android uses resource names. Unknown names throw, so gate by platform.
const colors = {
  label: Platform.select({
    ios: PlatformColor('label'),
    android: PlatformColor('?android:attr/textColorPrimary'),
    default: '#000000',
  }),
  background: Platform.select({
    ios: PlatformColor('systemBackground'),
    android: PlatformColor('?android:attr/colorBackground'),
    default: '#FFFFFF',
  }),
  separator: Platform.select({
    ios: PlatformColor('separator'),
    default: 'rgba(60,60,67,0.29)',
  }),
  accent: Platform.select({
    ios: PlatformColor('systemBlue'),
    default: '#007AFF',
  }),
};

// DynamicColorIOS: your own color with light/dark (and optional high-contrast) variants.
// The OS resolves it — no re-render needed on appearance change.
const brand = Platform.select({
  ios: DynamicColorIOS({
    light: '#1B6EF3',
    dark: '#5C9BFF',
    highContrastLight: '#0B4FC0',
    highContrastDark: '#8FB9FF',
  }),
  default: '#1B6EF3',
});
```

`DynamicColorIOS` is strictly better than branching on `useColorScheme()` for colors: resolution happens natively, so appearance flips have no JS round-trip and no flash of the wrong color.

### Reactive appearance for what PlatformColor can't cover

```js
import { useColorScheme } from 'react-native';

const scheme = useColorScheme(); // 'light' | 'dark' | null — updates live
```

Use this for things that aren't colors: choosing an image asset, a status bar style, a map style, a syntax theme. Never read appearance once at module scope — Auto mode flips it mid-session.

### Building a token layer that survives

When you need one palette across iOS and Android, define tokens semantically (never `blue500`, always `textPrimary` / `surfaceElevated`), and let iOS resolve to `PlatformColor` while Android and web fall back to hex:

```js
const token = (ios, fallback) => Platform.select({ ios: PlatformColor(ios), default: fallback });

export const theme = {
  textPrimary:    token('label', '#000000'),
  textSecondary:  token('secondaryLabel', 'rgba(60,60,67,0.6)'),
  surface:        token('systemBackground', '#FFFFFF'),
  surfaceGrouped: token('secondarySystemGroupedBackground', '#FFFFFF'),
  pageGrouped:    token('systemGroupedBackground', '#F2F2F7'),
  hairline:       token('separator', 'rgba(60,60,67,0.29)'),
  fill:           token('tertiarySystemFill', 'rgba(118,118,128,0.12)'),
};
```

Two things this gets right that a plain hex palette doesn't: translucent tokens stay translucent, and iOS picks up any future OS value changes for free.

### Materials in RN

RN has no built-in blur. `expo-blur` (`BlurView`, with `tint` and `intensity`) or `@react-native-community/blur` bridge `UIVisualEffectView` on iOS and `NSVisualEffectView` on macOS. Map HIG thickness to intensity roughly: `ultraThin` ≈ 20, `thin` ≈ 40, `regular` ≈ 60, `thick` ≈ 85.

For Liquid Glass specifically, see [rn/liquid-glass.md](../rn/liquid-glass.md) — approximating it with blur alone misses the parts that actually make it read as glass.

Two things to get right regardless of library:

```js
// 1. Respect Reduce Transparency — a translucent surface is an accessibility problem
//    for some people, and the system setting is how they tell you so.
import { AccessibilityInfo } from 'react-native';

const [reduceTransparency, setReduceTransparency] = useState(false);
useEffect(() => {
  AccessibilityInfo.isReduceTransparencyEnabled().then(setReduceTransparency);
  const sub = AccessibilityInfo.addEventListener(
    'reduceTransparencyChanged',
    setReduceTransparency,
  );
  return () => sub.remove();
}, []);

// Fall back to an opaque background rather than a fainter blur.
return reduceTransparency
  ? <View style={[s.bar, { backgroundColor: theme.surface }]} />
  : <BlurView intensity={60} tint="default" style={s.bar} />;
```

```js
// 2. Android has no equivalent material. Don't ship a fake blur that costs frames —
//    use an elevated opaque surface, which is the correct Android idiom anyway.
const Surface = Platform.OS === 'ios' ? BlurView : View;
```

### Contrast checking in CI

Because the "4.5:1 minimum / 7:1 target" rule is mechanically checkable, it's worth a test rather than a code review habit. Compute relative luminance per WCAG on your resolved token pairs (light and dark separately) and fail the build under 4.5:1. Note this only works on hex fallbacks — `PlatformColor` values are opaque handles that JS can't read, so run the check against the fallback palette that mirrors them.

## Review checklist

- [ ] No hard-coded hex where a `PlatformColor` exists on iOS.
- [ ] Every custom color has light + dark variants (`DynamicColorIOS` or equivalent), and high-contrast variants where text is small.
- [ ] Semantic colors used for their stated role — no `separator` as text, no `secondaryLabel` as background.
- [ ] Grouped vs. system background sets not mixed within one screen.
- [ ] Translucent tokens (labels, separators, fills) still translucent, not flattened to opaque.
- [ ] No information conveyed by color alone; a second signal exists everywhere.
- [ ] No in-app appearance toggle; appearance is reactive state, not read once.
- [ ] Text contrast ≥ 4.5:1 in both appearances; ≥ 7:1 for custom small text.
- [ ] Tested under Dark Mode × Increase Contrast × Reduce Transparency, individually and combined.
- [ ] Liquid Glass confined to the chrome layer; standard materials used inside content.
- [ ] `clear` glass over bright media has a 35% dark dimming layer.
- [ ] At most one or two colored control backgrounds per view; monochrome chrome if content is colorful.
- [ ] Reduce Transparency falls back to opaque, not to weaker blur.
- [ ] tvOS: focus never signaled by color alone.
- [ ] visionOS: no large opaque areas; color reserved for bold/large text.
- [ ] watchOS: no full-screen background color on long-lived screens.
