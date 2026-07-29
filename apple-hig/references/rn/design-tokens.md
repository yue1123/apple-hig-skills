# React Native Design Tokens for HIG

A complete, copy-pasteable token layer implementing the HIG values from [color-and-materials.md](../foundations/color-and-materials.md), [typography.md](../foundations/typography.md), [layout.md](../foundations/layout.md), and [accessibility.md](../foundations/accessibility.md).

Drop these files into the project, adapt the accent color, and the rest of the skill's guidance becomes mechanical to follow.

## Contents

- [Why tokens, not a theme library](#why-tokens-not-a-theme-library)
- [tokens/colors.ts](#tokenscolorsts)
- [tokens/typography.ts](#tokenstypographyts)
- [tokens/metrics.ts](#tokensmetricsts)
- [tokens/index.ts](#tokensindexts)
- [Using them](#using-them)
- [Contrast test](#contrast-test)
- [Adapting for NativeWind or Unistyles](#adapting-for-nativewind-or-unistyles)

## Why tokens, not a theme library

Two HIG rules make a plain token module the right shape:

1. **"Never hard-code a system color value"** — so iOS tokens must resolve to `PlatformColor` handles, not strings. Most theme libraries assume string colors and can't hold an opaque native handle.
2. **"Semantic colors used for their stated role"** — so tokens must be named by role (`textSecondary`, `surfaceGrouped`) rather than by value (`gray600`). A value-named palette makes the rule unenforceable.

Everything below is plain objects and `StyleSheet`, which works with any styling approach.

## tokens/colors.ts

```ts
import { Platform, PlatformColor, DynamicColorIOS } from 'react-native';

/**
 * Resolve to a system color on Apple platforms, fall back to hex elsewhere.
 * The fallbacks mirror the documented iOS values — they exist for Android and
 * web, and as the source for CI contrast checks (PlatformColor handles are
 * opaque to JS and can't be measured).
 */
const sys = <T extends string>(iosName: string, androidAttr: string | null, fallback: T) =>
  Platform.select({
    ios: PlatformColor(iosName),
    macos: PlatformColor(iosName),
    tvos: PlatformColor(iosName),
    visionos: PlatformColor(iosName),
    android: androidAttr ? PlatformColor(androidAttr) : fallback,
    default: fallback,
  });

/** Your own colors need explicit light/dark/high-contrast variants. */
const dynamic = (v: {
  light: string; dark: string; highContrastLight?: string; highContrastDark?: string;
}) => Platform.select({ ios: DynamicColorIOS(v), default: v.light });

export const colors = {
  // ---- Foreground -------------------------------------------------------
  textPrimary:     sys('label', '?android:attr/textColorPrimary', '#000000'),
  textSecondary:   sys('secondaryLabel', '?android:attr/textColorSecondary', 'rgba(60,60,67,0.60)'),
  textTertiary:    sys('tertiaryLabel', null, 'rgba(60,60,67,0.30)'),
  textQuaternary:  sys('quaternaryLabel', null, 'rgba(60,60,67,0.18)'),
  textPlaceholder: sys('placeholderText', '?android:attr/textColorHint', 'rgba(60,60,67,0.30)'),
  link:            sys('link', null, '#007AFF'),

  // ---- Separators -------------------------------------------------------
  // Translucent by design — do not flatten to opaque.
  separator:       sys('separator', null, 'rgba(60,60,67,0.29)'),
  separatorOpaque: sys('opaqueSeparator', null, '#C6C6C8'),

  // ---- Backgrounds: system set (non-grouped screens) --------------------
  surface:           sys('systemBackground', '?android:attr/colorBackground', '#FFFFFF'),
  surfaceSecondary:  sys('secondarySystemBackground', null, '#F2F2F7'),
  surfaceTertiary:   sys('tertiarySystemBackground', null, '#FFFFFF'),

  // ---- Backgrounds: grouped set (forms, settings-style lists) -----------
  // Never mix these with the system set on one screen — the page/card
  // inversion between the two is what makes cards read as cards.
  pageGrouped:    sys('systemGroupedBackground', null, '#F2F2F7'),
  cardGrouped:    sys('secondarySystemGroupedBackground', null, '#FFFFFF'),
  nestedGrouped:  sys('tertiarySystemGroupedBackground', null, '#F2F2F7'),

  // ---- Fills (translucent shapes behind content) ------------------------
  fill:           sys('systemFill', null, 'rgba(120,120,128,0.20)'),
  fillSecondary:  sys('secondarySystemFill', null, 'rgba(120,120,128,0.16)'),
  fillTertiary:   sys('tertiarySystemFill', null, 'rgba(118,118,128,0.12)'),
  fillQuaternary: sys('quaternarySystemFill', null, 'rgba(116,116,128,0.08)'),

  // ---- Semantic status --------------------------------------------------
  red:    sys('systemRed', null, '#FF3B30'),
  orange: sys('systemOrange', null, '#FF9500'),
  yellow: sys('systemYellow', null, '#FFCC00'),
  green:  sys('systemGreen', null, '#34C759'),
  teal:   sys('systemTeal', null, '#30B0C7'),
  blue:   sys('systemBlue', null, '#007AFF'),
  indigo: sys('systemIndigo', null, '#5856D6'),
  purple: sys('systemPurple', null, '#AF52DE'),
  pink:   sys('systemPink', null, '#FF2D55'),

  // ---- Grays ------------------------------------------------------------
  gray:  sys('systemGray', null, '#8E8E93'),
  gray2: sys('systemGray2', null, '#AEAEB2'),
  gray3: sys('systemGray3', null, '#C7C7CC'),
  gray4: sys('systemGray4', null, '#D1D1D6'),
  gray5: sys('systemGray5', null, '#E5E5EA'),
  gray6: sys('systemGray6', null, '#F2F2F7'),

  // ---- Your brand -------------------------------------------------------
  // Replace with your accent. High-contrast variants are required if this
  // is used behind small text.
  accent: dynamic({
    light: '#1B6EF3',
    dark: '#5C9BFF',
    highContrastLight: '#0B4FC0',
    highContrastDark: '#8FB9FF',
  }),

  // ---- Roles that map onto the above ------------------------------------
  destructive: sys('systemRed', null, '#FF3B30'),
  success:     sys('systemGreen', null, '#34C759'),
  warning:     sys('systemOrange', null, '#FF9500'),
} as const;

/**
 * Hex-only mirror for CI contrast checks and any place that needs a readable
 * string (SVG fills, chart libraries that reject native handles).
 * Keep in sync with the fallbacks above.
 */
export const colorValues = {
  light: {
    textPrimary: '#000000', textSecondary: 'rgba(60,60,67,0.60)',
    surface: '#FFFFFF', pageGrouped: '#F2F2F7', cardGrouped: '#FFFFFF',
    separator: 'rgba(60,60,67,0.29)', accent: '#1B6EF3',
  },
  dark: {
    textPrimary: '#FFFFFF', textSecondary: 'rgba(235,235,245,0.60)',
    surface: '#000000', pageGrouped: '#000000', cardGrouped: '#1C1C1E',
    separator: 'rgba(84,84,88,0.65)', accent: '#5C9BFF',
  },
} as const;
```

## tokens/typography.ts

```ts
import { Platform, PixelRatio, TextStyle } from 'react-native';

/**
 * The "L" (default) column of the HIG Dynamic Type table for iOS/iPadOS,
 * with platform overrides. `fontFamily` is deliberately omitted so the
 * system font resolves per platform (SF on Apple, Roboto on Android).
 */
const ios = {
  largeTitle: { fontSize: 34, lineHeight: 41, fontWeight: '400' },
  title1:     { fontSize: 28, lineHeight: 34, fontWeight: '400' },
  title2:     { fontSize: 22, lineHeight: 28, fontWeight: '400' },
  title3:     { fontSize: 20, lineHeight: 25, fontWeight: '400' },
  headline:   { fontSize: 17, lineHeight: 22, fontWeight: '600' },
  body:       { fontSize: 17, lineHeight: 22, fontWeight: '400' },
  callout:    { fontSize: 16, lineHeight: 21, fontWeight: '400' },
  subhead:    { fontSize: 15, lineHeight: 20, fontWeight: '400' },
  footnote:   { fontSize: 13, lineHeight: 18, fontWeight: '400' },
  caption1:   { fontSize: 12, lineHeight: 16, fontWeight: '400' },
  caption2:   { fontSize: 11, lineHeight: 13, fontWeight: '400' },
} satisfies Record<string, TextStyle>;

/** macOS: fixed sizes, no Dynamic Type, and `headline` is Bold not Semibold. */
const macos = {
  largeTitle: { fontSize: 26, lineHeight: 32, fontWeight: '400' },
  title1:     { fontSize: 22, lineHeight: 26, fontWeight: '400' },
  title2:     { fontSize: 17, lineHeight: 22, fontWeight: '400' },
  title3:     { fontSize: 15, lineHeight: 20, fontWeight: '400' },
  headline:   { fontSize: 13, lineHeight: 16, fontWeight: '700' },
  body:       { fontSize: 13, lineHeight: 16, fontWeight: '400' },
  callout:    { fontSize: 12, lineHeight: 15, fontWeight: '400' },
  subhead:    { fontSize: 11, lineHeight: 14, fontWeight: '400' },
  footnote:   { fontSize: 10, lineHeight: 13, fontWeight: '400' },
  caption1:   { fontSize: 10, lineHeight: 13, fontWeight: '400' },
  caption2:   { fontSize: 10, lineHeight: 13, fontWeight: '500' },
} satisfies Record<string, TextStyle>;

/** tvOS: huge, and Medium weight by default — thin text dies at 10 feet. */
const tvos = {
  largeTitle: { fontSize: 76, lineHeight: 96, fontWeight: '500' },
  title1:     { fontSize: 76, lineHeight: 96, fontWeight: '500' },
  title2:     { fontSize: 57, lineHeight: 66, fontWeight: '500' },
  title3:     { fontSize: 48, lineHeight: 56, fontWeight: '500' },
  headline:   { fontSize: 38, lineHeight: 46, fontWeight: '500' },
  body:       { fontSize: 29, lineHeight: 36, fontWeight: '500' },
  callout:    { fontSize: 31, lineHeight: 38, fontWeight: '500' },
  subhead:    { fontSize: 38, lineHeight: 46, fontWeight: '400' },
  footnote:   { fontSize: 25, lineHeight: 32, fontWeight: '500' },
  caption1:   { fontSize: 25, lineHeight: 32, fontWeight: '500' },
  caption2:   { fontSize: 23, lineHeight: 30, fontWeight: '500' },
} satisfies Record<string, TextStyle>;

/** visionOS: iOS sizes, bolder body and titles. */
const visionos = {
  ...ios,
  body:     { ...ios.body, fontWeight: '500' },
  title1:   { ...ios.title1, fontWeight: '500' },
  title2:   { ...ios.title2, fontWeight: '500' },
  headline: { ...ios.headline, fontWeight: '700' },
} satisfies Record<string, TextStyle>;

export const text = Platform.select({
  ios, macos, tvos, visionos, default: ios,
}) as typeof ios;

/** Minimum font sizes per platform — never go below these. */
export const minFontSize = Platform.select({
  ios: 11, macos: 10, tvos: 23, visionos: 12, android: 12, default: 11,
});

/**
 * Font scale, plus the accessibility-size boolean that should switch layout
 * MODE (inline → stacked, fewer columns) rather than nudge values.
 */
export function useFontScale() {
  const scale = PixelRatio.getFontScale();
  return { scale, isAccessibilitySize: scale >= 1.35 };
}

/**
 * Caps for chrome text. Content text should scale freely — capping it is an
 * accessibility tradeoff you must be able to justify.
 */
export const maxScale = {
  content: undefined,  // no cap
  chrome: 1.4,         // tab labels, toolbar titles
  numeric: 1.2,        // fixed-geometry readouts
} as const;
```

## tokens/metrics.ts

```ts
import { Platform } from 'react-native';

/** Minimum hit targets from the HIG accessibility table. Android's 48 is the
 *  stricter Material figure and satisfies both when shared. */
export const minTarget = Platform.select({
  ios: 44, ipados: 44, macos: 28, tvos: 66, visionos: 60, android: 48, default: 44,
});

/** Padding around controls: bezeled controls read as targets, borderless
 *  ones don't — hence the asymmetry. */
export const controlPadding = { bezeled: 12, borderless: 24 } as const;

/** visionOS eye-targeting spacing. Do not reuse iOS values here. */
export const vision = {
  minTarget: 60,
  itemMargin: 16,
  centerSpacing: 60,
  largeButtonPadding: 4,
} as const;

/** tvOS overscan safe area — RN does not apply it. */
export const tvSafeArea = { top: 60, bottom: 60, left: 80, right: 80 } as const;

/** Spacing scale. 8-point grid with a 4 for tight groupings. */
export const space = { xs: 4, sm: 8, md: 12, lg: 16, xl: 24, xxl: 32 } as const;

/** Standard iOS side margin — matches where system components inset content. */
export const margin = 16;

/** Corner radii. Match container radii concentrically inside bars and cards. */
export const radius = { sm: 6, md: 10, lg: 14, xl: 20, capsule: 999 } as const;

/** Widget margins (WidgetKit extension, but useful for matching in-app cards). */
export const widgetMargin = { standard: 16, tight: 11 } as const;

/** Breakpoints derived from the size-class table, expressed as capability
 *  rather than device: check whether the layout FITS. */
export const breakpoint = {
  regularWidth: 768,   // iPad portrait and up
  tabletMin: 744,      // iPad mini
} as const;

/** Contrast minimums from the accessibility table. */
export const contrast = { bodyText: 4.5, largeOrBoldText: 3.0, target: 7.0 } as const;

/** Animation values that land close to system feel. Not HIG specs —
 *  Apple doesn't publish spring constants. */
export const motion = {
  screen:  { damping: 20, stiffness: 200 },
  sheet:   { damping: 22, stiffness: 240 },
  reduced: { damping: 100, stiffness: 500 },   // critically damped
  quickMs: 180,
  standardMs: 250,
  maxRoutineMs: 350,   // above this, routine transitions feel slow on repeat
} as const;
```

## tokens/index.ts

```ts
export { colors, colorValues } from './colors';
export { text, minFontSize, maxScale, useFontScale } from './typography';
export * from './metrics';
```

## Using them

```tsx
import { StyleSheet, View, Text, Pressable } from 'react-native';
import { colors, text, space, radius, minTarget, margin } from '@/tokens';

const s = StyleSheet.create({
  // Grouped list screen: gray page, white cards.
  page: { flex: 1, backgroundColor: colors.pageGrouped },
  card: {
    backgroundColor: colors.cardGrouped,
    borderRadius: radius.lg,
    marginHorizontal: margin,
  },
  row: {
    minHeight: minTarget,           // grows with Dynamic Type, never fixed
    paddingHorizontal: space.lg,
    paddingVertical: space.md,
    flexDirection: 'row',
    alignItems: 'center',
    gap: space.md,
  },
  hairline: { height: StyleSheet.hairlineWidth, backgroundColor: colors.separator },
  title: { ...text.body, color: colors.textPrimary },
  detail: { ...text.footnote, color: colors.textSecondary },
});

export function SettingRow({ title, detail, onPress }) {
  return (
    <Pressable style={s.row} onPress={onPress} accessibilityRole="button">
      <View style={{ flex: 1 }}>
        <Text style={s.title}>{title}</Text>
        {detail && <Text style={s.detail}>{detail}</Text>}
      </View>
      <Icon name="chevron.right" color={colors.textTertiary} />
    </Pressable>
  );
}
```

Note what the tokens make automatic: the page/card pairing comes from the grouped set, the hairline stays translucent, `minHeight` lets the row grow, and every color resolves natively so appearance and contrast settings are honored without a re-render.

### Layout-mode switching

```tsx
import { useFontScale } from '@/tokens';

function ListRow({ title, timestamp }) {
  const { isAccessibilitySize } = useFontScale();
  return (
    <View style={isAccessibilitySize ? s.stacked : s.inline}>
      <Text style={s.title}>{title}</Text>
      <Text style={s.detail}>{timestamp}</Text>
    </View>
  );
}
```

This is the HIG's "restructure at large sizes" rule, and it's the difference between a layout that survives AX5 and one that truncates.

## Contrast test

Because "4.5:1 minimum" is mechanically checkable, make it a test rather than a review habit:

```ts
// tokens/__tests__/contrast.test.ts
import { colorValues } from '../colors';
import { contrast } from '../metrics';

// WCAG relative luminance. Composite translucent colors over their
// background first — a token like rgba(60,60,67,0.6) has no contrast
// ratio on its own.
function luminance(hex: string) {
  const [r, g, b] = hex.match(/\w\w/g)!.map(h => {
    const c = parseInt(h, 16) / 255;
    return c <= 0.03928 ? c / 12.92 : ((c + 0.055) / 1.055) ** 2.4;
  });
  return 0.2126 * r + 0.7152 * g + 0.0722 * b;
}

function ratio(fg: string, bg: string) {
  const [a, b] = [luminance(fg), luminance(bg)].sort((x, y) => y - x);
  return (a + 0.05) / (b + 0.05);
}

describe('token contrast', () => {
  for (const mode of ['light', 'dark'] as const) {
    const c = colorValues[mode];
    it(`${mode}: primary text on surface meets AA`, () => {
      expect(ratio(c.textPrimary, c.surface)).toBeGreaterThanOrEqual(contrast.bodyText);
    });
    it(`${mode}: primary text on grouped card meets AA`, () => {
      expect(ratio(c.textPrimary, c.cardGrouped)).toBeGreaterThanOrEqual(contrast.bodyText);
    });
    it(`${mode}: white on accent meets AA`, () => {
      expect(ratio('#FFFFFF', c.accent)).toBeGreaterThanOrEqual(contrast.bodyText);
    });
  }
});
```

The `colorValues` mirror exists for exactly this: `PlatformColor` handles can't be read from JS, so the check runs against the fallbacks that mirror them. Keeping the mirror in sync is the small cost of having the check at all.

## Adapting for NativeWind or Unistyles

**NativeWind** — feed the hex mirror into `tailwind.config.js`, and keep a small escape hatch for the `PlatformColor` tokens:

```js
// tailwind.config.js
const { colorValues } = require('./tokens/colors');
module.exports = {
  theme: { extend: {
    colors: {
      'text-primary': colorValues.light.textPrimary,
      'surface': colorValues.light.surface,
      'page-grouped': colorValues.light.pageGrouped,
      // …with dark: variants via the dark mirror
    },
    fontSize: { body: ['17px', '22px'], headline: ['17px', '22px'], footnote: ['13px', '18px'] },
    spacing: { md: '12px', lg: '16px' },
    minHeight: { target: '44px' },
  }},
};
```

Caveat worth knowing: Tailwind classes compile to string colors, so you **lose `PlatformColor` resolution** — appearance and increased-contrast changes then depend on your `dark:` variants rather than the OS. For chrome and text that must track system colors exactly, keep using the token objects via `style={}`.

**Unistyles** — the token objects drop straight into a theme, and `PlatformColor` handles survive because Unistyles passes style values through:

```ts
import { colors, text, space } from './tokens';
export const lightTheme = { colors, text, space };
// Breakpoints from metrics.breakpoint; Unistyles handles the media-query switching.
```

Unistyles is the better fit if you want both the native color resolution and a breakpoint system.
