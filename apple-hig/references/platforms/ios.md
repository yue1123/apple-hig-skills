# iOS (iPhone)

Source: HIG › Getting started › [Designing for iOS](https://developer.apple.com/design/human-interface-guidelines/designing-for-ios), plus the iOS sections of every Foundations, Patterns, and Components page.

React Native support: **first-class.** Everything here is directly implementable.

## Contents

- [Device characteristics](#device-characteristics)
- [Best practices](#best-practices)
- [The iOS rules that matter most](#the-ios-rules-that-matter-most)
- [Key metrics](#key-metrics)
- [Gestures](#gestures)
- [System features to integrate](#system-features-to-integrate)
- [React Native landing notes](#react-native-landing-notes)
- [Review checklist](#review-checklist)

## Device characteristics

**Display** — medium-size, high-resolution.

**Ergonomics** — held in one or both hands, switching between portrait and landscape. Viewing distance is **no more than a foot or two**.

**Inputs** — Multi-Touch gestures, virtual keyboards, voice. People often want apps to use personal data, and input from the gyroscope and accelerometer.

**App interactions** — sessions range from a minute (checking updates, sending a message) to an hour or more (browsing, gaming, media). **People typically have multiple apps open and switch frequently.**

## Best practices

Apple's four iOS priorities, each of which has concrete consequences:

- **Limit onscreen controls; make secondary details and actions discoverable with minimal interaction.** Help people concentrate on the primary task and content.
- **Adapt seamlessly to appearance changes** — orientation, Dark Mode, Dynamic Type — so people can use the configurations that suit them.
- **Support one-handed reach.** Controls in the **middle or bottom** of the display are easier and more comfortable to reach. This is why **swipe to navigate back** and **swipe actions in list rows** matter so much on iPhone specifically: they put navigation and actions within thumb reach.
- **Integrate platform capabilities instead of asking for data** (with permission) — payments, biometric authentication, location.

## The iOS rules that matter most

Collected from across the HIG, these are the iOS-specific rules that most often separate a native-feeling app from a ported one:

**Layout**
- Content and backgrounds extend to all screen edges; chrome floats above on the Liquid Glass layer. → [layout.md](../foundations/layout.md)
- Safe areas applied as padding, not by shrinking the frame.
- **Avoid full-width buttons.** Buttons belong inset within system margins; a full-width button must harmonize with the hardware corner radius.
- **Keep the status bar** unless you're a game or immersive media. Obscure content behind it with a scroll edge effect.
- Support both orientations where reasonable; a landscape-only app must work rotated both ways.

**Navigation**
- Tab bar for navigation only — never for actions; keep it visible on pushed screens. → [navigation-and-search.md](../components/navigation-and-search.md)
- **Large titles** transition to standard on scroll and back at the top.
- **Search at the bottom** when it's a priority (Settings, Mail, Notes); top when bottom content must stay visible; inline when scoped to one view.
- Never disable or hide tab bar buttons; explain empty sections inside them.

**Presentation**
- Sheets support the **medium detent** for progressive disclosure, with a **grabber** when resizable. → [presentation.md](../components/presentation.md)
- **Swipe to dismiss**, confirmed with an **action sheet** if there are unsaved changes.
- **Action sheet, not alert**, for choices related to an intentional action.
- **No popovers in compact width** — use a sheet.
- One sheet at a time; never sheet-behind-sheet.

**Controls**
- **44 × 44 pt** minimum hit region; ~12 pt padding around bezeled controls, ~24 pt around borderless ones. → [accessibility.md](../foundations/accessibility.md)
- Switch style **only in a list row**; a toggling button elsewhere.
- **Clear button** on text fields; correct `keyboardType` and `textContentType` on every field.
- Segmented controls: **≤ 5 segments on iPhone**, text or icons but not both.
- Context menus with a **graphical preview**; destructive items last and flagged.

**Color and type**
- `PlatformColor` / `DynamicColorIOS` — never hard-coded hex where a semantic color exists. → [color-and-materials.md](../foundations/color-and-materials.md)
- Grouped vs. system background sets not mixed on one screen.
- Body text 17 pt; layout survives **3.1×** scaling at AX5. → [typography.md](../foundations/typography.md)
- No in-app appearance toggle.

**System integration**
- Home Screen quick actions: **up to four**, SF Symbols not emoji.
- Notifications: honest interruption levels, no marketing at Time Sensitive.
- Share via the system share sheet with `square.and.arrow.up`.

## Key metrics

| Metric | Value |
|---|---|
| Minimum hit target | 44 × 44 pt |
| Padding around bezeled controls | ~12 pt |
| Padding around borderless controls | ~24 pt |
| Default body text | 17 pt |
| Minimum text size | 11 pt |
| Max Dynamic Type scale (body) | ~3.1× (53 pt at AX5) |
| Widget standard margin | 16 pt (11 pt for tight groupings) |
| Text contrast minimum | 4.5:1 (3:1 at 18 pt+ or bold) |
| Logical widths to design against | 320 / 375 / 390 / 393 / 402 / 414 / 430 / 440 pt |
| Scale factors | @2x and @3x |

## Gestures

**Give people more than one way to interact.** Never assume a gesture is available — voice, keyboard, and Switch Control users need alternatives. → [accessibility.md](../foundations/accessibility.md)

**Respond consistently with expectations.** Tap activates or selects. **Don't repurpose a familiar gesture** for something unique to your app, and **don't invent a gesture for a standard action** like activating a button or scrolling.

**Handle gestures responsively**, with feedback that helps people predict the result and understand how much movement is needed.

**Indicate when a gesture isn't available.** Otherwise people conclude the app froze or they're doing it wrong — a locked object that doesn't move needs to look locked.

### System gestures you must not conflict with

| Gesture | System action |
|---|---|
| Three-finger swipe left / right | Undo / redo |
| Three-finger pinch in / out | Copy / paste |
| Shake | Undo / redo |
| Edge swipe from left | Navigate back |
| Swipe up from bottom | Home / app switcher |
| Swipe down from top-right | Control Center |
| Swipe down from top-left | Notification Center |

### Custom gestures

Add them only for specialized, frequent tasks not covered by existing gestures. A custom gesture must be **discoverable**, **straightforward**, **distinct from other gestures**, and **never the only way** to perform an important action.

**Shortcut gestures supplement standard ones, never replace them.** Apps with hierarchical navigation offer a Back button *and* an edge swipe — the gesture accelerates, the button guarantees.

**If a gesture is hard to describe in simple language and graphics, people will find it hard to learn.** That's a useful test.

## System features to integrate

Widgets, Home Screen quick actions, Spotlight, Shortcuts, Activity views. → [system-experiences.md](../components/system-experiences.md), [patterns/data-entry-search-settings.md](../patterns/data-entry-search-settings.md)

## React Native landing notes

### Baseline setup

```bash
# Safe areas, native navigation, gestures, animation — the four essentials.
npx expo install react-native-safe-area-context react-native-screens \
  react-native-gesture-handler react-native-reanimated
```

```jsx
// App root
import { GestureHandlerRootView } from 'react-native-gesture-handler';
import { SafeAreaProvider } from 'react-native-safe-area-context';
import { enableScreens } from 'react-native-screens';

enableScreens();   // native UIViewController-backed screens

export default function App() {
  return (
    <GestureHandlerRootView style={{ flex: 1 }}>
      <SafeAreaProvider>
        <NavigationContainer>{/* native-stack */}</NavigationContainer>
      </SafeAreaProvider>
    </GestureHandlerRootView>
  );
}
```

**Use `@react-navigation/native-stack`, not `@react-navigation/stack`.** The native stack wraps real `UINavigationController`, which gives you the correct push animation, the interactive back-swipe with its rubber-banding, large titles, the real search controller, and sheet presentation. The JS stack approximates all of these, and on iOS 26 the difference is immediately visible.

### The one-handed-reach consequence

Because reachability drives so much of the iOS guidance, prefer bottom-anchored primary actions and swipe affordances over top-anchored ones:

```jsx
// Primary action within thumb reach, above the tab bar.
<View style={{ position: 'absolute', bottom: insets.bottom + TAB_BAR_H + 12, left: 16, right: 16 }}>
  <Button title="Continue" role="primary" />
</View>

// Row actions by swipe (with a menu alternative for accessibility).
<Swipeable renderRightActions={() => <DeleteAction />}>
  <Row />
</Swipeable>
```

### Interactive back-swipe

```jsx
// On by default with native-stack. Only disable where a swipe would lose work,
// and then a visible Back/Cancel button is mandatory.
<Stack.Screen options={{ gestureEnabled: false }} />
```

Never build a custom edge-swipe-back; it will fight the system gesture and lose.

### Avoiding system gesture conflicts

```jsx
// Keep custom pan gestures away from screen edges and out of the bottom
// ~20 pt where the home indicator lives.
const pan = Gesture.Pan()
  .activeOffsetX([-15, 15])          // don't claim tiny movements
  .simultaneousWithExternalGesture(scrollRef);   // cooperate rather than compete
```

RN has no undo/redo hook for the three-finger swipe or shake, so those gestures are effectively unavailable to your app — provide an explicit Undo affordance instead. → [patterns/feedback-and-loading.md](../patterns/feedback-and-loading.md)

### Adaptivity checklist in code

```jsx
// All four appearance changes iOS expects you to handle, in one place.
const { width, height } = useWindowDimensions();   // orientation + resize
const scheme = useColorScheme();                    // Dark Mode
const fontScale = PixelRatio.getFontScale();        // Dynamic Type
const insets = useSafeAreaInsets();                 // hardware features
```

If a screen doesn't read at least three of these, it probably isn't adapting.

### Haptics and system feel

```js
import * as Haptics from 'expo-haptics';
// iOS haptics are strong and expected; see patterns/media-and-haptics.md
// for the semantic mapping. Never the only feedback channel.
```

### What needs native work

- Widgets, Live Activities, Controls, App Intents → SwiftUI extensions. → [system-experiences.md](../components/system-experiences.md)
- Real Liquid Glass on custom views → [rn/liquid-glass.md](../rn/liquid-glass.md)
- SF Symbols → `expo-symbols` (a native module, but off-the-shelf).
- Spotlight indexing, Quick Look previews, `UIDragInteraction` for cross-app drag.

## Review checklist

- [ ] `native-stack` used for navigation; `enableScreens()` called.
- [ ] `SafeAreaProvider` at the root; insets applied as padding with left/right handled.
- [ ] Content and backgrounds reach all screen edges; content scrolls under floating bars.
- [ ] Status bar visible (except games/immersive media) with content behind it obscured.
- [ ] Interactive back-swipe enabled; disabled only alongside a visible Back/Cancel button.
- [ ] No custom gesture competing with edge swipes, the bottom home-indicator area, or Control/Notification Center.
- [ ] Every custom gesture has a button or accessibility-action alternative.
- [ ] Primary actions within thumb reach; swipe actions available on list rows.
- [ ] No full-width buttons unless aligned to hardware curvature.
- [ ] Tab bar for navigation only, always visible on pushes, no hidden or disabled tabs.
- [ ] Large titles used where the screen scrolls.
- [ ] Search placed per its role (bottom / top / inline) using the native search bar.
- [ ] Sheets use native presentation with detents and a grabber; unsaved changes confirmed by action sheet.
- [ ] Action sheets used for intentional-action choices; alerts reserved for the unexpected.
- [ ] No popovers in compact width.
- [ ] 44 × 44 pt hit targets throughout, with `hitSlop` where glyphs are smaller.
- [ ] Switches only in list rows; toggling buttons elsewhere.
- [ ] `keyboardType`, `textContentType`, `autoCapitalize`, `returnKeyType` set on every field.
- [ ] Segmented controls ≤ 5 segments, native implementation.
- [ ] `PlatformColor`/`DynamicColorIOS` used; grouped and system background sets not mixed.
- [ ] Layout verified at AX5 text size and in both orientations.
- [ ] No in-app appearance toggle.
- [ ] Quick actions ≤ 4 with SF Symbols; share via the system sheet with the Apple share glyph.
- [ ] Notification interruption levels honest; no marketing at Time Sensitive.
