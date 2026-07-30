# Navigation in React Native

Design guidance: [components/navigation-and-search.md](../components/navigation-and-search.md) for choosing a structure,
[components/presentation.md](../components/presentation.md) for sheets and alerts,
[patterns/modality-and-multitasking.md](../patterns/modality-and-multitasking.md) for modality rules.
This page is about implementation.

Navigation is where an RN app most often stops feeling native, because the JS reimplementations
are the defaults and they're *almost* right. Nearly every item here is a one-line change that
recovers a system behavior you can't rebuild by hand.

## Contents

- [The one non-negotiable: native-stack](#the-one-non-negotiable-native-stack)
- [Headers](#headers)
- [Tab bars](#tab-bars)
- [Sheets and modals](#sheets-and-modals)
- [Intercepting dismissal](#intercepting-dismissal)
- [Per-platform structure](#per-platform-structure)
- [Review checklist](#review-checklist)

## The one non-negotiable: native-stack

`@react-navigation/stack` is a JS reimplementation of a push transition. `@react-navigation/native-stack`
wraps `UINavigationController`. Using the former is the single most visible cause of an app not
feeling like iOS:

| | `stack` (JS) | `native-stack` |
|---|---|---|
| Push animation | Approximated in JS | Real, on the UI thread |
| Interactive back-swipe | Partial, JS-driven | Real, with the system's rubber-banding |
| Large titles | No | `headerLargeTitle` |
| Real sheets with detents | No | `presentation: 'formSheet'` |
| Search field in the header | No | `headerSearchBarOptions` |
| Liquid Glass on the bar | No | Automatic |

The only reasons to stay on `stack` are a genuinely custom transition or a shared-element animation
the native stack can't express. That is rarer than teams assume, and it costs everything above.

## Headers

Let the native header render. A hand-rolled `<View>` with a centered title is a reimplemented
`UINavigationBar` — it loses the material, the back gesture, Dynamic Type scaling on the title,
and correct safe-area behavior.

```jsx
<Stack.Screen
  name="Settings"
  options={{
    title: 'Settings',              // also becomes the back button label on the next screen
    headerLargeTitle: true,         // collapses on scroll; conventional for top-level list screens
    headerTransparent: true,        // content scrolls under the bar
    headerBlurEffect: 'systemMaterial',
    headerShadowVisible: false,
    // Never headerStyle.backgroundColor — it overrides the material.
  }}
/>
```

Customize *within* the header rather than replacing it: `headerLeft`, `headerRight`, `headerTitle`.
Buttons passed this way are real bar-button items, so they inherit the correct hit target, tint, and
Dynamic Type behavior.

With `headerLargeTitle`, the scroll view needs `contentInsetAdjustmentBehavior="automatic"` or the
large title won't collapse correctly.

**A search field belongs in the header**, not in your content:

```jsx
options={{
  headerSearchBarOptions: {
    placeholder: 'Search Your Library',   // the placeholder conveys the scope
    onChangeText: e => setQuery(e.nativeEvent.text),
    hideWhenScrolling: true,
  },
}}
```

## Tab bars

`@react-navigation/bottom-tabs` renders a JS view. It cannot have the real material, the scroll-under
behavior, or the selection haptic. For iOS and Android, `react-native-bottom-tabs` mounts a real
`UITabBarController` / `BottomNavigationView`:

```jsx
import { createNativeBottomTabNavigator } from 'react-native-bottom-tabs/react-navigation';

const Tab = createNativeBottomTabNavigator();

<Tab.Screen
  name="Library"
  options={{
    tabBarLabel: 'Library',
    tabBarIcon: () => ({ sfSymbol: 'books.vertical' }),   // real SF Symbol
  }}
/>
```

Platform boundaries worth knowing before you commit:

- `react-native-bottom-tabs` is **iOS and Android only** — it is not the tvOS or macOS path.
- On tvOS the tab bar belongs at the **top** of the screen. React Navigation 7 added
  `tabBarPosition: 'bottom' | 'top' | 'left' | 'right'` for exactly this; v6 has no such option.
  → [platforms/tvos.md](../platforms/tvos.md)
- `tabBarPosition: 'left' | 'right'` renders as a sidebar, which is the iPad
  tab-bar-to-sidebar adaptation the HIG asks for — drive it off `useWindowDimensions()`, not
  `Platform.isPad`.

Rules that are design decisions, not implementation details: every tab is a **destination** (a tab
must never perform an action or open a modal), the tab bar stays visible on pushed screens and is
hidden only under modals, and the tab count stays small enough to avoid an overflow tab at any
supported width.

## Sheets and modals

`presentation` on a native-stack screen maps to real `UIViewController` presentation. Prefer it over
RN's `<Modal>` component, which makes it easy to stack modals — something the HIG explicitly warns
against.

```jsx
<Stack.Screen
  name="EditProfile"
  options={{
    presentation: 'formSheet',
    sheetAllowedDetents: 'fitToContents',   // or an array of fractions, e.g. [0.5, 1]
    sheetGrabberVisible: true,
    sheetCornerRadius: 20,
  }}
/>
```

- `sheetAllowedDetents` accepts `'fitToContents'` or an array of screen fractions.
- `sheetGrabberVisible` is not decoration — it's the only way a VoiceOver user can resize the sheet.
- `sheetLargestUndimmedDetentIndex` controls which detents dim the content behind them. Use it when
  the sheet is meant to be worked alongside the content below (a player, a map) rather than on top of it.
- `presentation: 'modal'` for a full card, `'formSheet'` for the sheet, `'fullScreenModal'` only for
  genuinely immersive or multistep tasks.

**Pair the sheet's Done with a Cancel.** Done alone implies completing the task is the only way out.
Put Cancel leading, Done trailing, and title the bar after the task.

## Intercepting dismissal

The drag-down gesture bypasses your Cancel button, so guarding only the button is a bug. One listener
covers the button, the gesture, and the iPad Escape key:

```jsx
useEffect(() => navigation.addListener('beforeRemove', e => {
  if (!isDirty) return;                    // never confirm when there's nothing to lose
  e.preventDefault();
  // An ACTION SHEET, not an alert: the swipe was intentional and there are three valid outcomes.
  showActionSheetWithOptions(
    { options: ['Save Changes', 'Discard Changes', 'Cancel'], destructiveButtonIndex: 1, cancelButtonIndex: 2 },
    i => {
      if (i === 0) save().then(() => navigation.dispatch(e.data.action));
      if (i === 1) navigation.dispatch(e.data.action);
    },
  );
}), [navigation, isDirty]);
```

Set `gestureEnabled: false` only for a genuinely uninterruptible moment (mid-save), never as a
substitute for a Cancel button.

## Per-platform structure

| Platform | Structure | Implementation note |
|---|---|---|
| iOS | Tab bar or stack | `native-stack` + `react-native-bottom-tabs` |
| iPadOS | Tab bar that becomes a sidebar at width | `tabBarPosition: 'left'`, driven by `useWindowDimensions()` |
| macOS | Sidebar; toolbar in the window chrome | `react-native-macos`; real toolbars need native code |
| tvOS | Tab bar at the **top**; focus-driven | `tabBarPosition: 'top'`; Menu returns focus to the bar |
| visionOS | Tab bar with symbol **and** label | Labels revealed on gaze |

Back means the **hierarchical parent**, not the previous screen. On tvOS this matters most, because
mapping the remote's Menu button to `navigation.goBack()` is wrong whenever the previous screen isn't
the parent.

## Review checklist

- [ ] `@react-navigation/native-stack`, not `@react-navigation/stack`.
- [ ] Native header used; no hand-rolled title bar; customization via `headerLeft`/`headerRight`/`headerTitle`.
- [ ] No `backgroundColor` on a header or tab bar.
- [ ] `headerTransparent` + `headerBlurEffect` so content scrolls under; `contentInsetAdjustmentBehavior="automatic"` where large titles are used.
- [ ] Search implemented via `headerSearchBarOptions`, with a scope-conveying placeholder.
- [ ] Native tab bar (`react-native-bottom-tabs`) on iOS/Android rather than the JS one.
- [ ] `tabBarPosition` correct per platform — top on tvOS, sidebar at iPad widths.
- [ ] Every tab is a destination; none performs an action or opens a modal.
- [ ] Sheets use `native-stack` `presentation`, not RN `<Modal>`; only one presented at a time.
- [ ] Detents set; `sheetGrabberVisible` true wherever the sheet is resizable.
- [ ] Done paired with Cancel; sheet title names the task.
- [ ] `beforeRemove` guards unsaved changes, confirming with an **action sheet** that offers Save.
- [ ] No confirmation shown when nothing has changed.
- [ ] Back/Menu resolves to the hierarchical parent, not `goBack()`.
