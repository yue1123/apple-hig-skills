# Layout, Adaptivity, Safe Areas & Right-to-Left

Source: HIG › Foundations › [Layout](https://developer.apple.com/design/human-interface-guidelines/layout), [Right to left](https://developer.apple.com/design/human-interface-guidelines/right-to-left)

Read this when structuring a screen, handling rotation or window resizing, dealing with notches/Dynamic Island/home indicator, choosing breakpoints, or supporting Arabic/Hebrew.

## Contents

- [Rules that matter most](#rules-that-matter-most)
- [Visual hierarchy](#visual-hierarchy)
- [Adaptivity](#adaptivity)
- [Safe areas and layout guides](#safe-areas-and-layout-guides)
- [Screen dimensions and size classes](#screen-dimensions-and-size-classes)
- [Platform differences](#platform-differences)
- [Right to left](#right-to-left)
- [React Native mapping](#react-native-mapping)
- [Review checklist](#review-checklist)

## Rules that matter most

**Content extends edge to edge; chrome floats on top of it.** Backgrounds and full-screen artwork reach the display edges. Scrollable content continues to the very bottom and both sides. Since Liquid Glass, sidebars and tab bars sit on a *separate layer above* content rather than beside it — so your scroll content must run underneath them, with inset padding rather than a shortened frame. Getting this wrong produces the two most recognizable "not-native" tells: a dead band of background color under the tab bar, and content that stops short of the screen edge.

**Group related items; give the important ones room.** Use negative space, background shapes, color, materials, or separators to bundle related things — while keeping content and controls clearly distinguishable from each other. Don't crowd the primary information with secondary detail; move that to another part of the view or a detail screen.

**Place by importance, in reading order.** People scan top-to-bottom, leading-to-trailing. Most important content goes top and leading. "Leading" is direction-dependent — see [Right to left](#right-to-left).

**Align things.** Alignment plus indentation is how people perceive organization and hierarchy while their eyes are moving. Misalignment reads as carelessness even when nobody can articulate why.

**Use progressive disclosure for what doesn't fit.** If a collection is too large to show at once, indicate that more exists — a disclosure control, or partially visible items that hint at scrollability. A grid that ends exactly at the fold looks complete when it isn't.

**Space out controls.** Unrelated controls placed too close together, or crowded by content, become hard to tell apart and hard to hit.

## Visual hierarchy

**Differentiate controls from content** using Liquid Glass, which gives controls a consistent appearance across iOS, iPadOS, and macOS.

**Prefer a scroll edge effect over a background** for the transition between content and a control area. The scroll edge effect blurs and fades content as it approaches the chrome, which is what keeps a floating toolbar legible without drawing an opaque bar. A solid background under a toolbar is the old idiom and now looks dated.

**When content doesn't span the full window, use a background extension view** so the content appears to continue behind the control layer — under a sidebar or inspector, for instance — instead of ending at a visible seam.

## Adaptivity

Every app must adapt when the device or system context changes. The variations you're responsible for:

- Screen sizes, resolutions, and color spaces
- Orientation (portrait/landscape)
- System hardware features — Dynamic Island, camera housing, camera controls
- External displays, Display Zoom, and resizable windows on iPad
- Dynamic Type size changes
- Locale: layout direction, date/time/number formats, font variation, and **text length**

**Adapt gracefully while staying recognizably consistent.** When someone rotates, resizes, adds a display, or moves to another device, the experience should still be familiar. Respect system safe areas, margins, and guides rather than reimplementing them.

**Test the extremes first.** Build and check the largest and smallest layouts before the middle ones — bugs cluster at the boundaries, and fixing the extremes usually fixes everything between.

**Scale artwork, never restretch it.** A different aspect ratio may crop, letterbox, or pillarbox your art. Scale it so the important content stays visible; don't change the aspect ratio.

## Safe areas and layout guides

A **layout guide** is a rectangular region for positioning and aligning content — standard margins, readable content width. A **safe area** is the region not covered by bars or system UI, *and* not overlapping hardware features: Dynamic Island, the home indicator, the rounded corners, the Mac camera housing.

Safe areas do two jobs: they keep you clear of hardware, and they reposition content automatically when bar sizes change. Ignoring them is what makes an app feel foreign on the platform.

The important nuance: **respecting the safe area does not mean stopping your background at it.** Backgrounds and scroll content go to the edges; *interactive and readable* content stays inside.

## Screen dimensions and size classes

### Logical widths worth designing against (portrait, points)

| Width | Devices |
|---|---|
| 320 | iPhone SE (4-inch), iPod touch |
| 360 | iPhone 12/13 mini |
| 375 | iPhone SE (4.7-inch), 6–8, X, XS, 11 Pro |
| 390 | iPhone 12/13/14, 16e |
| 393 | iPhone 15, 15 Pro, 16 |
| 402 | iPhone 17, 17 Pro |
| 414 | iPhone 6–8 Plus, XR, XS Max, 11, 11 Pro Max |
| 420 | iPhone Air |
| 428 | iPhone 12/13 Pro Max, 14 Plus |
| 430 | iPhone 14 Pro Max, 15 Plus, 15 Pro Max, 16 Plus |
| 440 | iPhone 16 Pro Max, 17 Pro Max |
| 744 | iPad mini (8.3-inch) |
| 768 | iPad / iPad Air / iPad Pro (9.7-inch) |
| 810 | iPad (10.2-inch) |
| 820 | iPad Air (10.9/11-inch), iPad (11-inch) |
| 834 | iPad Pro (11-inch), iPad Air/Pro (10.5-inch) |
| 1024 | iPad Pro 12.9-inch, iPad Air 13-inch |
| 1032 | iPad Pro 13-inch |

Scale factors are @2x or @3x depending on model — and the *UIKit* scale factor can differ from the native one, so don't derive pixel dimensions by multiplying logical size by an assumed factor.

### watchOS screen sizes (pixels)

| Case | Width × Height |
|---|---|
| 38mm (S1–3) | 272 × 340 |
| 40mm (S4–6, SE) | 324 × 394 |
| 41mm (S7–9) | 352 × 430 |
| 42mm (S10, 11) | 374 × 446 |
| 44mm (S4–6, SE) | 368 × 448 |
| 45mm (S7–9) | 396 × 484 |
| 46mm (S10, 11) | 416 × 496 |
| 49mm (Ultra 1–2) | 410 × 502 |
| 49mm (Ultra 3) | 422 × 514 |

### Size classes

A size class is `regular` or `compact` per axis. The pattern:

- **All iPads: regular × regular**, both orientations, full screen.
- **iPhone portrait: compact width × regular height**, universally.
- **iPhone landscape splits by model.** The large phones (Plus / Max / Air) become **regular width × compact height**; standard and Pro sizes stay **compact × compact**.

That landscape split is the practically useful part: on a Max in landscape you get a regular-width layout (two columns, split view), on a non-Max you don't. Designing landscape assuming "iPhone = compact width" breaks on Max devices, and assuming "landscape = regular width" breaks on everything else.

## Platform differences

### iOS

- **Support both orientations** where you reasonably can. If you support only one, don't tell people to rotate — they'll try it. A landscape-only app must work rotated **both** left and right.
- **Avoid full-width buttons.** Buttons belong inset within system margins. A full-width button must harmonize with the hardware corner radius and align with adjacent safe areas.
- **Keep the status bar** unless hiding it genuinely adds value (games, immersive media). It shows useful information and occupies space most apps don't use anyway.
- Games: prefer full-bleed, accommodating corner radius, sensor housing, and Dynamic Island; optionally offer letterboxed/pillarboxed as a player choice.

### iPadOS

- **Windows are freely resizable down to a minimum**, like macOS. Design for the whole range, not for a few device sizes.
- **Defer the switch to a compact layout as long as possible.** Design full-screen first; collapse only when a version of the full layout genuinely no longer fits. Premature collapsing makes the UI feel jumpy. For split views, hide **tertiary** columns (inspectors) first as width shrinks.
- **Test at the system window sizes** — halves, thirds, quadrants — across devices, and minimize surprising jumps as people drag through minimum and maximum sizes.
- **Consider a convertible tab bar**: launch as sidebar or tab bar, let people switch, and let the presentation follow the width. This avoids having to pick one navigation model for all widths.

### macOS

- **Don't put controls or critical information at the bottom of a window.** People routinely drag windows so the bottom edge falls off-screen.
- **Don't place content in the camera housing region** at the top edge.

### tvOS

- **Layouts do not adapt to screen size.** Every TV gets the same interface, so design one layout that survives a wide size range.
- **Safe area insets are explicit: 60 pt top and bottom, 80 pt left and right** for primary content. Content nearer the edge is hard to see and can be cropped by overscan on older TVs. Only deliberately-offscreen or partially-visible content belongs outside that zone.
- **Pad between focusable elements.** Focused elements *grow*, so unfocused spacing must leave room for the focused size — otherwise a focused card overlaps its neighbor's text.
- **Grids:** consistent spacing is what makes a grid readable as a grid. Titled rows need extra vertical space — between the previous row and the title's center, and between the title and the row's items. Keep partially visible offscreen content **symmetric** on both sides so attention stays on the fully visible items.

### visionOS

- **Center important content and controls.** Middle-of-window is easiest to discover and interact with, especially in large windows.
- **Keep content inside window bounds.** The system draws window controls just *outside* the bounds — Share above, resize/move/close below. Content spilling into those zones makes system controls hard to use.
- **Use an ornament** for controls that need to be associated with the window but not inside it (toolbars and tab bars are ornaments).
- **Space interactive components generously: button centers at least 60 pt apart.** This is an eye-tracking requirement, not an aesthetic one — targets need to be comfortably distinguishable by gaze, and the hover effect needs room so it doesn't obscure neighbors.
- Depth added to content in a standard window extends past the window bounds on the z-axis, and the system **clips** it if it goes too far.

### watchOS

- **Extend content edge to edge.** The physical bezel already supplies visual padding, so internal padding wastes scarce space. Minimize padding between elements.
- **No more than two or three controls side by side** — at most three glyph buttons, or two text buttons, in a row. Text buttons generally want full width; two short-labeled buttons side by side work if the screen doesn't scroll.
- **Support autorotation** for content people might show someone else — an image, a QR code — instead of sleeping the display on a wrist-turn.

## Right to left

System frameworks flip automatically. If you use standard components and layouts, most of RTL is free. What follows is what isn't free.

### Text alignment

- **Match alignment to interface direction** where the system doesn't do it for you.
- **Align a paragraph by its own language, not the context.** For three or more lines, alignment must follow the text's language — right-aligning an LTR paragraph makes the start of each line hard to find. One- and two-line blocks follow the context instead.
- **Use one consistent alignment across a list**, including items in a different script. Mixed alignment destroys scannability.

### Numbers

RTL languages differ in numeral systems: Hebrew uses Western Arabic numerals; Arabic text may use Western or Eastern Arabic numerals, varying by country and even within a country.

- **Never reverse the digits within a number.** "541", a phone number, a card number — digit order is invariant regardless of language.
- **Do reverse the order of numerals that express progress or sequence.** Tick labels on a flipped slider, step numbers, ranked lists — the sequence flips, the numerals themselves don't.

### Controls

- **Flip controls showing progress between values** — sliders, progress indicators — and swap the glyphs marking their start and end values too.
- **Flip navigation and ordered-access controls.** In RTL, the back button points **right**; next/previous flip to match reading order.
- **Don't flip a control that refers to a literal direction or points at something on screen.** A control meaning "to the right" points right in every locale.
- **Bump RTL font size by ~2 pt when adjacent to all-caps Latin text.** Arabic and Hebrew have no uppercase, so they look undersized next to capitals in buttons, labels, and titles.

### Images and icons

- **Don't flip photographs, illustrations, or artwork.** Flipping changes meaning, and flipping a copyrighted image may be a violation. If the content is inherently tied to reading direction, author a second version.
- **Do reverse the positions of images whose order is meaningful** — chronological, alphabetical, ranked.
- **Flip icons representing text or reading direction** (left-aligned bars become right-aligned).
- **Flip icons depicting forward or backward motion.** A speaker icon emits waves in the reading direction, so it flips.
- **Don't flip logos, or universal marks like the checkmark.** Legal risk on one, broken expectations on the other.
- **Generally don't flip icons of real-world objects.** A clock works the same everywhere. Right-handed tools look slanted but shouldn't flip — most people are right-handed in every locale.
- **Localize icons that contain actual text** (signature, rich-text, I-beam pointer symbols ship in Latin/Hebrew/Arabic variants). If a custom icon uses letters for a concept unrelated to writing, redesign it without text.

SF Symbols provide RTL variants and localized symbols automatically; custom symbols let you declare directionality.

## React Native mapping

### Safe areas

RN's own `SafeAreaView` is iOS-only and doesn't handle landscape side insets or Android cutouts. Use `react-native-safe-area-context`:

```jsx
import { SafeAreaProvider, useSafeAreaInsets } from 'react-native-safe-area-context';

// Wrap the app once.
<SafeAreaProvider><Root /></SafeAreaProvider>
```

The correct pattern is **padding, not a shrunken frame** — content scrolls edge to edge, only the readable content is inset:

```jsx
function Screen({ children }) {
  const insets = useSafeAreaInsets();
  return (
    <View style={{ flex: 1, backgroundColor: theme.surface }}>{/* full bleed */}
      <ScrollView
        // Content starts below the notch and ends above the home indicator,
        // but the scroll view itself — and its background — fill the screen.
        contentContainerStyle={{
          paddingTop: insets.top,
          paddingBottom: insets.bottom + TAB_BAR_HEIGHT,
          paddingLeft: insets.left,   // non-zero in landscape on notched devices
          paddingRight: insets.right,
        }}
        // Keeps the scroll indicator off the notch.
        scrollIndicatorInsets={{ top: insets.top, bottom: insets.bottom }}
      >
        {children}
      </ScrollView>
    </View>
  );
}
```

Two mistakes this avoids: `<SafeAreaView style={{flex:1}}>` as the outermost element (which shrinks the frame, leaving a colored band at the top and stopping content short of the edges), and forgetting `insets.left/right` (which only matter in landscape on notched devices — and are exactly what reviewers catch).

For a floating Liquid Glass-style tab bar, content must scroll *under* it, so add the bar height to `paddingBottom` rather than reserving layout space for it.

### Breakpoints and size classes

RN has no size classes. Derive them:

```js
import { useWindowDimensions } from 'react-native';

export function useSizeClass() {
  const { width, height } = useWindowDimensions();
  // Thresholds chosen from the size-class table: iPhone portrait is always compact
  // width; large phones in landscape and all iPads reach regular width.
  return {
    horizontal: width >= 768 ? 'regular' : width >= 700 ? 'regular' : 'compact',
    vertical: height >= 700 ? 'regular' : 'compact',
    isTablet: width >= 744,
  };
}
```

**Use `useWindowDimensions()`, never `Dimensions.get('window')` at module scope.** The latter is captured once and will be wrong after rotation, iPad window resize, or Stage Manager changes. This is the single most common source of "layout is wrong after rotating" in RN.

Follow the iPadOS rule about deferring collapse: switch on *whether the full layout fits*, not on a device check.

```js
// Better than `isTablet ? <TwoColumn/> : <OneColumn/>`
const canFitTwoColumns = width >= SIDEBAR_WIDTH + CONTENT_MIN_WIDTH;
```

### tvOS safe area

RN doesn't apply the tvOS overscan insets. Hard-code them:

```js
const TV_SAFE = { top: 60, bottom: 60, left: 80, right: 80 };
```

And leave room for focus growth — a card that scales to 1.1 on focus needs `(1.1 - 1) × size / 2` of clearance on each side, or it will overlap its neighbor.

### RTL

```js
import { I18nManager, StyleSheet } from 'react-native';

I18nManager.allowRTL(true);
const isRTL = I18nManager.isRTL;
```

**Use logical properties everywhere and RTL is nearly free:**

```js
const s = StyleSheet.create({
  row: {
    flexDirection: 'row',        // flips automatically under RTL
    paddingStart: 16,            // NOT paddingLeft
    paddingEnd: 8,               // NOT paddingRight
    marginStart: 12,
    borderStartWidth: 1,
  },
  label: { textAlign: 'left' },  // 'left' means "start" in RN — resolves per direction
});
```

`start`/`end` variants exist for padding, margin, border width/color/radius, and `left`/`right` positioning. `textAlign: 'left'` already means "start"; use `'right'` only when you mean literal right.

What still needs manual work, matching the HIG rules above:

```js
// 1. Paragraphs align by their own language, not the UI direction.
<Text style={{ textAlign: isParagraph && isLatinText ? 'left' : undefined,
               writingDirection: isLatinText ? 'ltr' : 'rtl' }} />

// 2. Flip directional icons — but only the ones that mean "forward"/"back".
const chevronBack = isRTL ? 'chevron-right' : 'chevron-left';

// 3. Mirror a custom-drawn progress/slider track, not a literal-direction arrow.
const s2 = { transform: [{ scaleX: isRTL ? -1 : 1 }] };  // then un-mirror any text inside

// 4. ~2 pt bump for RTL text beside all-caps Latin.
const rtlBump = isRTL ? 2 : 0;
```

`I18nManager.forceRTL()` requires an app reload to take effect, so RTL cannot be toggled live — test it as a separate launch configuration.

### Rotation and window resizing

```js
// Re-derive layout from the hook on every render; don't cache dimensions in state.
const { width, height } = useWindowDimensions();
const isLandscape = width > height;
```

On iPad, resizing fires continuously as the user drags. Avoid animating layout on every dimension change, and avoid expensive work in a `useEffect` keyed on width — debounce or derive during render instead.

## Review checklist

- [ ] Backgrounds and scroll content reach all four screen edges; no colored band under bars.
- [ ] Safe areas applied as **padding**, not by shrinking the frame.
- [ ] `insets.left` / `insets.right` handled (landscape on notched devices).
- [ ] Content scrolls *under* floating tab bar / toolbar; bar height added to bottom padding.
- [ ] `scrollIndicatorInsets` set so indicators avoid the notch.
- [ ] `useWindowDimensions()` used; no module-scope `Dimensions.get()`.
- [ ] Layout verified after rotation and (iPad) window resize to halves/thirds/quadrants.
- [ ] Breakpoints keyed on "does the full layout fit", not on device type.
- [ ] Compact layout deferred as long as possible; tertiary columns hidden first.
- [ ] Both landscape rotations work if landscape is supported.
- [ ] No full-width buttons unless aligned to hardware curvature and safe areas.
- [ ] Status bar hidden only for games/immersive media.
- [ ] Logical `start`/`end` spacing properties used throughout — no `left`/`right`.
- [ ] Directional icons flip under RTL; logos, checkmarks, clocks, and literal-direction arrows do not.
- [ ] Digits within a number never reversed; progress/sequence numerals do reverse.
- [ ] Paragraphs (3+ lines) aligned by their own language.
- [ ] List item alignment consistent even across mixed scripts.
- [ ] macOS: nothing critical at the window bottom or under the camera housing.
- [ ] tvOS: 60 pt top/bottom, 80 pt side insets; spacing accounts for focus growth; offscreen content symmetric.
- [ ] visionOS: content within window bounds; button centers ≥ 60 pt apart; ornaments used for external controls.
- [ ] watchOS: edge-to-edge content, ≤ 3 glyph buttons or ≤ 2 text buttons per row.
