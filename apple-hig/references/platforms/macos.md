# macOS

Source: HIG › Getting started › [Designing for macOS](https://developer.apple.com/design/human-interface-guidelines/designing-for-macos), [The menu bar](https://developer.apple.com/design/human-interface-guidelines/the-menu-bar), [Windows](https://developer.apple.com/design/human-interface-guidelines/windows), plus the macOS sections of every other page.

React Native support: **[react-native-macos](https://github.com/microsoft/react-native-macos)** (Microsoft), out-of-tree. Views, text, layout, and most core components work. The menu bar, windows, panels, and pointer effects need native work — see the landing notes.

## Contents

- [Device characteristics](#device-characteristics)
- [Best practices](#best-practices)
- [The menu bar](#the-menu-bar)
- [Windows](#windows)
- [The macOS rules that matter most](#the-macos-rules-that-matter-most)
- [Key metrics](#key-metrics)
- [React Native landing notes](#react-native-landing-notes)
- [Review checklist](#review-checklist)

## Device characteristics

**Display** — typically large and high-resolution, extensible with additional displays including an iPad.

**Ergonomics** — stationary use, device on a desk or table, viewing distance **about 1 to 3 feet**.

**Inputs** — **any combination** of physical keyboard, pointing device, game controls, and Siri. High-precision input is expected.

**App interactions** — from a few minutes of quick tasks to several hours of deep concentration. **Multiple apps open simultaneously**, with smooth transitions between active and inactive states.

## Best practices

- **Leverage large displays to present more content in fewer nested levels and with less modality** — while keeping a comfortable information density that doesn't make people strain.
- **Let people resize, hide, show, and move windows** to fit their work style, and support full-screen mode for a distraction-free context.
- **Use the menu bar to give access to all commands.**
- **Support high-precision input** for pixel-perfect selections and edits.
- **Handle keyboard shortcuts** so people can accelerate actions and work keyboard-only.
- **Support personalization** — customizable toolbars, windows configured to show preferred views, chosen colors and fonts.

## The menu bar

The menu bar is the defining macOS structure, and the rule that governs everything else: **every command in your app must exist in the menu bar**, because the toolbar is customizable and hideable, and because the menu bar is where Full Keyboard Access users and first-time users look.

**Support the default system menus and their ordering.** People expect a familiar order, and the system implements much of the standard functionality for you — selecting text in a standard field makes Edit › Copy work automatically.

**Always show the same set of menu items.** Keeping items visible teaches people what your app can do. **Disable unavailable items; never hide them.**

**Use familiar icons** for common actions — the same ones the system uses for Copy, Share, Delete. → [icons-and-images.md](../foundations/icons-and-images.md)

**Support the standard keyboard shortcuts** for standard items — Copy, Cut, Paste, Save, Print. Define custom shortcuts only when necessary.

**Prefer short, one-word menu titles.** Display sizes and menu bar extras affect spacing, and one-word titles scan quickly. Multi-word titles use title-style capitalization.

### Standard menus

| Menu | Notes |
|---|---|
| **App** | **About first**, followed by a separator so it sits alone. Settings item belongs here (⌘,). |
| **File** | Document-level commands. Find items may belong here rather than Edit if you search for files or objects. |
| **Edit** | Undo/redo at the top, with ⌘Z / ⇧⌘Z. |
| **Format** | Includes the font and text color panels — don't duplicate them in Window. |
| **View** | **Provide it even for a subset of functions.** An app with no tab bar, toolbar, or sidebar but with full-screen support still provides a View menu containing only Enter/Exit Full Screen. Show/hide items must **reflect current state** — "Show Toolbar" when hidden, "Hide Toolbar" when visible. |
| **App-specific** | For custom commands. **Reflect your app's hierarchy** (Mail orders Mailbox, Message, Format because mailboxes contain messages contain formatting). Order **most general to least** — people expect leading menus to be more specialized than trailing ones. |
| **Window** | **Provide it even with one window**, including Minimize and Zoom so Full Keyboard Access users can invoke them. Consider show/hide items for panels. **Don't list panels in the documents list.** |
| **Help** | Your app's help documentation. |

### Dynamic menu items

Items revealed by holding a modifier key.

**Never the only way to accomplish a task** — they're hidden by default, so they suit shortcuts to advanced actions achievable another way (someone who never finds *Minimize All* can still minimize each window).

**Use them primarily in menu bar menus** — in contextual or Dock menus they're even harder to discover.

**One modifier key only.** Multiple modifiers while opening a menu and choosing an item is physically awkward and further reduces discoverability.

### Menu bar extras

**Use a symbol** — SF Symbols or an interface icon. Both use black and clear to define shape so the system can color them for light and dark menu bars and for the selected state. **Menu bar height is 24 pt.**

**Show a menu, not a popover**, unless the functionality is genuinely too complex for a menu.

**Let people decide whether it appears.** Typically added via a setting in your settings window; consider offering it during setup for discoverability.

**Don't rely on it being present.** The system hides and shows extras regularly, and you can't know what other extras exist or predict your position.

**Expose the functionality elsewhere too** — a Dock menu is always available while your app runs, whereas people can hide or ignore a menu bar extra.

### iPadOS menu bar

On iPad the menu bar is **often hidden in full screen**, so **all functions must be reachable through the UI**, and dynamic menu items — only available with a hardware keyboard — always need an alternative. **Don't use the menu bar as a catch-all** for functionality that didn't fit elsewhere.

**Reserve YourApp › Settings for opening your app's page in iPadOS Settings**; link an internal preferences area with a separate item beneath it in the same group.

**For tab-style navigation, add each tab to the View menu** with key bindings — the View menu is the natural place for switching views.

**Group items into submenus more aggressively than on Mac**, since iPad menu rows are taller for touch and some iPads have smaller screens.

## Windows

**Use system-defined appearances.** People rely on visual differences to identify the foreground window and know which will accept input. System components update background and button appearances automatically on state change; custom implementations must do it themselves.

**Avoid critical information or actions in a bottom bar** — people often position windows so the bottom edge is off-screen. If you need one, use it for a small amount of information directly related to the window's contents (the Finder's status bar shows item count, selection count, available space). For more information, use an **inspector** on the trailing side of a split view.

Also on window layout: **avoid content within the camera housing** at the top edge, and **use the system full-screen experience** so the housing is accommodated automatically. → [patterns/modality-and-multitasking.md](../patterns/modality-and-multitasking.md)

## The macOS rules that matter most

**Structure**
- Every command in the menu bar; every toolbar item also a menu command.
- Non-customizable, always-visible settings window toolbar; window title reflects the visible pane; last pane restored. Settings in the App menu with ⌘,. → [patterns/data-entry-search-settings.md](../patterns/data-entry-search-settings.md)
- Nothing critical at a window's bottom edge — this applies to windows, sidebars, and bottom bars alike.

**Color and materials**
- Far larger semantic palette tied to control states — `controlAccentColor`, `selectedContentBackgroundColor`, `keyboardFocusIndicatorColor`, the `unemphasizedSelected…` family for non-key windows. → [color-and-materials.md](../foundations/color-and-materials.md)
- App accent color applies only when the user's setting is *multicolor*; any other choice overrides yours — except fixed-color sidebar icons.
- **Desktop tinting** under the graphite accent: give custom component backgrounds slight transparency, but only in neutral states.
- **No Dynamic Type on macOS** — sizes are fixed; body is 13 pt. AppKit exposes control-matched font variants (`controlContentFont`, `menuFont`, `titleBarFont`, …). → [typography.md](../foundations/typography.md)

**Controls**
- **28 × 28 pt** default control size, **20 × 20 pt** minimum.
- Switches, checkboxes, radio buttons in the **window body**, never the frame. Checkboxes for setting hierarchies; radio buttons for >2 mutually exclusive options. → [selection-and-input.md](../components/selection-and-input.md)
- Square buttons, image buttons, help buttons, and path controls all belong **in a view, not the window frame** — use toolbar items in toolbars.
- **One help button per window**, positioned in the lower corner opposite the dismissal buttons.
- Append a **trailing ellipsis** to a push button title that opens another window, view, or app.
- Tab views (not segmented controls) for view switching in the main window area.

**Interaction**
- **Hover** produces tooltips; write them verb-first, ≤ 75 characters, without repeating the control name. → [patterns/launching-and-onboarding.md](../patterns/launching-and-onboarding.md)
- Support **drag from inactive windows** (background selections) without activating them, and **drag to the Finder** in a reopenable format.
- Change the **pointer** to indicate a drop outcome; consider a **numeric badge** for multi-item drags.
- **Spring loading** on buttons and segmented controls with a Magic Trackpad.
- **Autosave**, and handle the case where people turn it off — then show unsaved state and present a save dialog on close/quit/logout/restart. **Never show an unsaved dot while autosave is on.** → [patterns/notifications-sharing-files.md](../patterns/notifications-sharing-files.md)

**Presentation**
- Sheets at a reasonable default size; **let people bring other windows forward without dismissing a sheet**; use a **panel** for repeated input + observation. → [presentation.md](../components/presentation.md)
- Popovers can be **detachable** into panels.
- Caution symbols **sparingly** — only for genuinely unexpected data loss.

**Privacy**
- Signed with a Developer ID, sandboxed, and **no assumptions about who is signed in** (fast user switching). → [privacy.md](../foundations/privacy.md)

## Key metrics

| Metric | Value |
|---|---|
| Default control size | 28 × 28 pt |
| Minimum control size | 20 × 20 pt |
| Default body text | 13 pt |
| Minimum text size | 10 pt |
| Dynamic Type | **Not supported** |
| Menu bar height | 24 pt |
| Split view divider | 1 pt (thin, preferred) |
| Tooltip length | ≤ 60–75 characters |
| Viewing distance | 1–3 feet |
| Scale factors | @1x and @2x |
| Document icon minimum size | 16 × 16 px |

## React Native landing notes

### Setup

```bash
npx react-native-macos-init          # adds the macos/ target to an existing RN project
```

`react-native-macos` tracks upstream RN a few versions behind, so pin your RN version to what it supports. Many community packages have no macOS podspec — check before committing to a dependency.

### What works, what doesn't

| Area | Status in react-native-macos |
|---|---|
| View, Text, Image, ScrollView, FlatList, Pressable | Works |
| `PlatformColor` with AppKit names | Works — `PlatformColor('controlAccentColor')` etc. |
| `useColorScheme`, appearance changes | Works |
| Hover (`onMouseEnter`/`onMouseLeave` or Pressable hover) | Works |
| Keyboard events, key commands | Works via `onKeyDown` / native modules |
| Tooltips | `tooltip` prop on some components |
| **Menu bar** | Native `NSMenu` only — no JS API |
| **Multiple windows / panels** | Native `NSWindow` / `NSPanel` only |
| **Pointer effects, spring loading** | Native only |
| **Drag to Finder** | Native only |
| Dynamic Type | N/A (macOS has none) |

### The menu bar is the main native task

Because "every command in the menu bar" is non-negotiable on macOS, budget for this rather than treating it as optional polish:

```swift
// macos/MyApp-macOS/AppDelegate.mm (or Swift) — build the NSMenu natively,
// and forward selections into RN via an event emitter.
// Keep the standard menus and ordering: App, File, Edit, Format, View,
// app-specific, Window, Help.
```

```js
// JS side: receive the command and route it.
import { NativeEventEmitter, NativeModules } from 'react-native';
const emitter = new NativeEventEmitter(NativeModules.MenuBridge);
emitter.addListener('menuCommand', ({ id }) => {
  if (id === 'file.new') newDocument();
  if (id === 'edit.undo') undo();
});
```

`react-native-menubar-extra` and similar packages cover menu bar *extras*; the main menu bar generally needs your own bridge.

### Semantic colors

```js
// AppKit names, which differ from UIKit — see color-and-materials.md.
const mac = {
  windowBg:   PlatformColor('windowBackgroundColor'),
  controlBg:  PlatformColor('controlBackgroundColor'),
  label:      PlatformColor('labelColor'),
  secondary:  PlatformColor('secondaryLabelColor'),
  separator:  PlatformColor('separatorColor'),
  accent:     PlatformColor('controlAccentColor'),
  focusRing:  PlatformColor('keyboardFocusIndicatorColor'),
  selected:   PlatformColor('selectedContentBackgroundColor'),
  // Non-key window selection — using the emphasized color everywhere is a
  // recognizable macOS mistake.
  selectedInactive: PlatformColor('unemphasizedSelectedContentBackgroundColor'),
};
```

Track window key state so the inactive variants actually get used:

```jsx
// AppState 'active'/'inactive' approximates window key state well enough
// for selection styling.
const isKey = AppState.currentState === 'active';
<View style={{ backgroundColor: isKey ? mac.selected : mac.selectedInactive }} />
```

### Fixed type, no Dynamic Type

```js
// macOS text styles are fixed. Don't apply the iOS scaling machinery here.
export const macText = {
  largeTitle: { fontSize: 26, lineHeight: 32 },
  title1:     { fontSize: 22, lineHeight: 26 },
  title2:     { fontSize: 17, lineHeight: 22 },
  title3:     { fontSize: 15, lineHeight: 20 },
  headline:   { fontSize: 13, lineHeight: 16, fontWeight: '700' },  // Bold, not Semibold
  body:       { fontSize: 13, lineHeight: 16 },
  callout:    { fontSize: 12, lineHeight: 15 },
  subheadline:{ fontSize: 11, lineHeight: 14 },
  footnote:   { fontSize: 10, lineHeight: 13 },
};
```

Note macOS `headline` is **Bold** where iOS is Semibold — reusing the iOS scale verbatim gets this wrong.

### Hover and pointer

```jsx
// Hover feedback is expected on macOS, not optional.
<Pressable
  onHoverIn={() => setHover(true)}
  onHoverOut={() => setHover(false)}
  tooltip="Restore default settings"   // verb-first, ≤75 chars, no control name
  style={[s.row, hover && { backgroundColor: mac.controlBg }]}
/>
```

### Keyboard shortcuts

```jsx
// Standard shortcuts must work. Register them alongside the menu bar items
// so the menu displays the shortcut — a shortcut with no menu entry is
// undiscoverable, which is the macOS failure mode.
useKeyCommand({ input: 'z', modifiers: ['command'] }, undo);
useKeyCommand({ input: 'z', modifiers: ['command', 'shift'] }, redo);
useKeyCommand({ input: 's', modifiers: ['command'] }, save);
useKeyCommand({ input: ',', modifiers: ['command'] }, openSettings);
useKeyCommand({ input: 'f', modifiers: ['control', 'command'] }, toggleFullScreen);
```

Also implement a visible focus ring so keyboard-only navigation is possible:

```jsx
<Pressable
  onFocus={() => setFocused(true)} onBlur={() => setFocused(false)}
  style={[s.control, focused && { borderColor: mac.focusRing, borderWidth: 3 }]}
/>
```

### Windows, panels, inspectors

Multiple `NSWindow`s and `NSPanel`s require native work. The HIG allows a **split view pane** as an inspector, which is the pragmatic RN answer:

```jsx
// Inspector as a trailing split pane rather than a floating panel.
<View style={{ flexDirection: 'row', flex: 1 }}>
  <Sidebar />
  <Content style={{ flex: 1 }} />
  {inspectorVisible && <Inspector style={{ width: 280 }} />}
</View>
```

Keep nothing critical at the bottom of any of those panes.

### Autosave

```js
// Autosave by default; no unsaved-changes dot while autosaving.
// If you expose a "don't autosave" mode, you must then handle close/quit
// confirmation yourself — which is a strong reason not to expose it.
```

### Mac Catalyst alternative

If your app is primarily iPad, **Mac Catalyst** (running the iPad app on Mac) can be a better path than `react-native-macos` — you keep one codebase, and the iPadOS work in [ipados.md](ipados.md) (pointer, keyboard, resizable windows) is most of what Catalyst needs. The tradeoff is that Catalyst apps are recognizably iPad-derived: no real menu bar richness, no `NSPanel`, and denser Mac idioms are unavailable. → [technologies/system-integration.md](../technologies/system-integration.md)

## Review checklist

- [ ] Every command exists in the menu bar; every toolbar item has a menu equivalent.
- [ ] Standard menus present in the standard order, including View and Window even when minimal.
- [ ] Menu items always visible and disabled when unavailable — never hidden.
- [ ] Show/hide menu item titles reflect current state.
- [ ] Standard keyboard shortcuts supported (⌘Z, ⇧⌘Z, ⌘S, ⌘,, ⌃⌘F) and shown in their menu items.
- [ ] App-specific menus mirror the app hierarchy, ordered general → specialized.
- [ ] Dynamic menu items use one modifier and are never the only path.
- [ ] Menu bar extra uses a symbol, shows a menu not a popover, is user-enabled, and its functionality exists elsewhere.
- [ ] AppKit semantic colors used, including the non-key-window selection variants.
- [ ] Desktop tinting participated in via slight transparency on neutral custom backgrounds.
- [ ] Fixed macOS type scale used — no Dynamic Type machinery; `headline` is Bold.
- [ ] Control sizes ≥ 20 × 20 pt, defaulting to 28 × 28 pt.
- [ ] Hover feedback and tooltips present; tooltips verb-first, ≤ 75 chars, no control name.
- [ ] Visible keyboard focus ring; Full Keyboard Access order is logical.
- [ ] Nothing critical at the bottom of windows, sidebars, or panes.
- [ ] No content under the camera housing; system full-screen support used.
- [ ] Switches/checkboxes/radios and square/image/help buttons in the window body, not the frame.
- [ ] One help button per window, in the corner opposite the dismissal buttons.
- [ ] Push buttons that open another window/view/app end in an ellipsis.
- [ ] Settings in the App menu with ⌘,, stable non-customizable toolbar, last pane restored, minimize/maximize dimmed.
- [ ] Sidebar auto-collapses on window shrink; sidebar icons follow the system accent color.
- [ ] Split views use the 1 pt thin divider with sensible pane min/max sizes.
- [ ] Autosave on, with no unsaved-changes indicator; save dialogs only if autosave is disabled.
- [ ] Sheets allow other windows to come forward; panels used for repeated input + observation.
- [ ] Caution symbols only for unexpected data loss.
- [ ] Signed with Developer ID, sandboxed, no assumptions about the current user.
