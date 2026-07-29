# Inputs: Gestures, Keyboards, Pointing Devices & Hardware

Source: HIG › Inputs — [Gestures](https://developer.apple.com/design/human-interface-guidelines/gestures), [Keyboards](https://developer.apple.com/design/human-interface-guidelines/keyboards), [Pointing devices](https://developer.apple.com/design/human-interface-guidelines/pointing-devices), [Focus and selection](https://developer.apple.com/design/human-interface-guidelines/focus-and-selection), [Apple Pencil and Scribble](https://developer.apple.com/design/human-interface-guidelines/apple-pencil-and-scribble), [Action button](https://developer.apple.com/design/human-interface-guidelines/action-button), [Camera Control](https://developer.apple.com/design/human-interface-guidelines/camera-control), [Digital Crown](https://developer.apple.com/design/human-interface-guidelines/digital-crown), [Eyes](https://developer.apple.com/design/human-interface-guidelines/eyes), [Remotes](https://developer.apple.com/design/human-interface-guidelines/remotes), [Game controls](https://developer.apple.com/design/human-interface-guidelines/game-controls), [Gyroscope and accelerometer](https://developer.apple.com/design/human-interface-guidelines/gyro-and-accelerometer), [Nearby interactions](https://developer.apple.com/design/human-interface-guidelines/nearby-interactions)

Platform-specific input details live in the platform files: [tvos.md](platforms/tvos.md) (remote, focus engine), [visionos.md](platforms/visionos.md) (eyes, hands), [watchos.md](platforms/watchos.md) (Digital Crown), [macos.md](platforms/macos.md) (keyboard, pointer).

## Contents

- [The governing principle](#the-governing-principle)
- [Standard gestures](#standard-gestures)
- [Gesture rules](#gesture-rules)
- [Keyboards](#keyboards)
- [Pointing devices](#pointing-devices)
- [Focus](#focus)
- [Apple Pencil and Scribble](#apple-pencil-and-scribble)
- [Hardware buttons and sensors](#hardware-buttons-and-sensors)
- [React Native mapping](#react-native-mapping)
- [Review checklist](#review-checklist)

## The governing principle

**Always give people more than one way to interact.** People prefer or need different inputs — voice, keyboard, Switch Control, pointer. **Never assume a specific gesture is available for a given task.** This single rule generates most of the guidance below, and it's the one most often violated by gesture-first designs.

## Standard gestures

Supported across platforms, with expected meanings:

| Gesture | Platforms | Common action |
|---|---|---|
| **Tap** | iOS, iPadOS, macOS, tvOS, visionOS, watchOS | Activate a control; select an item |
| **Swipe** | all | Reveal actions and controls; dismiss views; scroll |
| **Drag** | all | Move a UI element |
| **Touch (or pinch) and hold** | iOS, iPadOS, tvOS, visionOS, watchOS | Reveal additional controls or functionality |
| **Double tap** | all | Zoom in; zoom out if already zoomed; **primary action** on Apple Watch Series 9 and Ultra 2 |
| **Zoom** | iOS, iPadOS, macOS, tvOS, visionOS | Zoom a view; magnify content |
| **Rotate** | iOS, iPadOS, macOS, tvOS, visionOS | Rotate a selected item |

Additional iOS/iPadOS system gestures: **three-finger swipe** (undo/redo), **three-finger pinch** (copy/paste), **four-finger swipe** (switch apps, iPadOS), **shake** (undo/redo).

## Gesture rules

**Respond consistently with expectations.** People expect gestures to work the same regardless of context. **Don't use a familiar gesture for something unique to your app**, and **don't invent a gesture for a standard action** like activating a button or scrolling.

**Handle gestures as responsively as possible**, with immediate feedback that helps people predict the result and, where needed, communicates the extent and type of movement required.

**Indicate when a gesture isn't available.** Without a signal, people think the app froze or they're doing it wrong. A locked object that doesn't move must *look* locked; an unavailable button must be clearly distinct from an available one.

### Custom gestures

Add them **only for specialized tasks people perform frequently that existing gestures don't cover** — a game, a drawing app. A custom gesture must be:

- **Discoverable**
- **Straightforward to perform**
- **Distinct from other gestures**
- **Never the only way** to perform an important action

**Make them easy to learn.** Offer moments in your app that teach them, and test in real scenarios. **If it's hard to describe with simple language and graphics, people will find it hard to learn** — that's a reliable test.

**Shortcut gestures supplement standard ones, never replace them.** Hierarchical navigation offers a Back button *and* an edge swipe: the gesture accelerates for people who know it, the button guarantees access for everyone.

**Don't conflict with system gestures** — edge swiping, the visionOS palm-look for system overlays, the tvOS remote behaviors. In games and immersive experiences you can *defer* a system gesture, but that's the exception.

**Consider simultaneous recognition** only where it genuinely helps — unlikely in non-game apps, but a game might have a joystick and firing buttons operated at once.

## Keyboards

**Support Full Keyboard Access** on iOS, iPadOS, macOS, and visionOS — it lets people navigate and activate windows, menus, controls, and system features with the keyboard alone. Test it by turning it on in Accessibility settings.

**Respect standard keyboard shortcuts.** People rely on the shortcuts that work everywhere else.

**Don't repurpose standard shortcuts.** Only consider it when the standard action is meaningless in your app — an app with no text editing doesn't need ⌘I for Italic, so it could reasonably become Get Info.

**Define custom shortcuts only for your most frequently used commands.** Too many makes the app seem hard to learn.

**Use modifier keys as people expect** — Command while dragging moves items as a group; Shift while drag-resizing constrains aspect ratio; holding an arrow key moves the selection by your smallest unit until release.

**List modifier keys in this order: Control, Option, Shift, Command.**

**Don't add Shift for the upper character of a two-character key.** People already know Shift produces the upper character, so list the character itself: Hide Status Bar is **⌘/**, Help is **⌘?** — *not* ⇧⌘/.

**Let the system localize and mirror shortcuts.** It adapts primary and modifier keys to the connected keyboard and mirrors them in right-to-left layouts.

**Don't create a shortcut by adding a modifier to an unrelated command's shortcut.** ⇧⌘Z belongs to redo, so using it for something unrelated to undo is confusing.

**Games:** people expect ⌘Q to quit, but also expect to **rebind keys** to fit their play style.

**visionOS:** the shortcut interface shows a **flat list** with no submenu titles for context, so **each shortcut title must be descriptive on its own**. Connecting a physical keyboard also shows a virtual keyboard overlay with typing completion.

### Shortcuts worth knowing

| Shortcut | Action |
|---|---|
| ⌘, | Open app settings |
| ⌘? | Open the app's Help menu |
| ⌘. or Esc | Cancel the current operation |
| ⌘Z / ⇧⌘Z | Undo / redo |
| ⌘Space | Spotlight |
| ⌘Tab / ⇧⌘Tab | Next / previous app |
| ⇧Tab | Navigate controls in reverse |
| ⌃Tab / ⌃⇧Tab | Next / previous control group or table |
| ⌃F1 | Toggle Full Keyboard Access |
| ⌃F2 | Focus the menu bar |
| ⌃F5 | Focus the toolbar |
| ⌃F6 / ⌃⇧F6 | Next / previous panel |
| ⌘F5 | Toggle VoiceOver |
| ⌘` / ⇧⌘` | Next / previous window in the frontmost app |
| ⌘[ / ⌘] / ⌘\| | Left-align / right-align / center-align a selection |
| ⌘; | Find misspelled words |
| ⌘: | Spelling window |
| ⌃⌘F | Toggle full screen |
| **Space** | Play/pause media (all platforms with a keyboard) |

## Pointing devices

Relevant on macOS always, and on iPadOS/visionOS when a trackpad or mouse is attached.

**Provide hover feedback** on interactive elements — it's how people know something is interactive with a pointer.

**Change the pointer to communicate what will happen** — copy, drag link, disappearing item, operation not allowed. → [macos.md](platforms/macos.md)

**Support high-precision input** for pixel-perfect selection and editing where your app warrants it.

**Support spring loading** on buttons and segmented controls with a Magic Trackpad — activating a control by force-clicking while continuing to hold dragged content.

**Support secondary click** for context menus.

**tvOS: avoid displaying a pointer.** People expect to navigate a fixed number of items by focus, not drag a tiny pointer across a huge screen.

## Focus

**Rely on system-provided focus effects.** They're precisely tuned; create custom ones only if absolutely necessary.

**Don't change focus without interaction.** The exception is when someone is moving focus with a **discrete directional input** (keyboard, remote, game controller) and the focused item **disappears** — then move focus to a nearby item, since only a few are within one step. When they're not using such an input, **hide the focus indicator** instead, because you can't predict their next target.

**Match the scope to the platform.** On iPadOS and macOS, Full Keyboard Access reaches every control, so you only handle focus for **content** elements — list items, text fields, search fields — not buttons, sliders, and toggles. On **tvOS every element needs focus support**, because directional gestures are the only way to reach anything.

**Focus rings for text and search fields; highlights for lists and collections.** A whole-row highlight is easier to track. A ring suits an item filling a cell, like a photo.

**iPadOS focus groups:** focus moves through groups in reading order — leading to trailing, top to bottom. You may need to declare a container as a **single focus group** so focus moves down a vertical stack before moving trailing. **Set a group's primary item priority** so the most likely target receives focus when the group does.

**Customize the halo effect** when the inferred shape is wrong — matching rounded corners or Bézier paths, or repositioning it when another component occludes or clips it.

## Apple Pencil and Scribble

**Distinguish Pencil from finger input** where the difference matters — drawing vs. scrolling.

**Scribble** lets people write into any text field by hand; it works automatically in standard text inputs, so the requirement is not to break it with custom text views.

**Apple Pencil Pro haptics:** prefer **short** haptics. Continuous or long-lasting haptics don't clarify writing or drawing and make holding the pencil less pleasant. → [patterns/media-and-haptics.md](patterns/media-and-haptics.md)

## Hardware buttons and sensors

**Action button** (iPhone, Apple Watch, Apple Watch Ultra) — initiates an essential action without looking at the screen. Provide **hint text constructed with verbs** so people understand what press-and-hold does, and a **placeholder** for situational titles and values. → [components/system-experiences.md](components/system-experiences.md)

**Camera Control** (iPhone 16 and later) — a capacitive control for camera features; light-press for a preview, further press to act.

**Digital Crown** (Apple Watch) — vertical navigation and data inspection, always with a touch equivalent and visual feedback. → [watchos.md](platforms/watchos.md)

**Gyroscope and accelerometer** — motion input. Never make motion the only way to perform an action; not everyone can move their device freely, and doing so is impossible in some contexts.

**Nearby interactions** — proximity-based experiences (U1/UWB). Explain what's happening and require permission.

**Game controllers** — support standard button mappings, let people **rebind** keys and buttons, and provide haptics through the controller where available.

## React Native mapping

### Gestures

Use `react-native-gesture-handler` rather than the built-in `PanResponder` — it runs on the UI thread and interoperates with native scroll views:

```jsx
import { Gesture, GestureDetector } from 'react-native-gesture-handler';

const tap = Gesture.Tap().onEnd(() => activate());
const longPress = Gesture.LongPress().minDuration(500).onStart(() => showMenu());
const pan = Gesture.Pan()
  .activeOffsetX([-15, 15])            // don't claim tiny movements
  .simultaneousWithExternalGesture(scrollRef)   // cooperate with scrolling
  .onUpdate(e => { x.value = e.translationX; });

// Compose rather than nesting detectors — composition makes precedence explicit.
const composed = Gesture.Exclusive(longPress, tap);
<GestureDetector gesture={composed}><Row /></GestureDetector>
```

Three rules that prevent most gesture bugs:

```jsx
// 1. Standard meanings only. A double tap should zoom, not delete.
// 2. Keep custom pans away from screen edges and the bottom home-indicator strip.
// 3. Every gesture needs a non-gesture path.
<Swipeable renderRightActions={DeleteAction}>
  <Row
    accessibilityActions={[{ name: 'delete', label: 'Delete' }]}
    onAccessibilityAction={({ nativeEvent }) => nativeEvent.actionName === 'delete' && del()}
  />
</Swipeable>
```

Show unavailability rather than silently ignoring:

```jsx
// A locked row must look locked, not just fail to move.
<Row style={[s.row, locked && s.rowLocked]} />
{locked && <Icon name="lock.fill" />}
```

### Keyboard shortcuts

```jsx
// react-native-keyevent, react-native-key-command, or native-stack's
// UIKeyCommand support. Register with discoverability titles so the
// hold-Command overlay lists them.
useKeyCommand({ input: 'n', modifiers: ['command'], title: 'New Document' }, newDoc);
useKeyCommand({ input: 'z', modifiers: ['command'], title: 'Undo' }, undo);
useKeyCommand({ input: 'z', modifiers: ['command', 'shift'], title: 'Redo' }, redo);
useKeyCommand({ input: ',', modifiers: ['command'], title: 'Settings' }, openSettings);
useKeyCommand({ input: '/', modifiers: ['command'], title: 'Hide Status Bar' }, toggleStatusBar);
```

Note the modifier order in the arrays matches the display order Apple specifies (Control, Option, Shift, Command), and `⌘/` rather than `⇧⌘/` for the slash-key case.

Form traversal with a hardware keyboard:

```jsx
<TextInput returnKeyType="next" onSubmitEditing={() => next.current?.focus()} blurOnSubmit={false} />
<TextInput ref={next} returnKeyType="go" onSubmitEditing={submit} />
```

Space to play/pause where you have a player:

```jsx
useKeyCommand({ input: ' ' }, togglePlayback);   // expected on every keyboard platform
```

### Hover and pointer

```jsx
// Works on iPadOS with a pointer, macOS, and visionOS (as the look-at hover).
<Pressable
  onHoverIn={() => setHovered(true)}
  onHoverOut={() => setHovered(false)}
  style={[s.item, hovered && s.itemHovered]}
/>
```

RN exposes no `UIPointerInteraction`, so system pointer effects (lift, highlight, shape morphing) and pointer-shape changes need native work. Plain hover styling covers most cases acceptably.

### Focus

```jsx
// Keyboard/pointer focus on iPad and macOS.
<Pressable
  onFocus={() => setFocused(true)}
  onBlur={() => setFocused(false)}
  style={[s.control, focused && { borderWidth: 3, borderColor: PlatformColor('keyboardFocusIndicatorColor') }]}
/>
```

tvOS focus is a different system entirely — see [platforms/tvos.md](platforms/tvos.md) for `TVFocusGuideView`, `hasTVPreferredFocus`, and `useTVEventHandler`.

Focus groups and halo customization have no RN API; on iPad, order your view tree to match the intended focus order, since focus follows reading order through the hierarchy.

### Apple Pencil

```jsx
// Distinguish stylus from touch where the distinction matters.
const pan = Gesture.Pan().onUpdate(e => {
  if (e.pointerType === 'stylus') draw(e);
  else scroll(e);
});
```

PencilKit has no RN binding — a real drawing surface needs a native canvas. Scribble works automatically in `TextInput`; the requirement is to verify custom text components don't break it.

### Hardware buttons

```js
// Action button → an App Intent (native). expo-quick-actions and
// App Intents can route into RN, but the intent itself is Swift.
// Camera Control → AVFoundation, native only.
// Digital Crown → watchOS only, no RN.
```

### Motion sensors

```js
import { Accelerometer, Gyroscope, DeviceMotion } from 'expo-sensors';

const sub = DeviceMotion.addListener(({ rotation }) => { /* … */ });
DeviceMotion.setUpdateInterval(1000 / 60);
// Always provide a non-motion alternative — motion input excludes people who
// can't move the device freely, and contexts where moving it isn't possible.
```

### Game controllers

```js
// react-native-gamepad / react-native-controller-input, or native GameController.
// Support standard mappings, allow rebinding, and route haptics through
// the controller where available.
```

## Review checklist

- [ ] Every gesture-driven action has a button, menu item, or `accessibilityAction` alternative.
- [ ] Standard gestures keep their standard meanings; no familiar gesture repurposed.
- [ ] No custom gesture invented for a standard action.
- [ ] Custom gestures are discoverable, distinct, simple to describe, and never the only path.
- [ ] Custom pans avoid screen edges and the bottom home-indicator strip; they cooperate with scroll views rather than competing.
- [ ] Unavailable gestures are visibly unavailable.
- [ ] Gesture feedback is immediate and communicates required movement.
- [ ] `react-native-gesture-handler` used, with explicit gesture composition rather than nested detectors.
- [ ] Standard keyboard shortcuts respected and not repurposed; custom shortcuts limited to frequent commands.
- [ ] Modifier keys listed Control → Option → Shift → Command; no Shift added for upper characters of two-character keys.
- [ ] Shortcuts registered with discoverability titles so the Command overlay lists them.
- [ ] ⌘Z / ⇧⌘Z / ⌘, / ⌘? / Esc / Space handled where applicable.
- [ ] `returnKeyType` chains traverse forms with a hardware keyboard.
- [ ] Full Keyboard Access works; focus order is logical and a visible focus indicator exists.
- [ ] Hover feedback present wherever a pointer may be attached.
- [ ] Focus rings on text/search fields, highlights on list rows.
- [ ] Focus not moved programmatically except when the focused item disappears.
- [ ] Pencil input distinguished from touch where relevant; Scribble not broken by custom text views.
- [ ] Pencil Pro haptics kept short.
- [ ] Motion input never the only path to an action.
- [ ] Game controller mappings standard and rebindable.
- [ ] Action button intents provide verb-based hint text and placeholders.
