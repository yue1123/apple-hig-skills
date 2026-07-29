# Lists, Tables, Collections, Split Views & Labels

Source: HIG › Components › Layout and organization — [Lists and tables](https://developer.apple.com/design/human-interface-guidelines/lists-and-tables), [Collections](https://developer.apple.com/design/human-interface-guidelines/collections), [Split views](https://developer.apple.com/design/human-interface-guidelines/split-views), [Tab views](https://developer.apple.com/design/human-interface-guidelines/tab-views), [Disclosure controls](https://developer.apple.com/design/human-interface-guidelines/disclosure-controls), [Labels](https://developer.apple.com/design/human-interface-guidelines/labels), [Boxes](https://developer.apple.com/design/human-interface-guidelines/boxes), [Lockups](https://developer.apple.com/design/human-interface-guidelines/lockups), [Outline views](https://developer.apple.com/design/human-interface-guidelines/outline-views), [Column views](https://developer.apple.com/design/human-interface-guidelines/column-views)

## Contents

- [List/table vs. collection](#listtable-vs-collection)
- [Lists and tables](#lists-and-tables)
- [Collections](#collections)
- [Split views](#split-views)
- [Tab views](#tab-views)
- [Disclosure controls](#disclosure-controls)
- [Labels and boxes](#labels-and-boxes)
- [Lockups (tvOS)](#lockups-tvos)
- [Outline and column views (macOS)](#outline-and-column-views-macos)
- [React Native mapping](#react-native-mapping)
- [Review checklist](#review-checklist)

## List/table vs. collection

**Prefer a list or table for text.** The row format makes text easy to scan and read, and it's simpler and more efficient to digest.

**Use a collection when items vary widely in size**, or when you need to display a large number of images.

## Lists and tables

**Let people edit when it makes sense.** Reordering is appreciated even where adding and removing aren't possible. On iOS/iPadOS, people must enter an **edit mode** before selecting table items.

**Match selection feedback to what selection means:**

- **Navigation** — **persistently** highlight the selected row, so the path being taken stays clear.
- **Option selection** — highlight **briefly**, then add a checkmark or similar image to mark the choice.

Using persistent highlight for options, or a transient flash for navigation, both leave people unsure what state they're in.

**Keep item text succinct** to minimize truncation and wrapping. If items carry a lot of text, show **titles only** and reveal content in a detail view rather than growing rows.

**Preserve readability when text is clipped.** In a narrow or resizable table, a **middle ellipsis** often distinguishes items better than end truncation, because it keeps both the beginning and the end.

**Use descriptive column headings** in multicolumn tables — nouns or short noun phrases, **title-style capitalization, no ending punctuation**. A single-column table with no heading needs a label or header for context.

**Choose a style that suits the data and platform.** iOS/iPadOS **grouped** style uses headers, footers, and extra space to separate groups; watchOS **elliptical** style makes items appear to roll off a curved surface; macOS **bordered** style uses alternating row backgrounds to make large tables tractable.

**Choose a row style that fits the content** — a small leading image with a brief label, for instance.

### iOS / iPadOS / visionOS

**Info button vs. disclosure indicator** — these are not interchangeable. An **info button** (a *detail disclosure button* in a row) reveals **more information about the row's content**. A **disclosure indicator** navigates into the row's **subviews**. Using an info button for navigation, or vice versa, misrepresents what will happen.

**Don't add an index to a table with trailing-edge row controls.** Both the index and disclosure indicators live on the trailing side, so people can't use one without hitting the other.

### macOS

**Let people click a column heading to sort** by that column, and clicking an already-sorted heading **reverses** the direction.

**Let people resize columns** — table data varies in width, and resizing is how people reveal clipped values.

## Collections

**Use the standard row or grid layout wherever possible.** Custom layouts risk confusing people and drawing attention to themselves rather than the content.

**Use a table instead for text.**

**Make items easy to choose.** Adequate padding around images keeps focus and hover effects visible and prevents content overlap. If items are hard to reach, people lose interest before getting to the content.

**Add custom gestures only when necessary.** Tap to select, touch and hold to edit, and swipe to scroll come by default.

**Consider animations for insert, delete, and reorder** — standard animations exist, and custom ones are possible.

**iOS/iPadOS: be careful with dynamic layout changes.** Avoid changing the layout while people are viewing and interacting with it unless it's a response to an explicit action.

## Split views

**Persistently highlight the current selection in each pane that leads to the detail view.** That's what keeps the relationship between panes legible and people oriented.

**Consider drag and drop between panes** — a split view exposes multiple hierarchy levels at once, which makes dragging between them a natural way to move content.

### Platform notes

- **iOS** — prefer split views in a **regular**, not compact, environment. In compact width (iPhone portrait) multiple panes wrap or truncate, hurting legibility and interaction.
- **iPadOS** — **account for narrow, compact, and intermediate widths**, since windows resize fluidly. Ensure it stays possible to navigate between panes logically at every width.
- **macOS** — set sensible **minimum and maximum pane sizes** so the divider stays visible; a pane that gets too small makes the divider seem to vanish. **Let people hide a pane** where it helps (Keynote hides navigator and presenter notes for slide editing), and **provide multiple ways to reveal it again** — a toolbar button and a menu command with a keyboard shortcut. **Prefer the thin (1 pt) divider** for maximum content space; use a thicker one only when both sides have strong linear elements that would obscure a thin divider.
- **tvOS** — keep panes balanced: the default is one-third primary / two-thirds secondary, or specify half-and-half. **Display a single title above the split view**, describing the content as a whole — people already know how split views work and don't need per-pane titles.

## Tab views

Distinct from a **tab bar**: a tab view is an enclosed control for switching among closely related panes, typically inside a window.

**Present closely related areas of content.** The enclosure implies the panes are similar or related.

**Panes must be fully self-contained** — controls in a pane affect only that pane, since panes are mutually exclusive.

**Label each tab to describe its pane's contents** — nouns or short noun phrases (a verb phrase can work), **title-style capitalization**. A good label lets people predict a pane's contents before selecting it.

**Don't use a pop-up button to switch tabs.** A tabbed control takes one click and shows all choices at once; a pop-up button takes two and hides them. (A pop-up button *is* reasonable when there are too many panes for tabs.)

**No more than six tabs.** Beyond that it's overwhelming and creates layout problems — switch to a pop-up button menu of view options.

**Inset the tab view with a margin of window body on all sides.** This looks clean and leaves room for controls unrelated to the tab contents. Extending to the window edges is unusual.

**watchOS:** tab views render as **pages**; stacked vertically, the Digital Crown moves between them. See [presentation.md](presentation.md).

## Disclosure controls

**Hide details until they're relevant.** Put the controls people are most likely to use at the **top of the hierarchy**, always visible, with advanced functionality hidden by default.

**Give a disclosure triangle a descriptive label** indicating what's revealed — "Advanced Options".

**Place a disclosure button near the content it controls**, so the relationship is unambiguous.

**No more than one disclosure button per view.** Multiple ones add complexity and confuse.

## Labels and boxes

### Labels

**For a small amount of non-editable text.** Editable small text → text field. Large text (optionally editable) → text view.

**Prefer system fonts.** Labels support Dynamic Type by default; custom fonts and style adjustments must stay legible.

**Use the four system label colors** to convey relative importance — see [color-and-materials.md](../foundations/color-and-materials.md).

**Make useful label text selectable** — error messages, locations, IP addresses. People want to copy these.

### Boxes

**Keep a box relatively small compared to its container.** As a box approaches the window or screen size it stops communicating grouping and starts crowding other content.

**Use padding and alignment for sub-grouping, not nested boxes.** A box's border is a strong visual element; nesting makes the interface busy and constrained.

**Add a succinct title if it clarifies the contents** — the border already says "these are related", but a title can explain *how*, and it helps **VoiceOver users predict** what's inside.

**Title format:** brief descriptive phrase, **sentence-style capitalization**, no ending punctuation — except in a settings pane, where you append a **colon**.

## Lockups (tvOS)

A lockup groups an image with related text as a single focusable unit.

**Leave adequate space between lockups.** A focused lockup **expands**, so spacing must account for the focused size or lockups will overlap or displace each other.

**Use consistent sizes within a row or group** — matching widths and heights look substantially better.

**Prefer images over initials.** An image of a person creates a more intimate connection than text does.

## Outline and column views (macOS)

### Outline views

**Use a table instead if the data isn't hierarchical.**

**Expose hierarchy in the first column only.** Other columns show attributes of the hierarchical data in the primary column.

**Always provide column headings in a multi-column outline view** — nouns or short noun phrases, title-style capitalization, **no trailing colon**. Single-column views without a heading need a label for context.

**Support sorting by column heading.** Clicking the **primary** column sorts at each hierarchy level (in the Finder, top-level folders sort, then items within each folder). Clicking an already-sorted heading reverses.

**Let people resize columns.**

**Make expanding and collapsing easy.** Clicking a disclosure triangle expands one container; **Option-clicking expands all subfolders**.

**Retain expansion state** across sessions, so people don't have to navigate back to the same place.

**Consider alternating row colors** in multi-column outline views — they help track values across columns, especially in wide views.

**Let people edit where it makes sense.** In an editable cell, a **single click** edits; a **double click** can do something different (single-click renames a file, double-click opens it). Consider allowing reorder, add, and remove.

**Consider a centered ellipsis** rather than clipping, to preserve both ends of cell text.

**Consider a search field in the toolbar** for lengthy outline views.

### Column views

**Show the root level in the first column**, so people can scroll back and restart navigation from the top.

**Show information about the selected item when it has no nested items** — the Finder previews the item plus creation date, modification date, type, and size.

**Let people resize columns**, especially where item names exceed the default width.

## React Native mapping

### Lists

```jsx
import { FlatList, SectionList } from 'react-native';

<FlatList
  data={items}
  keyExtractor={i => i.id}
  renderItem={({ item }) => <Row item={item} />}
  // Grouped style: section headers/footers with separation.
  ItemSeparatorComponent={() => <View style={s.hairline} />}
  // Rows must grow with Dynamic Type — minHeight, never height.
  // See typography.md.
  contentInsetAdjustmentBehavior="automatic"
/>
```

For large or sectioned lists, `FlashList` (Shopify) or `@legendapp/list` handle recycling far better than `FlatList`, which matters at the sizes where the HIG says to use a table over a collection.

Selection feedback per the two-mode rule:

```jsx
// Navigation list: persistent highlight of the selected row.
<Pressable
  style={({ pressed }) => [s.row, isSelected && s.rowSelected, pressed && s.rowPressed]}
  accessibilityRole="button"
  accessibilityState={{ selected: isSelected }}
/>

// Option list: brief highlight, then a checkmark.
<Pressable style={({ pressed }) => [s.row, pressed && s.rowPressed]}>
  <Text>{option.label}</Text>
  {isChosen && <Icon name="checkmark" color={PlatformColor('systemBlue')} />}
</Pressable>
```

Accessory semantics — the distinction that's easy to get wrong:

```jsx
// Disclosure indicator: navigates into subviews.
<Icon name="chevron.right" color={PlatformColor('tertiaryLabel')} />

// Info button: reveals more information about this row. Not navigation.
<Pressable onPress={showDetail} accessibilityLabel="More info">
  <Icon name="info.circle" />
</Pressable>
```

Middle truncation for narrow rows:

```jsx
<Text numberOfLines={1} ellipsizeMode="middle">{item.path}</Text>
```

Swipe actions and reordering, with the accessibility alternatives from [accessibility.md](../foundations/accessibility.md):

```jsx
import ReorderableList from 'react-native-reorderable-list';
// or DraggableFlatList — either way, add accessibilityActions for Move Up/Move Down.
```

### Collections (grids)

```jsx
// Standard grid — resist custom layouts.
<FlatList
  data={photos}
  numColumns={numColumns}                 // derived from width, not hard-coded
  key={numColumns}                        // force relayout when the count changes
  columnWrapperStyle={{ gap: 8 }}
  contentContainerStyle={{ gap: 8, padding: 16 }}   // padding keeps focus/hover effects visible
/>
```

`numColumns` changes require a `key` change or `FlatList` won't relayout — a common bug when handling rotation and iPad resizing.

Animate insert/delete/reorder:

```jsx
import { LinearTransition } from 'react-native-reanimated';
<Animated.FlatList itemLayoutAnimation={LinearTransition} />
```

Disable it under Reduce Motion.

### Split views

```jsx
// Regular width only — collapse to a stack in compact.
const { width } = useWindowDimensions();
const canSplit = width >= 768;

canSplit ? (
  <View style={{ flexDirection: 'row', flex: 1 }}>
    <View style={{ width: sidebarWidth, minWidth: 200, maxWidth: 360 }}>
      <MasterList selectedId={selectedId} onSelect={setSelectedId} />
    </View>
    <View style={s.divider} />{/* 1 pt — the thin divider */}
    <View style={{ flex: 1, minWidth: 320 }}>
      <Detail id={selectedId} />
    </View>
  </View>
) : (
  <Stack.Navigator>{/* master pushes to detail */}</Stack.Navigator>
)
```

The `minWidth`/`maxWidth` pair is the RN form of "reasonable minimum and maximum pane sizes"; without it, dragging a resizable divider can collapse a pane until the divider is unusable.

Keep the master selection highlighted while the detail is showing — in a stack navigator that state is lost on push, so it must be lifted:

```jsx
const [selectedId, setSelectedId] = useState(null);   // owned above both panes
```

`react-native-split-view` and the drawer's `permanent` type both work; for iPad specifically, `UISplitViewController` via a native module gives the correct collapse/expand animation.

### Tab views

RN has no `NSTabView` equivalent. A segmented control above a container is the closest correct analogue on iOS — and per [selection-and-input.md](selection-and-input.md), a segmented control is the right control for switching subviews:

```jsx
<View>
  <SegmentedControl values={['General', 'Advanced']} selectedIndex={i} onChange={…} />
  <View style={{ flex: 1, margin: 16 }}>{/* inset, per the tab view margin rule */}
    {i === 0 ? <General /> : <Advanced />}
  </View>
</View>
```

Keep it to ≤ 6 panes; beyond that use a pop-up button menu.

### Disclosure

```jsx
// One per view; label states what's hidden.
const [expanded, setExpanded] = useState(false);

<Pressable
  onPress={() => setExpanded(e => !e)}
  accessibilityRole="button"
  accessibilityState={{ expanded }}
  accessibilityLabel="Advanced Options"
>
  <Icon name={expanded ? 'chevron.down' : 'chevron.right'} />
  <Text style={textStyles.headline}>Advanced Options</Text>
</Pressable>
{expanded && <AdvancedFields />}
```

`accessibilityState={{ expanded }}` is what makes VoiceOver announce "collapsed"/"expanded" — without it the control is opaque to screen reader users.

Animate the reveal with `LayoutAnimation` or Reanimated's `Layout`, disabled under Reduce Motion.

### Labels and boxes

```jsx
// Selectable label for copyable information.
<Text selectable style={{ color: PlatformColor('secondaryLabel') }}>{ipAddress}</Text>

// Box equivalent: a grouped card with a title that VoiceOver can use.
<View accessibilityRole="summary" accessibilityLabel="Notification settings">
  <Text style={textStyles.footnote}>Notifications</Text>
  <View style={s.card}>{rows}</View>
</View>
```

The iOS grouped-list look is a section header outside an inset card — reach for `secondarySystemGroupedBackground` on the card over `systemBackground`, per [color-and-materials.md](../foundations/color-and-materials.md).

### tvOS lockups

```jsx
// Space for focus growth — a 1.1× focused card needs (0.1 × size / 2) clearance per side.
const CARD_W = 300, FOCUS_SCALE = 1.1;
const gap = Math.ceil((CARD_W * (FOCUS_SCALE - 1)) / 2) + 16;

<FlatList horizontal data={shows} contentContainerStyle={{ gap }}
  renderItem={({ item }) => <Lockup image={item.art} title={item.title} width={CARD_W} />} />
```

Consistent sizes within the row, and prefer real images over initials.

## Review checklist

- [ ] Text-heavy content uses a list, not a grid; size-varied or image-heavy content uses a grid.
- [ ] Rows use `minHeight` and grow with Dynamic Type; no fixed heights.
- [ ] Navigation lists highlight the selection persistently; option lists flash then checkmark.
- [ ] Disclosure indicators used for navigation, info buttons only for more information about the row.
- [ ] No index on a list with trailing-edge row controls.
- [ ] Narrow rows use middle truncation where both ends matter.
- [ ] Multicolumn tables have noun column headings, title case, no punctuation.
- [ ] Grouped-style lists use the grouped background colors, not the system set.
- [ ] Swipe actions and drag-reorder have accessibility-action equivalents.
- [ ] Grids use a standard row/grid layout with adequate padding around items.
- [ ] `numColumns` derived from width, with a `key` change so relayout happens on resize.
- [ ] Insert/delete/reorder animations present, and disabled under Reduce Motion.
- [ ] Split views used only in regular width; collapse to a stack in compact.
- [ ] Selected master item stays highlighted while the detail shows; selection state owned above both panes.
- [ ] Panes have sensible min/max widths so the divider stays usable; thin divider preferred.
- [ ] macOS/iPad: hidden panes revealable by more than one route.
- [ ] tvOS: single title above the split view; balanced pane proportions.
- [ ] Tab views hold ≤ 6 self-contained panes with noun labels and an inset margin; no pop-up button used to switch tabs.
- [ ] At most one disclosure control per view, labelled with what it reveals, with `accessibilityState.expanded` set.
- [ ] Labels use system label colors for hierarchy; informational labels are selectable.
- [ ] Boxes stay small relative to their container, use padding not nesting for sub-grouping, and carry a title where it aids VoiceOver.
- [ ] tvOS lockups sized consistently with spacing that accounts for focus growth; images preferred over initials.
- [ ] macOS outline views: hierarchy in column one, sortable headings, resizable columns, expansion state retained.
