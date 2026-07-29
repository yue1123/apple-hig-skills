# Buttons, Toolbars, Menus & Actions

Source: HIG › Components › Menus and actions — [Buttons](https://developer.apple.com/design/human-interface-guidelines/buttons), [Toolbars](https://developer.apple.com/design/human-interface-guidelines/toolbars), [Menus](https://developer.apple.com/design/human-interface-guidelines/menus), [Context menus](https://developer.apple.com/design/human-interface-guidelines/context-menus), [Pull-down buttons](https://developer.apple.com/design/human-interface-guidelines/pull-down-buttons), [Pop-up buttons](https://developer.apple.com/design/human-interface-guidelines/pop-up-buttons), [Edit menus](https://developer.apple.com/design/human-interface-guidelines/edit-menus), [Activity views](https://developer.apple.com/design/human-interface-guidelines/activity-views), [Ornaments](https://developer.apple.com/design/human-interface-guidelines/ornaments), [Home Screen quick actions](https://developer.apple.com/design/human-interface-guidelines/home-screen-quick-actions), [Dock menus](https://developer.apple.com/design/human-interface-guidelines/dock-menus)

The macOS menu bar has its own section in [platforms/macos.md](../platforms/macos.md).

## Contents

- [Buttons](#buttons)
- [Toolbars](#toolbars)
- [Menus](#menus)
- [Context menus](#context-menus)
- [Pull-down vs. pop-up buttons](#pull-down-vs-pop-up-buttons)
- [Edit menus](#edit-menus)
- [Activity views](#activity-views)
- [Ornaments (visionOS)](#ornaments-visionos)
- [Quick actions and Dock menus](#quick-actions-and-dock-menus)
- [React Native mapping](#react-native-mapping)
- [Review checklist](#review-checklist)

## Buttons

A button combines three attributes: **style** (size, color, shape), **content** (symbol, label, or both), and **role** (semantic meaning that can affect appearance).

**Hit region ≥ 44×44 pt** — 60×60 pt on visionOS — regardless of glyph size. Include enough surrounding space that people can visually distinguish the button from neighbors.

**Always include a press state on a custom button.** Without one, the button feels unresponsive and people wonder whether their input registered.

### Style

**Use a prominent style for the most likely action in a view**, so the system applies the accent color to its background. **Keep prominent buttons to one or two per view** — more increases cognitive load, because people have to weigh options instead of recognizing the obvious one.

**Distinguish the preferred option by style, not size.** Same-size buttons signal a coherent set of choices; differently-sized ones look inconsistent. To highlight the preferred option, give it a more prominent *style* and the rest a less prominent one.

**Don't apply a color similar to your content-layer background to button labels.** If the content layer is bright and colorful, use the default monochromatic label appearance.

### Content

**Associate familiar actions with familiar symbols.** People can predict that `square.and.arrow.up` shares. See [icons-and-images.md](../foundations/icons-and-images.md) for the standard action symbols.

**Use text when a short label is clearer than an icon.** A few words, **title-style capitalization**, starting with a verb — "Add to Cart".

On macOS and visionOS the system shows a tooltip on hover, so a symbol-only button is more defensible there than on touch platforms.

### Role

| Role | Meaning |
|---|---|
| **Normal** | No specific meaning |
| **Primary** | The default button — the one people are most likely to choose |
| **Cancel** | Cancels the current action |
| **Destructive** | Can destroy data |

**Assign primary to the most likely choice.** A primary button responds to **Return**, and in a temporary view (sheet, editable view, alert) that also lets the view close automatically on Return.

**Never make a destructive action primary, even when it's the most likely choice.** Visual prominence causes people to press buttons before reading them; keeping primary non-destructive is what prevents that from costing data.

## Toolbars

**Choose items deliberately; don't overcrowd.** Define which items move to the overflow menu as the toolbar narrows.

**Add a More menu only if you really need it.** Try to fit all actions in the toolbar first, and put the least important ones in More.

**Let people customize the toolbar on iPadOS and macOS**, especially in apps with many items, advanced functionality not everyone needs, or long usage sessions.

**Reduce toolbar backgrounds and tinted controls.** Custom backgrounds interfere with the system's background effects. Let the **content layer** inform the toolbar's color and appearance, and use a **scroll edge effect** to separate toolbar from content when needed. A solid toolbar background is the pre-Liquid-Glass idiom.

**Prefer standard components in a toolbar.** Standard buttons, text fields, headers, and footers get corner radii **concentric with the bar's corners**. A custom component must match that concentricity or it will read as pasted on.

**Prefer system symbols without borders.** The section already provides a visible container, and the system defines hover and selection appearances. Outlined-circle symbols are redundant.

**Use `.prominent` for one key action** like Done or Submit, placed on the **trailing** side. Only one.

**Group logically by function and frequency, and keep it to about three groups maximum.** Put navigation controls and critical actions (Done, Close, Save) in dedicated, visually distinct sections. Keep groupings and placement consistent **across platforms**.

**Separate text-labeled actions with fixed space.** A text button next to a symbol button reads as one combined item; two adjacent text buttons run together. Insert explicit fixed space.

### Titles

**Give each window a useful title** — it confirms location and distinguishes multiple windows. Leave the title area empty if a title would be redundant (Notes doesn't title a single note, because the first line supplies context; but *does* title separate note windows so they're distinguishable).

**Never title a window with your app name** — it says nothing about the content hierarchy.

**Keep titles under about 15 characters** so there's room for controls.

**Use the standard Back and Close buttons with standard symbols** — not text labels reading "Back" or "Close". A custom version must look the same, behave the same, and be used consistently.

### Platform notes

- **iOS** — space is tightest; prioritize ruthlessly and use a More menu. **Large titles** transition to standard titles on scroll and back at the top, which is what keeps people oriented.
- **iPadOS** — a toolbar and a tab bar can share the same horizontal space at the top, useful when you want top-level navigation without giving up content width.
- **macOS** — **every toolbar item must also exist as a menu bar command**, because the toolbar is customizable and hideable. The reverse isn't true: not every menu command deserves toolbar space.

## Menus

**Label each item clearly and succinctly**, using a **verb or verb phrase** for actions. **Title-style capitalization.** **Drop articles** (*a*, *an*, *the*) — they lengthen labels without adding clarity.

**Append an ellipsis (…)** when the action needs more information before completing — it signals additional input or choices are coming.

**Dim unavailable items rather than hiding them** (regular menus only — context menus differ). **Keep a menu itself available even when all its items are unavailable**, so people can open it and learn what's there.

**Use standard icons for common actions.** Use icons **sparingly and purposefully** — for the most common actions, key features, file system locations, connected devices, visual concepts like rotate/flip, and user content like folders. **Don't use an icon you can't find a clear one for.** And apply a **uniform treatment within a group**: icons for all items in the group, or none.

**List important or frequent items first** — people scan from the top.

**Group logically related commands with separators**, and keep related commands together **even when their importance differs**. Paste and Match Style is used far less than Paste, but belongs in the same group.

**Be mindful of length.** Long menus mean people miss the command they wanted; consider splitting into separate menus. The exception is user-defined or dynamically generated content (Safari's History and Bookmarks) — people expect those to grow, and scrolling is fine.

**Use submenus sparingly, one level deep.** A reasonable trigger: a term appearing in more than two items of the same group — replace "Sort by Date / Sort by Score / Sort by Time" with a "Sort by" submenu containing Date, Score, Time. Keep the repeated term in the parent label so people can predict the contents. **If a submenu holds more than about five items, make it a new menu.** And prefer a submenu over **indenting** items, which is inconsistent with the system and doesn't express relationships clearly.

### Toggled items

**Use a changeable label** — one item whose label flips between "Show Map" and "Hide Map" — rather than two items.

**Add a verb if the changeable label is ambiguous.** "HDR On" / "HDR Off" could be a state or an action; "Turn HDR On" / "Turn HDR Off" can't be misread.

**Show both items instead when seeing both states helps** — a game listing both "Take Account Online" and "Take Account Offline", with only the applicable one enabled.

**Use a checkmark for an attribute currently in effect** — easy to scan for in a list of attributes.

**Consider an item that clears multiple toggled attributes at once** — a "Plain" item that removes all formatting.

### Platform notes

- **iOS/iPadOS** — small and medium menu layouts streamline choices. Medium suits three important frequent actions (Notes: Scan, Lock, Pin). Small suits closely related grouped actions (Bold, Italic, Underline, Strikethrough) — each needs a recognizable symbol, since there's no label.
- **visionOS** — display a menu **near the content it controls**, since people must look at an item to tap it and may miss a distant effect. Prefer the **subtle** breakthrough effect (the default under `automatic` when overlapping 3D content); `prominent` can disrupt and cause discomfort; `none` fully occludes the menu behind 3D content.

## Context menus

**Prioritize relevance, not advanced or rare items.** A context menu is for the commands most likely needed right now. Mail's inbox context menu has reply and move — not message editing, mailbox management, or filtering.

**Keep it short.** Long context menus are hard to scan and scroll.

**Be consistent throughout the app.** Providing them in some places and not others makes people think something is broken.

**Every context menu item must also exist in the main interface.** A context menu is a shortcut, never the only path.

**Hide unavailable items — don't dim them.** This is the opposite of regular menus, because a context menu shows only what's relevant to the current selection. (macOS exception: Cut, Copy, Paste may appear dimmed.)

**Place frequent items where people look first.** People read from where their finger or pointer opened the menu — and the menu may open above *or* below the content, so you may need to **reverse the item order** to match.

**Don't show keyboard shortcuts** in context menus — the menu is already the shortcut.

**Use separators, but no more than about three groups.**

**Warn about destructive items** on iOS/iPadOS/visionOS: list them **at the end** and mark them destructive so the system can render them in red.

**Include a title only if it clarifies the effect** — e.g. stating the number of selected messages, so people remember the command applies to all of them.

### Platform notes

- **iOS/iPadOS** — **provide either a context menu or an edit menu for an item, never both**; both confuses people and makes intent hard for the system to detect. Consider a context menu for **creating** a new object (Files creates a folder from a long press on empty space). Prefer a **graphical preview** that shows a condensed version of the actual content, so people can confirm the target — and **match the preview's clipping path to the image shape** so corners don't visibly change during the emerge animation.
- **visionOS** — consider a context menu **instead of a panel or inspector window** to keep someone's space uncluttered. Avoid exceeding the window height, since system controls sit just above and below the window edges.

## Pull-down vs. pop-up buttons

The distinction is worth getting right because it's about **actions vs. state**:

| | Pull-down button | Pop-up button |
|---|---|---|
| Contains | Commands or items related to the button's action | A flat list of **mutually exclusive** options or states |
| Label | Stays constant | **Updates to show the current selection** |
| Use for | Doing several related things | Choosing one thing |

### Pull-down

**Don't put all of a view's actions in one pull-down.** Primary actions must be discoverable without opening anything.

**Aim for at least three items.** With one or two, use plain buttons or toggles instead — the interaction cost isn't worth it. Too many items slows people down.

**Show a menu title only if it adds meaning.** Usually the button content plus descriptive items is enough.

**Flag destructive items and confirm them.** The system renders them red, and choosing one presents an action sheet (iOS) or popover (iPadOS) for confirmation. That confirmation appears in a **different location** and requires deliberate dismissal, which is what protects against a mis-tap.

**Add an icon after a label when it clarifies meaning** — SF Symbols stay aligned with text at every scale.

**iOS/iPadOS More button:** useful where space is constrained, but the ellipsis doesn't help people predict contents. Weigh convenience against discoverability.

### Pop-up

**Provide a useful default selection** — whatever most people are likely to want.

**Let people predict the options without opening it** — an introductory label, or a button label describing the effect.

**Good when space is limited** and you don't need all options visible always.

**Consider a Custom option** for occasionally-needed items, with explanatory text below the list if the options need clarifying.

**iPadOS:** inside a popover or modal, a pop-up button can replace a disclosure indicator for a small well-defined option set — people choose without navigating to a detail view.

## Edit menus

**Prefer the system edit menu.** Recreating the same commands is redundant and confusing.

**Let people reveal it with the system interactions they know** — touch and hold, pinch and hold in visionOS, secondary click with a trackpad or keyboard. Don't invent an interaction for a standard task.

**Show only contextually relevant commands.** No Copy or Cut with nothing selected; no Paste with nothing to paste.

**List custom commands near the related system ones** — custom formatting commands after the system formatting section. Don't overwhelm with custom commands.

**Let people select and copy non-editable text** where it makes sense — an image caption, a status line. Copy *content* text, not control labels.

**Support undo and redo** — an edit menu performs actions without confirmation, so undo is the recovery path.

**Don't duplicate edit menu functions with other controls.** Redundant controls crowd the interface and cost space that could introduce less familiar actions.

**Differentiate deletion commands.** Delete behaves like the Delete key; **Cut copies to the pasteboard first**.

**iOS/iPadOS:** the menu has two styles — **compact horizontal** when revealed by Multi-Touch, **vertical** when revealed by keyboard or pointer. Both must work. You can't change the menu's shape or pointer, but you **can** reposition it to avoid covering important content.

## Activity views

**Don't duplicate actions the activity view already provides.** A second Print action is confusing because people can't tell yours from the system's. If you need something similar, give it a distinct title — "Print Transaction".

**Represent custom activities with a symbol.** A custom interface icon should be centered in about **70×70 px**.

**Write succinct titles** — a verb or brief verb phrase; long titles wrap and truncate. **Don't include your company or product name** in an action title. (The share sheet does show a share activity's title — typically a company name — under its icon; that's a different slot.)

**Exclude inapplicable activities.** You can't reorder system tasks, but you can remove ones that don't apply (Print, if your app can't print) and control which custom tasks appear.

**Use the Share button to present it.** People expect the Share button to show system activities; providing an alternative path to the same thing confuses.

**Extensions:** prefer the system composition view for share extensions. Include your app name for action extensions, and include elements of your app's interface so people connect the extension to the app. **Streamline to a few steps.** **Don't stack a modal view above your extension** (an alert may be necessary; nothing more). For long operations, **continue in the background and report status in your main app** — notify about problems, not about mere completion.

## Ornaments (visionOS)

An ornament presents frequently needed controls in a consistent location outside the window, so it doesn't clutter the content. Music uses one for Now Playing controls.

**Generally keep ornaments visible.** Hiding makes sense when people dive into content — a video, a photo — but usually consistent access is what people want.

**Prioritize visual balance if you have several.** Constrain the total number; ornaments add visual weight and can make the app feel complicated. If you remove one, relocate its elements into the window.

**Keep an ornament's width equal to or narrower than its window**, or it interferes with a tab bar or other side content.

**Use borderless buttons.** The ornament's glass background usually makes a border unnecessary, and the system applies the hover effect automatically.

**Toolbars and tab bars already appear as ornaments** in visionOS — don't build an ornament to recreate them.

## Quick actions and Dock menus

**Home Screen quick actions:** create them for compelling high-value tasks. **You get four**, and people expect at least one from every app. Dynamic actions are good — based on location, recent activity, time of day, settings — but they must change **predictably**. Write a **succinct title** that communicates the result ("Directions Home", "New Message"), with an optional subtitle for context. **No app name or extraneous text**; keep it short against truncation and localization. Use **SF Symbols** — **never an emoji**, because emoji are full-color while quick action symbols are monochromatic and adapt in Dark Mode.

**Dock menus (macOS):** make every custom Dock menu item available elsewhere too, since not everyone uses them. Prefer high-value items — currently or recently open windows (a fast way to jump to a specific one), plus a few actions useful when the app isn't frontmost or has no open windows (Mail offers get-mail and compose).

## React Native mapping

### Buttons

`<Button>` in RN is intentionally minimal and not HIG-shaped. Build on `Pressable`:

```jsx
// Roles map to style + behavior, not just color.
const ROLE = {
  primary:     { bg: PlatformColor('systemBlue'), fg: '#fff' },
  normal:      { bg: PlatformColor('tertiarySystemFill'), fg: PlatformColor('label') },
  cancel:      { bg: 'transparent', fg: PlatformColor('systemBlue') },
  destructive: { bg: 'transparent', fg: PlatformColor('systemRed') },
};

function Button({ title, role = 'normal', icon, onPress, loading, disabled }) {
  const c = ROLE[role];
  return (
    <Pressable
      onPress={onPress}
      disabled={disabled || loading}
      accessibilityRole="button"
      accessibilityLabel={title}
      accessibilityState={{ disabled: disabled || loading, busy: loading }}
      hitSlop={8}
      // Press state is mandatory for custom buttons.
      style={({ pressed }) => [
        s.base,
        { backgroundColor: c.bg, opacity: pressed ? 0.6 : 1 },
        disabled && { opacity: 0.4 },
      ]}
    >
      {loading && <ActivityIndicator color={c.fg} />}
      {!loading && icon && <Icon name={icon} color={c.fg} />}
      <Text style={[textStyles.body, { color: c.fg }]}>
        {loading ? 'Checking out…' : title}{/* label change during activity, per iOS guidance */}
      </Text>
    </Pressable>
  );
}

const s = StyleSheet.create({
  base: {
    minHeight: 44,                      // the hit-region floor
    flexDirection: 'row', alignItems: 'center', justifyContent: 'center', gap: 6,
    paddingHorizontal: 16, borderRadius: 12,
  },
});
```

Two rules worth encoding structurally: only one `primary` per screen, and `destructive` never uses the primary style. A lint rule or a review habit is cheaper than the data loss.

Enter/Return should activate the primary button where a keyboard exists (iPad, macOS) — that's part of what "primary" means.

### Toolbars and headers

Use the native header rather than a custom `<View>`, so you get large titles, scroll-edge behavior, and the real Back button:

```jsx
<Stack.Screen
  name="Notes"
  options={{
    headerLargeTitle: true,             // large → standard on scroll, per iOS guidance
    headerTransparent: true,            // content scrolls under; scroll edge effect applies
    headerBlurEffect: 'systemMaterial',
    title: 'Notes',                     // ≤ ~15 chars, never the app name
    headerRight: () => (
      <View style={{ flexDirection: 'row', gap: 16 }}>{/* explicit gap separates items */}
        <ToolbarButton icon="square.and.pencil" label="Compose" />
        <ToolbarButton icon="ellipsis.circle" label="More" />
      </View>
    ),
  }}
/>
```

The `gap` is the RN equivalent of `fixedSpace` — without it, adjacent items visually merge, which is the failure the HIG calls out.

Don't set a `headerStyle.backgroundColor` if you want the Liquid Glass look; `headerTransparent` + `headerBlurEffect` is what lets content inform the bar's appearance.

### Menus and context menus

Use a native menu library so you get the real `UIMenu` — including the preview animation, destructive styling, and correct placement:

```jsx
import { MenuView } from '@react-native-menu/menu';

<MenuView
  actions={[
    { id: 'share',  title: 'Share',      image: 'square.and.arrow.up' },
    { id: 'dup',    title: 'Duplicate',  image: 'plus.square.on.square' },
    // Destructive last, and flagged so the system renders it red.
    { id: 'delete', title: 'Delete', image: 'trash', attributes: { destructive: true } },
  ]}
  onPressAction={({ nativeEvent }) => handle(nativeEvent.event)}
  shouldOpenOnLongPress          // context menu; false → pull-down button
>
  <NoteRow note={note} />
</MenuView>
```

`react-native-context-menu-view` and `expo-menu` (where available) cover the same ground; `zeego` wraps them with a nicer API. What matters is that it's native — a JS-rendered popup menu misses the preview, the haptic, the dismiss behavior, and the placement flip.

Also register every menu action in the main UI:

```jsx
// The same three actions must be reachable without the context menu.
<Toolbar actions={['share', 'duplicate', 'delete']} />
```

### Pull-down vs. pop-up

```jsx
// Pull-down: label constant, ≥3 action items.
<MenuView actions={sortActions} onPressAction={…}>
  <Button title="Actions" icon="ellipsis" />
</MenuView>

// Pop-up: label reflects the current selection.
<MenuView
  actions={SORTS.map(s => ({ id: s.id, title: s.label, state: s.id === sort ? 'on' : 'off' }))}
  onPressAction={({ nativeEvent }) => setSort(nativeEvent.event)}
>
  <Button title={`Sort: ${SORTS.find(s => s.id === sort).label}`} icon="chevron.up.chevron.down" />
</MenuView>
```

The `state: 'on'` is the checkmark — which is how a pop-up communicates current selection inside the menu, complementing the updated button label.

### Edit menus

`<TextInput>` and selectable `<Text>` already get the system edit menu — don't replace it:

```jsx
// Let people copy non-editable content text.
<Text selectable>{post.body}</Text>

// Add custom items rather than a custom menu.
<TextInput
  onSelectionChange={…}
  // iOS 16+: editMenuInteraction / custom UIMenu items need a native module or
  // react-native-ios-context-menu. Add items near the related system section.
/>
```

Don't add your own Copy/Paste buttons alongside a text field — that's the redundant-controls rule.

### Share sheet and activity view

See [patterns/notifications-sharing-files.md](../patterns/notifications-sharing-files.md). Key point here: the trigger is the Share button with `square.and.arrow.up`, and you exclude inapplicable activities rather than adding your own duplicates:

```js
import { Share } from 'react-native';
await Share.share({ url, message }, { excludedActivityTypes: ['com.apple.UIKit.activity.Print'] });
```

### Quick actions

```js
import * as QuickActions from 'expo-quick-actions';

// Max four. Symbols, not emoji. Titles state the result, no app name.
await QuickActions.setItems([
  { id: 'new-message', title: 'New Message', icon: 'symbol:square.and.pencil', params: { href: '/compose' } },
  { id: 'inbox', title: 'Inbox', subtitle: `${unread} unread`, icon: 'symbol:tray', params: { href: '/inbox' } },
]);

// Handle the launch.
QuickActions.addListener(action => router.push(action.params.href));
```

Update dynamically, but predictably — recompute on meaningful state change (unread count, recent activity), not on every render.

### Ornaments and Dock menus

visionOS ornaments come from SwiftUI's `.ornament` modifier; `react-native-visionos` exposes toolbars and tab bars as ornaments automatically, but a custom ornament needs native work. macOS Dock menus require an `NSApplicationDelegate` hook — not available from JS in `react-native-macos`.

## Review checklist

- [ ] Every custom button has a visible press state and a ≥ 44 pt hit region (60 pt visionOS).
- [ ] At most one or two prominent buttons per view; exactly one primary.
- [ ] Destructive actions never styled as primary.
- [ ] Preferred option distinguished by style, not size; sibling buttons share a size.
- [ ] Button labels start with a verb, use title-style capitalization.
- [ ] Standard symbols used for standard actions.
- [ ] Loading buttons show an activity indicator and an updated label.
- [ ] Return activates the primary button where a keyboard exists.
- [ ] Native header used, not a hand-built toolbar; large titles on iOS where appropriate.
- [ ] Toolbar has no custom opaque background; content informs its appearance; scroll edge effect used for separation.
- [ ] Toolbar items grouped into ≤ 3 groups; one trailing prominent action; explicit spacing between text-labeled items.
- [ ] Window titles ≤ ~15 characters and never the app name; standard Back/Close symbols used.
- [ ] macOS: every toolbar item also exists as a menu bar command.
- [ ] Menu item labels are verbs, title-cased, articles dropped; ellipsis where more input follows.
- [ ] Regular menus dim unavailable items; context menus hide them.
- [ ] Menu icons applied to all items in a group or none; no icon used where none clearly fits.
- [ ] Submenus one level deep, ≤ ~5 items, parent label predicts contents; no indentation used for hierarchy.
- [ ] Toggled items use one changeable label (with a verb if ambiguous) or checkmarks.
- [ ] Context menus are short, consistently available, and every item also exists in the main UI.
- [ ] Context menu destructive items are last and flagged destructive.
- [ ] No keyboard shortcuts shown in context menus.
- [ ] Context menus use a native implementation (real `UIMenu`), with a preview whose clipping path matches the image.
- [ ] An item has a context menu or an edit menu, never both.
- [ ] Pull-down buttons hold ≥ 3 action items and keep a constant label; pop-up buttons reflect the current selection and have a sensible default.
- [ ] Destructive pull-down items confirmed via action sheet/popover.
- [ ] System edit menu used; non-editable content text selectable; no duplicate Copy/Paste controls.
- [ ] Share uses the Share button and the system activity view; no duplicated system activities; inapplicable ones excluded.
- [ ] Quick actions ≤ 4, SF Symbols not emoji, titles state the result with no app name, updates predictable.
- [ ] visionOS: ornaments kept narrow and few, borderless buttons, menus placed near the content they control with the subtle breakthrough effect.
