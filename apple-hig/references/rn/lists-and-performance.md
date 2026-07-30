# Lists and performance in React Native

Design guidance: [components/layout-and-organization.md](../components/layout-and-organization.md).
This page is about implementation.

Lists are where HIG requirements and RN performance advice actively contradict each other. Getting
that resolution right is most of this page.

## Contents

- [The Dynamic Type vs. `getItemLayout` conflict](#the-dynamic-type-vs-getitemlayout-conflict)
- [Choosing a list component](#choosing-a-list-component)
- [Building a grouped list](#building-a-grouped-list)
- [Row content and press states](#row-content-and-press-states)
- [Swipe actions](#swipe-actions)
- [Images in rows](#images-in-rows)
- [Accessibility](#accessibility)
- [Review checklist](#review-checklist)

## The Dynamic Type vs. `getItemLayout` conflict

This is the trap. The HIG requires rows to grow with Dynamic Type, which means **`minHeight`, never
`height`**. Every FlatList performance guide tells you to pass `getItemLayout` — which requires
knowing each row's height in advance. The two are incompatible, and the usual resolution is silently
wrong: a fixed height that truncates text at accessibility sizes.

```jsx
// WRONG — fast, and broken at large Dynamic Type sizes.
const ROW = 56;
<FlatList getItemLayout={(_, i) => ({ length: ROW, offset: ROW * i, index: i })}
          renderItem={() => <View style={{ height: ROW }} />} />
```

Resolve it in this order:

1. **Use FlashList v2.** It measures rendered items, so it needs no size estimates at all — v2
   removed `estimatedItemSize`, `estimatedListSize`, and `estimatedFirstItemOffset` entirely, and
   `overrideItemLayout` now only sets `span`. Variable-height rows are the supported case, not a
   workaround. This makes it the correct default for any list containing text.

   ```jsx
   import { FlashList } from '@shopify/flash-list';

   <FlashList
     data={items}
     renderItem={({ item }) => <Row item={item} />}   // minHeight inside; no fixed height anywhere
     keyExtractor={i => i.id}
     ItemSeparatorComponent={Separator}
   />
   ```

2. **If you must stay on FlatList, drop `getItemLayout`** rather than fixing the height. Correct
   layout at AX5 matters more than scroll-position precision. Keep `windowSize`, `removeClippedSubviews`,
   and a memoized `renderItem`.

3. **Only fix a height when the row provably contains no text** — a color swatch grid, a photo grid.
   Then `getItemLayout` is safe.

If you need both, recompute the row height from the current text scale rather than hard-coding it:

```jsx
const { fontScale } = useWindowDimensions();   // re-renders on text-size change
const rowHeight = Math.max(44, Math.ceil(56 * fontScale));
```

## Choosing a list component

| Need | Use |
|---|---|
| Any list with text rows | `FlashList` (v2) |
| Long list, fixed-size non-text cells | `FlatList` + `getItemLayout`, or `FlashList` |
| Sectioned/grouped list | `FlashList` with a flattened data array, or `SectionList` |
| Short, known-small list (a settings group) | Plain `View`s in a `ScrollView` — virtualization costs more than it saves |
| Grid | `FlashList` with `numColumns`; `masonry` prop for ragged grids |

`MasonryFlashList` is gone in v2 — it's the `masonry` prop on `FlashList` now.

## Building a grouped list

The inset-grouped look is the iOS settings convention: a grouped page background, rows in an elevated
card, separators inset to the text.

```jsx
const s = StyleSheet.create({
  page: { flex: 1, backgroundColor: PlatformColor('systemGroupedBackground') },
  card: {
    backgroundColor: PlatformColor('secondarySystemGroupedBackground'),
    borderRadius: 10,
    marginHorizontal: 16,
    overflow: 'hidden',
  },
  row: {
    minHeight: 44,                 // not height
    paddingVertical: 11,
    paddingHorizontal: 16,
    flexDirection: 'row',
    alignItems: 'center',
    gap: 12,
  },
  separator: {
    height: StyleSheet.hairlineWidth,
    backgroundColor: PlatformColor('separator'),
    marginStart: 16,               // logical property, inset to the label
  },
});
```

Don't mix the grouped background set with the plain system background set inside one screen — pick
one. → [foundations/color-and-materials.md](../foundations/color-and-materials.md)

## Row content and press states

`TouchableOpacity`'s opacity fade is an Android/legacy idiom. iOS rows highlight with a solid fill.

```jsx
<Pressable
  onPress={onPress}
  accessibilityRole="button"
  style={({ pressed }) => [s.row, pressed && { backgroundColor: PlatformColor('systemGray5') }]}
>
  <Text style={{ fontSize: 17, lineHeight: 22, color: PlatformColor('label'), flexShrink: 1 }}>
    {label}
  </Text>
  <View style={{ flex: 1 }} />
  <SymbolView name="chevron.right" size={15} tintColor={PlatformColor('tertiaryLabel')} />
</Pressable>
```

`flexShrink: 1` on the label matters: without it, a long label at a large text size pushes the
chevron off the row instead of wrapping.

## Swipe actions

Use `react-native-gesture-handler`'s `Swipeable`, or `ReanimatedSwipeable` for the current API. Two
HIG rules that are easy to miss: a swipe action must have a non-gesture equivalent (a context menu or
a row in an edit mode), and a destructive swipe action needs undo rather than a confirmation.
→ [rn/gestures.md](gestures.md), [patterns/feedback-and-loading.md](../patterns/feedback-and-loading.md)

## Images in rows

`expo-image` over RN's `Image` for lists: it has memory/disk caching, `placeholder` for a blur-up,
and `recyclingKey`, which prevents the wrong image flashing into a recycled row.

```jsx
<Image
  source={item.thumb}
  recyclingKey={item.id}          // essential in a recycled list
  placeholder={item.blurhash}
  contentFit="cover"
  transition={150}
  style={{ width: 44, height: 44, borderRadius: 8 }}
/>
```

Request images at the size you display them at, times the screen scale. On tvOS, request them at the
**focused** (enlarged) size or they soften when a card scales up.

## Accessibility

A row is one focus stop, not four. Group it, and give it a single composed label:

```jsx
<Pressable
  accessible
  accessibilityRole="button"
  accessibilityLabel={`${title}, ${subtitle}`}
  accessibilityHint="Opens the episode"
>
```

Decorative thumbnails get `accessibilityElementsHidden` and
`importantForAccessibility="no-hide-descendants"`. A row's trailing chevron is decorative — never
labeled.

## Review checklist

- [ ] No fixed `height` on any row containing text; `minHeight` used.
- [ ] `getItemLayout` absent unless rows are provably text-free.
- [ ] `FlashList` v2 used for text lists; no leftover `estimatedItemSize` props.
- [ ] Labels have `flexShrink: 1` so they wrap instead of displacing trailing accessories.
- [ ] Layout verified at the largest accessibility text size.
- [ ] Separators are `StyleSheet.hairlineWidth`, `PlatformColor('separator')`, inset with `marginStart`.
- [ ] One background set per screen — grouped or plain, not both.
- [ ] Press state is a solid fill, not an opacity fade.
- [ ] Every swipe action has a non-gesture equivalent; destructive ones offer undo.
- [ ] `recyclingKey` set on images inside recycled rows.
- [ ] Each row is a single focus stop with a composed label; decorative images hidden.
- [ ] Virtualization not used for short static groups.
