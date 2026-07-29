# React Native Platform Strategy

How to structure a React Native project that targets several Apple platforms without either duplicating the app or flattening every platform into iOS.

## Contents

- [Runtime support per platform](#runtime-support-per-platform)
- [Project structure](#project-structure)
- [The library baseline](#the-library-baseline)
- [Platform branching that scales](#platform-branching-that-scales)
- [What always needs native code](#what-always-needs-native-code)
- [Expo config plugins and native targets](#expo-config-plugins-and-native-targets)
- [Testing matrix](#testing-matrix)
- [Decision guide](#decision-guide)

## Runtime support per platform

| Platform | Runtime | Maturity | Notes |
|---|---|---|---|
| **iOS** | `react-native` | First-class | Reference platform |
| **iPadOS** | `react-native` | First-class | Same target; the work is layout, input, window resizing |
| **macOS** | `react-native-macos` (Microsoft) | Usable, out-of-tree | Lags upstream RN; menu bar / windows / panels need native code |
| **tvOS** | `react-native-tvos` (fork) | Usable | Focus engine integrated; no Expo; narrower package ecosystem |
| **visionOS** | `react-native-visionos` (Callstack) | Early | Renders as a Shared Space window; volumes and immersive spaces are native-only |
| **watchOS** | — | **None** | No RN runtime; SwiftUI only |

Two consequences worth deciding up front:

1. **macOS and tvOS need separate RN dependencies** (a different `react-native` package), so a single `package.json` can't serve iOS + macOS + tvOS cleanly. Use a monorepo with per-platform apps sharing a UI package, or accept separate repos.
2. **Mac Catalyst is often the better macOS path** for an iPad-first app — one codebase, and the iPadOS pointer/keyboard/resize work is most of what Catalyst needs. The tradeoff is that Catalyst apps read as iPad-derived: no menu bar richness, no `NSPanel`. → [platforms/macos.md](../platforms/macos.md)

## Project structure

For multiple Apple platforms, a monorepo with a shared UI package:

```
my-app/
├── packages/
│   ├── ui/                   # shared components, tokens, hooks
│   │   ├── tokens/           # → rn/design-tokens.md
│   │   ├── components/
│   │   │   ├── Button.tsx
│   │   │   ├── Button.tvos.tsx      # platform override by extension
│   │   │   └── Button.macos.tsx
│   │   └── hooks/
│   └── core/                 # business logic, API, state — no platform code
├── apps/
│   ├── mobile/               # react-native (iOS + iPadOS + Android)
│   ├── desktop/              # react-native-macos
│   ├── tv/                   # react-native-tvos
│   └── vision/               # react-native-visionos
└── native/
    ├── widgets/              # WidgetKit extension (Swift)
    ├── watch/                # watchOS app (SwiftUI)
    └── intents/              # App Intents (Swift)
```

The `.ios.tsx` / `.macos.tsx` / `.tvos.tsx` / `.visionos.tsx` extension mechanism is the cleanest branching tool RN has — the bundler picks the right file and you get no runtime cost or conditional sprawl. Use it when a component's *structure* differs by platform; use `Platform.select` when only a *value* differs.

```
Button.tsx        // iOS/iPadOS/Android — 44pt targets, capsule
Button.tvos.tsx   // focus states, scale-on-focus, 66pt targets
Button.macos.tsx  // 28pt, hover, focus ring
Button.visionos.tsx // 60pt, capsule/rounded-rect by context, no custom hover
```

## The library baseline

```bash
# iOS/iPadOS — the four essentials plus native navigation.
npx expo install \
  react-native-safe-area-context \
  react-native-screens \
  react-native-gesture-handler \
  react-native-reanimated \
  @react-navigation/native @react-navigation/native-stack
```

| Need | Package | Why this one |
|---|---|---|
| Navigation | `@react-navigation/native-stack` | Real `UINavigationController`: correct push animation, back-swipe, large titles, native search bar, sheet presentation |
| Tab bar | `react-native-bottom-tabs` | Real `UITabBarController`: Liquid Glass, scroll-under, iPadOS sidebar-adaptable |
| Safe areas | `react-native-safe-area-context` | Handles landscape side insets and Android cutouts, unlike core `SafeAreaView` |
| Gestures | `react-native-gesture-handler` | UI-thread, composes with native scroll views |
| Animation | `react-native-reanimated` | Worklets, `useReducedMotion` built in |
| SF Symbols | `expo-symbols` | Real symbols with weight matching and animations |
| Blur | `expo-blur` | Wraps `UIVisualEffectView` |
| Menus | `@react-native-menu/menu` or `zeego` | Real `UIMenu` with preview and destructive styling |
| Segmented control | `@react-native-segmented-control/segmented-control` | Real `UISegmentedControl` |
| Haptics | `expo-haptics` | Semantic notification/impact/selection categories |
| Lists | `@shopify/flash-list` | Recycling for the sizes where a table beats a grid |
| Keyboard | `react-native-keyboard-controller` | Substantially better than `KeyboardAvoidingView`, especially on Android |
| Secure storage | `expo-secure-store` / `react-native-keychain` | Keychain-backed; never `AsyncStorage` for secrets |

The through-line: **prefer the package that wraps the real UIKit component** over the one that reimplements it in JS. Every native-backed component brings the current platform appearance, the correct gestures, the haptics, and the accessibility behavior for free — and keeps bringing them after the next OS update.

## Platform branching that scales

### Branch on capability, not identity

```ts
// Wrong: device identity tells you nothing about the current window size.
const isTablet = Platform.OS === 'ios' && Platform.isPad;

// Right: measured size, expressed as a question about the layout.
const { width } = useWindowDimensions();
const fitsTwoColumns = width >= SIDEBAR_W + CONTENT_MIN_W;
```

### Centralize the platform table

Scattered `Platform.OS === '…'` checks are where multi-platform RN code rots. Put the differences in one table:

```ts
// packages/ui/platform.ts
import { Platform } from 'react-native';

export const P = {
  minTarget: Platform.select({ ios: 44, macos: 28, tvos: 66, visionos: 60, android: 48, default: 44 }),
  hasHover:  Platform.select({ macos: true, visionos: true, default: false }),  // iPad: runtime-detected
  hasHaptics: Platform.select({ ios: true, android: true, default: false }),    // none on visionOS/tvOS
  hasDynamicType: Platform.select({ macos: false, default: true }),
  hasDarkMode: Platform.select({ visionos: false, watchos: false, default: true }),
  focusDriven: Platform.OS === 'tvos',
  needsExplicitSafeArea: Platform.OS === 'tvos',   // RN doesn't apply overscan insets
} as const;
```

Then guards read as statements about the platform rather than as string comparisons:

```jsx
{P.hasHaptics && <HapticFeedback />}
{P.focusDriven ? <FocusableCard /> : <PressableCard />}
```

### Value differences vs. structural differences

```jsx
// Value difference → Platform.select inline.
paddingHorizontal: Platform.select({ tvos: 80, default: 16 })

// Structural difference → separate file.
// Card.tsx vs Card.tvos.tsx — the tvOS one has five focus states and
// scale-on-focus; expressing that with conditionals in one file is unreadable.
```

## What always needs native code

No RN runtime reaches these, on any platform:

| Feature | Framework | Notes |
|---|---|---|
| Widgets | WidgetKit (SwiftUI) | Data via App Group; reload with `reloadAllTimelines` |
| Live Activities | ActivityKit (SwiftUI) | Start/update/end callable from JS via a bridge |
| Controls (Control Center, Action button) | ControlWidget / App Intents | |
| App Shortcuts / Siri | App Intents | Intents that open the app can bridge; intents that complete without opening cannot |
| Snippets | App Intents | |
| watchOS app | SwiftUI | Entirely separate target |
| Complications, watch faces | WidgetKit / ClockKit | |
| tvOS Top Shelf | TVTopShelf extension | |
| Layered/parallax images | Asset catalog + Parallax Exporter | |
| visionOS volumes, immersive spaces | RealityKit / SwiftUI | |
| Spatial Audio | PHASE / RealityKit | |
| Real Liquid Glass on custom views | SwiftUI `glassEffect` | → [liquid-glass.md](liquid-glass.md) |
| macOS menu bar, `NSPanel`, multiple windows | AppKit | |
| Spotlight indexing, Quick Look generators | Core Spotlight / Quick Look | |
| Cross-app drag and drop | `UIDragInteraction` | Import direction is reachable via document picker |
| PencilKit | PencilKit | |
| Pointer effects, spring loading | `UIPointerInteraction` | |
| System color picker | `UIColorPickerViewController` | |

Plan these as Swift work from the start. Discovering mid-project that widgets can't be written in JS is a schedule problem, not a technical one.

## Expo config plugins and native targets

The failure mode: `npx expo prebuild --clean` **deletes hand-added Xcode targets**. Anything native must be generated by a config plugin.

```js
// app.config.js
export default {
  expo: {
    plugins: [
      // Generates widget / watch / intent targets and preserves them.
      ['@bacons/apple-targets', { appleTeamId: 'XXXXXXXXXX' }],
      'expo-apple-authentication',
      ['expo-build-properties', { ios: { deploymentTarget: '17.0' } }],
    ],
    ios: {
      // App Group for sharing data with widgets and the watch app.
      entitlements: { 'com.apple.security.application-groups': ['group.com.example.app'] },
      infoPlist: {
        // Purpose strings — sentence case, active voice, terminal period,
        // stating the benefit. See foundations/privacy.md.
        NSCameraUsageDescription: 'Lets you take a photo of the item you\'re reporting.',
      },
    },
  },
};
```

If you're on bare RN, commit the `ios/` directory and treat Xcode project changes as reviewable code.

## Testing matrix

The minimum set that catches most of what this skill's checklists cover:

**Appearance and text**
- Light and Dark
- Increase Contrast on
- Reduce Transparency on
- Bold Text on
- Larger Accessibility Text at **maximum** (≈3.1× body)
- Invert Colors on

**Layout**
- Smallest supported width (iPhone SE, 320 pt) and largest (iPad Pro / external display)
- Both orientations, and **both landscape rotations**
- iPad: halves, thirds, quadrants, minimum and maximum window sizes
- RTL (a separate launch — `forceRTL` needs a reload)

**Interaction**
- VoiceOver / TalkBack — actually navigate the app, don't just lint
- Reduce Motion on
- Full Keyboard Access with a hardware keyboard
- Switch Control, at least once per release

**Per platform**
- tvOS: focus reaches every element; focused items don't overlap neighbors
- macOS: every command in the menu bar; keyboard-only operation
- visionOS: target spacing ≥ 60 pt centers; test while wearing the device
- Oldest supported device for blur/animation frame rate

```jsx
// Worth building: a dev-only overlay that reports the current a11y state,
// so "did I test with Bold Text on?" is answerable at a glance.
if (__DEV__) return <A11yStateBadge />;
```

## Decision guide

**Targeting iOS + Android only?** Standard RN. Use the native-backed component list above so iOS looks native, and let Android take Material idioms where they differ (share glyph, elevation instead of blur, 48 dp targets).

**Adding iPad?** No new dependency. Do the [ipados.md](../platforms/ipados.md) work: size-driven layout, hover, keyboard shortcuts, split views, popovers. This is the highest-value platform addition per unit of effort.

**Adding Mac?** Prefer **Mac Catalyst** if the app is iPad-shaped and you can accept iPad-derived Mac idioms. Choose `react-native-macos` if you need real Mac structure — and budget for the menu bar as native work either way.

**Adding Apple TV?** Separate app with `react-native-tvos`, sharing your `core` package. The UI mostly can't be shared: focus states, 10-foot type, and overscan insets change nearly every component. Expect a parallel component tree, not a reskin.

**Adding Vision Pro?** `react-native-visionos` for the windowed UI. If the product's value is spatial — volumes, immersion, 3D — that part is SwiftUI/RealityKit regardless, so decide whether RN is carrying enough of the app to be worth the split.

**Need a Watch app?** SwiftUI, sharing data through an App Group. Design the complications and notifications first — they're used more than the app. → [platforms/watchos.md](../platforms/watchos.md)

**Need widgets, Live Activities, or Siri?** Swift extensions from day one, scaffolded by a config plugin.
