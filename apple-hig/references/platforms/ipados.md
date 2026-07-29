# iPadOS

Source: HIG › Getting started › [Designing for iPadOS](https://developer.apple.com/design/human-interface-guidelines/designing-for-ipados), plus the iPadOS sections of every Foundations, Patterns, and Components page.

React Native support: **first-class** (same target as iOS). Most iPad-specific work is layout, input, and window-resizing behavior — not new APIs. Read [ios.md](ios.md) first; this page covers only what differs.

## Contents

- [Device characteristics](#device-characteristics)
- [Best practices](#best-practices)
- [Resizable windows](#resizable-windows)
- [Adaptive navigation](#adaptive-navigation)
- [Multiple input modes](#multiple-input-modes)
- [The iPadOS rules that matter most](#the-ipados-rules-that-matter-most)
- [Key metrics](#key-metrics)
- [React Native landing notes](#react-native-landing-notes)
- [Review checklist](#review-checklist)

## Device characteristics

**Display** — large, high-resolution.

**Ergonomics** — held, set on a surface, or on a stand. Position changes viewing distance, but people are **typically within about 3 feet**.

**Inputs** — Multi-Touch gestures and virtual keyboards, an attached **keyboard** or **pointing device**, **Apple Pencil**, or **voice** — and **people often combine multiple input modes**. That combination is the defining iPad design problem.

**App interactions** — from a few quick actions to hours of immersive gaming, media, creation, or productivity. **People frequently run multiple apps at once**, want more than one onscreen, and expect inter-app capabilities like drag and drop.

## Best practices

- **Use the large display to elevate content** — minimize modal interfaces and full-screen transitions, and position controls where they're easy to reach but not in the way.
- **Let viewing distance and input mode determine content size and density.** A pointer allows denser targets than a fingertip; 3 feet allows smaller text than 10 feet.
- **Support Multi-Touch, physical keyboard/trackpad, and Apple Pencil**, and consider interactions that combine them.
- **Adapt seamlessly to orientation, multitasking modes, Dark Mode, and Dynamic Type — and transition effortlessly to running on macOS.**

## Resizable windows

This is the largest behavioral difference from iPhone: **iPad windows resize freely down to a minimum, like macOS**, and **your app gets no indication of the multitasking configuration people chose**. Everything must be driven by the size you're actually given.

**Design for the full-screen view first, and defer switching to a compact layout as long as possible.** Only collapse when a version of the full layout genuinely no longer fits. Premature collapsing makes the UI feel unstable as people drag.

**For split views, hide tertiary columns first** — inspectors before content lists.

**Test at the system window sizes** — halves, thirds, quadrants — across devices, and minimize surprising changes as people move through the minimum and maximum.

**Account for narrow, compact, and intermediate widths** in split view layouts specifically, ensuring it stays possible to navigate between panes logically at every width. → [layout-and-organization.md](../components/layout-and-organization.md)

## Adaptive navigation

**Prefer a tab bar**; it gives more space for content and enough flexibility for most apps' main areas.

**Consider a convertible tab bar.** The app launches with your choice of sidebar or tab bar, people can switch, and the presentation follows the view width. This removes the need to choose one navigation model for all widths.

**Let people customize the tab bar** in apps with many sections — Music lets someone put a favorite playlist there. If tabs are customizable, keep the **default list to five or fewer** so compact and regular views stay continuous.

**A toolbar and a tab bar can share the same horizontal space** at the top of the view — useful when you want top-level navigation while keeping full window width for content.

**Sidebar:** consider a tab bar first; use the tab bar's convertible sidebar appearance to expose areas people use less often. Let people hide the sidebar with the **built-in edge swipe**. → [navigation-and-search.md](../components/navigation-and-search.md)

## Multiple input modes

The iPad rule is not "support touch and also support pointer" — it's that people **switch and combine** them mid-task. Practical consequences:

**Hover states matter.** With a trackpad or mouse attached, people expect pointer feedback on interactive elements.

**Keyboard shortcuts matter.** With a keyboard attached, people expect ⌘-key shortcuts and a shortcut overlay on holding Command. Provide New/Open-style commands, and don't override system shortcuts. → [accessibility.md](../foundations/accessibility.md)

**Full Keyboard Access must work.** Focus must move through the interface in a logical order.

**Apple Pencil** support means recognizing pencil input distinctly from touch where it matters (drawing vs. scrolling), and Scribble for text entry.

**Popovers become viable** at regular width — they're the correct presentation for small temporary content on iPad where a sheet would be on iPhone.

**Search placement changes:** the **trailing side of the toolbar** is the familiar iPad pattern, especially for split views searching across columns. The **top of the sidebar** works for filtering navigation. A **dedicated tab or sidebar item** works for discovery-oriented search. In a dedicated search area, focus the field on arrival — **except when only a virtual keyboard is available**, where auto-focus would cover the view.

## The iPadOS rules that matter most

**Layout**
- Nothing may depend on device identity or multitasking mode — only on measured size.
- Collapse late, and hide tertiary columns first.
- Split views need regular width; account for every intermediate width.

**Presentation**
- Prefer **page** or **form** sheet styles — consistent default sizes, centered over a dimmed background.
- Popovers are available and preferred at regular width.
- Consider a **context menu for creating objects** (Files creates a folder from a long press on empty space).
- Inside a popover or modal, a **pop-up button** can replace a disclosure indicator for a small option set.

**Toolbars**
- **Let people customize the toolbar** — valuable in apps with many items, advanced functionality, or long sessions.
- Consider combining toolbar and tab bar in the same horizontal band.

**Interaction**
- **Drag and drop** is expected, including adding items to an in-progress drag session and accepting multiple simultaneous drops. → [patterns/notifications-sharing-files.md](../patterns/notifications-sharing-files.md)
- Support the standard ⌘Z / ⇧⌘Z for undo and redo.
- **Four-finger swipe switches apps** — don't conflict with it.

**Other**
- Widgets on iPad use the same rules as iPhone; @2x assets only.
- macOS transition should be effortless — which mostly means the pointer, keyboard, and window-resizing work above is already done.

## Key metrics

| Metric | Value |
|---|---|
| Size class | **Regular × regular** for all iPads, both orientations, full screen |
| Viewing distance | ~3 feet |
| Minimum hit target | 44 × 44 pt (same as iOS) |
| Default body text | 17 pt |
| Scale factor | @2x |
| Logical widths (portrait) | 744 / 768 / 810 / 820 / 834 / 1024 / 1032 pt |
| Default customizable tabs | ≤ 5 |
| Split view collapse threshold | Whenever the full layout stops fitting — not a fixed number |
| Extra iPad gesture | Four-finger swipe = switch apps |

## React Native landing notes

### Size-driven layout, never device-driven

```jsx
// Wrong — iPad identity says nothing about the current window size.
import { Platform } from 'react-native';
const isTablet = Platform.OS === 'ios' && Platform.isPad;
if (isTablet) return <TwoColumn />;

// Right — measured width, and expressed as "does the layout fit".
const { width } = useWindowDimensions();
const fitsTwoColumns = width >= SIDEBAR_W + CONTENT_MIN_W;
return fitsTwoColumns ? <TwoColumn /> : <SingleColumn />;
```

`useWindowDimensions()` updates continuously as people drag a window edge — which is exactly why module-scope `Dimensions.get()` produces the classic "correct until you resize" bug.

### Handling continuous resize without jank

```jsx
// Resize fires many times per drag. Derive during render; don't run
// expensive effects keyed on width.
const { width } = useWindowDimensions();
const columns = useMemo(() => Math.max(1, Math.floor(width / MIN_COL_W)), [width]);

// FlatList needs a key change to relayout on column count change.
<FlatList numColumns={columns} key={columns} />
```

Avoid animating layout on every dimension change — the system is already animating the window, and a second animation reads as lag.

### Adaptive navigation

```jsx
// react-native-bottom-tabs exposes the native sidebarAdaptable style,
// which is the real convertible tab bar rather than a hand-rolled switch.
<Tab.Navigator
  screenOptions={{ tabBarStyle: { position: 'absolute' } }}
  // On iPad this renders as a sidebar-adaptable UITabBarController.
/>
```

Hand-switching between a drawer and a tab bar at a breakpoint produces a jarring transition mid-drag; the native adaptable style animates properly.

### Pointer and hover

```jsx
// Pressable exposes hover on iPad with a pointer attached.
<Pressable
  onHoverIn={() => setHovered(true)}
  onHoverOut={() => setHovered(false)}
  style={[s.row, hovered && s.rowHovered]}
/>
```

For the system pointer effects (lift, highlight) you need `react-native-ios-context-menu`-style native interaction or a `UIPointerInteraction` wrapper — RN doesn't expose them.

### Keyboard shortcuts

```jsx
// react-native-key-command, or native-stack's UIKeyCommand support.
useKeyCommand({ input: 'n', modifiers: ['command'] }, newDocument);
useKeyCommand({ input: 'z', modifiers: ['command'] }, undo);
useKeyCommand({ input: 'z', modifiers: ['command', 'shift'] }, redo);
useKeyCommand({ input: 'f', modifiers: ['command'] }, focusSearch);
```

Registering these also populates the **hold-Command shortcut overlay**, which is how people discover them — so the shortcuts aren't just for power users.

Also make sure `TextInput` chains work with a hardware keyboard:

```jsx
<TextInput returnKeyType="next" onSubmitEditing={() => next.current?.focus()} />
```

### Split view

```jsx
const { width } = useWindowDimensions();
const canSplit = width >= 768;

canSplit ? (
  <View style={{ flexDirection: 'row', flex: 1 }}>
    <View style={{ width: sidebarW, minWidth: 240, maxWidth: 380 }}><MasterList /></View>
    {showInspector && width >= 1024 && <Inspector />}{/* tertiary hides first */}
    <View style={{ flex: 1, minWidth: 320 }}><Detail /></View>
  </View>
) : <Stack.Navigator>{/* master pushes detail */}</Stack.Navigator>
```

Selection state must live above both panes so the master row stays highlighted while the detail shows — pushing loses it otherwise. → [layout-and-organization.md](../components/layout-and-organization.md)

### Popovers instead of sheets at width

```jsx
const usePopover = width >= 768;
usePopover ? <Popover anchor={ref}>{content}</Popover> : <BottomSheet>{content}</BottomSheet>;
```

### Drag and drop

```jsx
// Within the app: gesture-handler based (see patterns/notifications-sharing-files.md).
// Cross-app drag needs native UIDragInteraction / UIDropInteraction —
// no RN binding. For the import direction, expo-document-picker plus a
// declared file-type association covers most real needs.
```

### Apple Pencil

```jsx
// PencilKit has no RN binding; drawing apps need a native canvas.
// For distinguishing pencil from finger in gestures:
const pan = Gesture.Pan().onUpdate(e => {
  // react-native-gesture-handler exposes pointerType on newer versions.
  if (e.pointerType === 'stylus') draw(e); else scroll(e);
});
```

Scribble works automatically in `TextInput` — no work required, but do test that custom text views don't break it.

### Stage Manager and external displays

Both arrive as ordinary size changes, so if the size-driven layout above is correct they need no special handling. What does need checking: **very small window sizes** (below iPhone width) and **very wide ones** (external display), which are the two ends most layouts have never been tested at.

## Review checklist

- [ ] No layout decision keyed on `Platform.isPad` or a device check; all driven by measured width.
- [ ] `useWindowDimensions()` used throughout; no module-scope `Dimensions.get()`.
- [ ] Tested at halves, thirds, and quadrants, plus minimum and maximum window sizes.
- [ ] Tested below iPhone width and on an external display width.
- [ ] Compact layout deferred until the full layout genuinely stops fitting.
- [ ] Tertiary columns (inspectors) hidden before content lists.
- [ ] `FlatList` column count changes carry a `key` change.
- [ ] No expensive effects keyed on width; no custom animation layered on window resize.
- [ ] Adaptive/convertible tab bar used rather than hand-switching between drawer and tabs.
- [ ] Default customizable tab list ≤ 5.
- [ ] Hover states present on interactive elements.
- [ ] ⌘-key shortcuts registered (including ⌘Z / ⇧⌘Z) so the Command overlay lists them.
- [ ] `returnKeyType` chains work with a hardware keyboard.
- [ ] Full Keyboard Access focus order is logical.
- [ ] Split view selection state owned above both panes; master stays highlighted.
- [ ] Split views have sensible pane min/max widths.
- [ ] Popovers used at regular width; sheets in compact.
- [ ] Sheets use page or form style.
- [ ] Toolbar customization considered for item-heavy apps.
- [ ] Drag and drop supported in-app, with menu alternatives.
- [ ] Four-finger swipe and edge swipes not conflicted with by custom gestures.
- [ ] Content density tuned for ~3 ft viewing and pointer precision, not copied from iPhone.
- [ ] Scribble still works in all text inputs.
