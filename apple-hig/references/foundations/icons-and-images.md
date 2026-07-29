# Icons, SF Symbols, Images & Branding

Source: HIG › Foundations › [SF Symbols](https://developer.apple.com/design/human-interface-guidelines/sf-symbols), [Icons](https://developer.apple.com/design/human-interface-guidelines/icons), [Images](https://developer.apple.com/design/human-interface-guidelines/images), [Branding](https://developer.apple.com/design/human-interface-guidelines/branding), [App icons](https://developer.apple.com/design/human-interface-guidelines/app-icons)

Read this when choosing or drawing icons, wiring up an icon library, exporting image assets, or deciding how much brand to put on screen.

## Contents

- [SF Symbols](#sf-symbols)
- [Standard symbols for common actions](#standard-symbols-for-common-actions)
- [Custom interface icons](#custom-interface-icons)
- [Images: resolution and formats](#images-resolution-and-formats)
- [Branding](#branding)
- [Platform differences](#platform-differences)
- [React Native mapping](#react-native-mapping)
- [Review checklist](#review-checklist)

## SF Symbols

Thousands of symbols designed to integrate with San Francisco — they align with text automatically at every weight and size. Use them anywhere an interface icon appears: toolbars, tab bars, context menus, inline in text.

**Availability is version-gated.** Symbols and features introduced in a given year don't exist on earlier OS versions. Check the SF Symbols app for the minimum OS of any symbol you use.

**License restriction that catches people out:** you may not use SF Symbols — or confusingly similar images — in **app icons, logos, or any trademarked use**. Symbols are for interface use only. Separately, symbols depicting Apple products and features are copyrighted and **cannot be customized** (the SF Symbols app badges these with an Info icon).

### Rendering modes

Symbols organize their paths into layers, which is what makes these modes possible. `cloud.sun.rain.fill` has three: cloud (primary), sun and rays (secondary), raindrops (tertiary).

- **Monochrome** — one color across all layers.
- **Hierarchical** — one color, varying **opacity** per layer. This is how you convey depth and foreground/background within a symbol.
- **Palette** — two or more colors, one per layer. Supply two colors for a three-layer symbol and the secondary and tertiary layers share one.
- **Multicolor** — intrinsic colors that carry meaning: `leaf` is green, `trash.slash` is red for data loss. Some multicolor symbols still accept your colors on other layers.

Use **system-provided colors** in any mode and symbols adapt automatically to accessibility settings, vibrancy, and Dark Mode.

**Verify the rendering mode in every context it appears.** Size and background contrast change how discernible the details are. The automatic setting picks a preferred mode, but check the result — a hierarchical symbol that reads beautifully at 32 pt can turn to mush at 17 pt on a busy background.

**Gradients** (SF Symbols 7+) generate a linear gradient from a single source color, in any rendering mode, for system and custom symbols. They render at any size but are designed for larger ones.

### Variable color

Represents a quantity that changes over time — capacity, signal strength, volume — by coloring layers as a value crosses thresholds between 0 and 100%. `speaker.wave.3` colors wave layers progressively; the speaker body itself opts out, because a speaker doesn't change when the volume does.

**Use variable color for change, not for depth.** Depth is Hierarchical's job. Mixing the two meanings makes the symbol ambiguous — is that faded layer *behind* or *inactive*?

### Weights and scales

Nine weights (ultralight → black), each matching a San Francisco font weight, so symbol and adjacent text can be weight-matched exactly.

Three scales — small, **medium** (default), large — defined relative to the SF **cap height**. Scale lets you change a symbol's emphasis relative to neighboring text *without* breaking weight matching at the same point size. That separation is the useful part: size the symbol by scale, match the text by weight.

### Design variants

- **Outline** — the most common; no solid areas, reads like text. Best in toolbars, lists, and anywhere a symbol sits beside text.
- **Fill** — solid interior areas, more visual emphasis. Best for iOS tab bars, swipe actions, and accent-colored selection states.
- **Slash** — indicates unavailable.
- **Enclosed** (circle, square, rectangle) — improves legibility at small sizes. Combines with outline/fill.

**Often you don't specify a variant at all** — an iOS tab bar prefers fill, a toolbar takes outline, and the view decides. Hard-coding the variant is how tab bars end up looking subtly wrong.

Language- and script-specific variants exist for Latin, Arabic, Hebrew, Hindi, Thai, Chinese, Japanese, Korean, Cyrillic, Devanagari, and several Indic numeral systems, and they **switch automatically with the device language**.

### Animations

Available on every symbol in the library, in all rendering modes, weights, and scales, and on custom symbols. Playback can run once or repeat until a condition is met; speed and auto-reverse are configurable.

| Animation | What it communicates |
|---|---|
| **Appear / Disappear** | Gradual emergence or recession from view |
| **Bounce** | An action occurred or needs to occur; elastic, returns to initial state, plays once by default |
| **Scale** | Persistent size change — selection, or feedback on choosing a symbol. Unlike bounce, it stays until you change or remove it |
| **Pulse** | Ongoing activity, via opacity only; animates layers annotated to pulse (optionally all) |
| **Variable color** | Progress or ongoing activity — playback, connecting, broadcasting. Cumulative (changes persist through the cycle) or iterative (one layer at a time) |
| **Replace** | One symbol becomes another. *Down-up* = state change; *up-up* = state change with forward progression; *off-up* = state change emphasizing the next action |
| **Magic Replace** | Smart transition between related symbols — slashes draw on/off, badges appear/disappear independently. **The new default**; falls back to down-up between unrelated symbols |
| **Wiggle** | Highlights a change or call to action someone might miss; also reinforces directional meaning |
| **Breathe** | Living quality for status or ongoing activity like recording. Like pulse but changes **size and opacity**, not opacity alone |
| **Rotate** | Progress, or imitating real-world behavior. Some symbols rotate wholly; others (a desk fan) rotate only annotated layers |
| **Draw On / Draw Off** | (SF Symbols 7+) Draws along a path through guide points — all layers at once, staggered, or one at a time. For progress, or reinforcing a symbol's meaning |

Variable color's repeat behavior depends on layer arrangement: **open loop** symbols have layers that don't meet end-to-end; **closed loop** symbols form a complete shape (like a circular progress indicator) and animate seamlessly on repeat.

**Apply animations judiciously.** There's no technical limit, and that's the problem — too many animations overwhelm the interface. Each animation type has a discrete meaning; make sure yours communicates the symbol's actual intent and won't be misread. Consider whether the animation matches your app's tone.

### Custom symbols

Export a template for a similar symbol, then modify it in a vector editor.

**Match the system's level of detail, optical weight, alignment, position, and perspective.** Aim for simple, recognizable, inclusive, and directly related to the action or content.

**Annotate layers** to assign colors or hierarchical levels (primary/secondary/tertiary), which is what enables rendering modes.

**Use negative side margins for optical alignment** when a symbol carries a badge or other width-increasing element — e.g. aligning a stack of folder symbols where only some have badges. Follow the naming pattern (`left-margin-Regular-M`).

**Draw whole shapes, not cutouts, if you want animations to work.** For a `person.2.fill`-like symbol, don't cut the left person out of the right one — draw the full left person, then draw an offset path for the gap and annotate it as an **erase layer**. Cutouts destroy the layer information animations need, and this shows up only when you test the animation, not when you look at the static symbol.

**Test every animation preset on custom symbols.** Paths that look fine static can behave unexpectedly in motion.

**Don't hand-build common variants** (enclosures, badges) — the SF Symbols app's component library generates them consistently.

**Provide accessibility descriptions** for custom symbols.

**Never replicate Apple products.** Copyrighted, and hardware designs change, which dates your UI.

## Standard symbols for common actions

Using these makes actions predictable across every app on the platform.

| Action | Symbol |
|---|---|
| Cut | `scissors` |
| Copy | `document.on.document` |
| Paste | `document.on.clipboard` |
| Done | `checkmark` |
| Cancel / Close / Deselect | `xmark` |
| Delete | `trash` |
| Undo | `arrow.uturn.backward` |
| Redo | `arrow.uturn.forward` |
| Compose | `square.and.pencil` |
| Duplicate | `plus.square.on.square` |
| Rename | `pencil` |
| Move to / Folder | `folder` |
| Attach | `paperclip` |
| Add | `plus` |
| More | `ellipsis` |
| Select | `checkmark.circle` |
| Search | `magnifyingglass` |
| Find | `text.page.badge.magnifyingglass` |
| Filter | `line.3.horizontal.decrease` |
| Share / Export | `square.and.arrow.up` |
| Print | `printer` |
| Account / User / Profile | `person.crop.circle` |
| Like | `hand.thumbsup` |
| Dislike | `hand.thumbsdown` |
| Alarm | `alarm` |
| Archive | `archivebox` |
| Calendar | `calendar` |
| Bold | `bold` |
| Italic | `italic` |
| Underline | `underline` |
| Superscript | `textformat.superscript` |
| Subscript | `textformat.subscript` |
| Align left | `text.alignleft` |
| Center | `text.aligncenter` |
| Align right | `text.alignright` |
| Justified | `text.justify` |
| Bring to front | `square.3.layers.3d.top.filled` |
| Send to back | `square.3.layers.3d.bottom.filled` |
| Bring forward | `square.2.layers.3d.top.filled` |
| Send backward | `square.2.layers.3d.bottom.filled` |

`square.and.arrow.up` is worth singling out: it is *the* share affordance on Apple platforms. Substituting Android's three-node share glyph on iOS is an immediately recognizable tell.

## Custom interface icons

An **interface icon** (or glyph) expresses one concept with streamlined shapes and minimal color — distinct from an **app icon**, which can use shading, texture, and highlights for personality.

**Simplify aggressively.** Too much detail makes an icon unreadable. Prefer familiar visual metaphors directly tied to the action or content.

**Keep the whole set visually consistent** — same size, level of detail, stroke weight, and perspective, whether you're using all custom icons or mixing custom with system ones. You may need to adjust individual dimensions so icons of differing visual weight *look* the same size.

**Match icon weight to adjacent text weight** unless you specifically want to emphasize one over the other.

**Optically center, don't geometrically center.** Asymmetric icons look off when centered by bounding box — a download arrow has more visual weight at the bottom and reads as sitting too low. Adjust position until it looks centered, then bake that offset in as **padding in the asset**, so consumers can center geometrically and get optical centering for free. The adjustments are tiny and the effect on overall polish is large.

**Don't author selected/unselected variants** for icons used in standard toolbars, tab bars, and buttons — the system handles selected appearance.

**Use inclusive imagery.** Prefer gender-neutral human figures; avoid metaphors that don't survive translation across cultures.

**Include text only when essential.** A character representing text formatting is the clearest option. If you include individual characters, **localize them**. To suggest a passage of text, draw an abstract representation and supply a flipped version for RTL.

**Use a vector format — PDF or SVG.** The system scales vectors for high-resolution displays automatically. PNG can't scale, so every PNG icon needs multiple resolution variants.

**Provide accessibility descriptions.**

**Avoid depicting Apple hardware.** If you must, use images from Apple Design Resources or the SF Symbols that represent Apple products.

## Images: resolution and formats

### Scale factors

| Platform | Scale factors |
|---|---|
| iPadOS, watchOS | @2x |
| iOS | @2x and @3x |
| visionOS | @2x or higher |
| macOS, tvOS | @1x and @2x |

**Design at the lowest resolution and scale up.** With vector shapes, put control points on whole values so they align to the raster grid at 1x — and therefore at 2x and 3x, which are multiples.

### Formats

| Content | Format |
|---|---|
| Bitmap / raster artwork | De-interlaced PNG |
| PNG not needing 24-bit color | 8-bit color palette |
| Photos | JPEG (optimized) or HEIC |
| Stereo / spatial photos | Stereo HEIC |
| Flat icons and artwork needing scaling | PDF or SVG |

**Embed a color profile in every image** so colors render as intended across displays.

**Avoid transparency where you can** — it inflates file size. If you always composite onto the same solid color, bake the background in. Transparency *is* required for template images: complication images, menu icons, and other interface icons where the system uses the alpha channel to decide where to apply color.

**Test on real devices.** Images that look right at design time can appear pixelated, stretched, or compressed in the field.

## Branding

**Branding must defer to content.** Space spent purely on brand assets is space not spent on what people came for. Incorporate brand in refined, unobtrusive ways.

**Don't scatter your logo through the app.** People rarely need reminding which app they're in; use the space for information and controls instead.

**Don't treat the launch screen as a branding surface.** It exists to minimize perceived startup time and disappears too fast to convey anything. If you want a branded moment, use a welcome or onboarding screen.

**Use standard patterns even in a highly stylized interface.** Familiar behaviors and expected component locations are what keep a distinctive design approachable.

**Brand through voice and tone**, an accent color, and — if the brand is tied to a typeface — a custom font. If you use a custom font, verify it's legible at all sizes and supports Bold Text and larger type. A good split: custom font for headlines and subheads, system font for body and captions, since the system fonts are engineered for small-size legibility.

**Follow Apple's trademark guidelines** — Apple trademarks must not appear in your app name or images.

## Platform differences

**macOS document icons.** If your app defines a custom document type you can supply a document icon; otherwise macOS composites your app icon with the file extension. You provide any combination of background fill, center image, and text, and the system masks it into the folded-corner shape. Key constraints: it renders as small as **16×16 px**, so simplify at small sizes (fewer, thicker grid lines at intermediate sizes; drop them entirely at 16 px); **keep important content out of the top-right corner** where the fold is drawn; the center image is **half the canvas size**; keep ~**10% margin** so the image occupies roughly 80% of its canvas. You can substitute a short descriptive term for the extension (SceneKit uses *scene* rather than *scn*) — the system scales and capitalizes it.

**tvOS layered images.** Standard views plus system focus APIs give layered images the parallax treatment automatically. Identify logical foreground (prominent elements, text), middle (secondary content, shadows), and background layers. **The background layer must be opaque** — you get an error otherwise — and this is what makes parallax, drop shadows, and system backgrounds look right. **Keep layering simple and subtle**: parallax is meant to be nearly unnoticeable, and heavy 3D reads as unrealistic. Leave a **safe zone** around foreground layers, since focus scaling and movement can crop them. Preview with Xcode, Parallax Previewer, or the Photoshop Parallax Exporter — then on a real TV.

**visionOS images.** Prefer vector art for 2D; bitmaps don't survive system scaling. If you must rasterize, balance quality against performance: @2x looks fine at typical distance but won't be sharp up close, and resolutions above @6x hurt runtime performance — apply high-quality filtering if you go high. App icons are **two to three layers** that move at slightly different rates on focus. For spatial photos use stereo HEIC with spatial metadata so visionOS applies its discomfort-mitigating treatments. Put text over spatial photos on a **feathered glass background** — it adds contrast and blurs detail that would otherwise cause discomfort. **Display spatial photos in standalone views** (a sheet or window), not inline with other content; if inline is unavoidable, leave generous spacing so eyes can adjust to the depth change. Spatial scenes take **several seconds to generate**, so gate them behind an explicit action and don't render many at once. Prefer **larger** spatial scenes centered in the field of view — small ones give too little parallax to be worth it.

**watchOS autoscaling PDFs.** One asset covers all screen sizes: design for 40mm/42mm at @2x, and WatchKit scales by device — 38mm 90%, 40mm 100%, 41mm 106%, 42mm 100%, 44mm 110%, 45mm 119%, 49mm 119%.

## React Native mapping

### SF Symbols in RN

Symbols are the correct default on Apple platforms, and there are three routes:

```jsx
// 1. expo-symbols — real SF Symbols, iOS 15+. Best fidelity: real weight matching,
//    rendering modes, and animations.
import { SymbolView } from 'expo-symbols';

<SymbolView
  name="square.and.arrow.up"
  type="hierarchical"          // monochrome | hierarchical | palette | multicolor
  tintColor={PlatformColor('systemBlue')}
  weight="semibold"            // match adjacent text weight
  size={22}
  animationSpec={{ effect: { type: 'bounce' } }}
  // Fallback for Android / older iOS.
  fallback={<Icon name="share" size={22} />}
/>

// 2. react-native-sfsymbols — similar, bare RN.
// 3. Vector icon sets (react-native-vector-icons, @expo/vector-icons) — cross-platform,
//    but the glyphs are not SF Symbols and won't weight-match SF text.
```

Because symbols are iOS-only, build one component that resolves per platform rather than scattering conditionals:

```jsx
// Icon.tsx — single seam between HIG symbols and the Android/web equivalent.
const SYMBOLS = {
  share:  { ios: 'square.and.arrow.up', md: 'share-variant' },
  delete: { ios: 'trash',               md: 'delete-outline' },
  more:   { ios: 'ellipsis',            md: 'dots-horizontal' },
  search: { ios: 'magnifyingglass',     md: 'magnify' },
};

export function Icon({ name, size = 22, weight = 'regular', color }) {
  const map = SYMBOLS[name];
  if (Platform.OS === 'ios' || Platform.OS === 'macos' || Platform.OS === 'tvos') {
    return <SymbolView name={map.ios} size={size} weight={weight} tintColor={color} />;
  }
  return <MaterialCommunityIcons name={map.md} size={size} color={color} />;
}
```

This keeps the HIG rule "use the platform's standard symbol" satisfiable without Android inheriting iOS metaphors it doesn't use — the share glyph being the clearest case.

### Weight matching with text

```jsx
// The symbol should carry the same weight as the text beside it.
<View style={{ flexDirection: 'row', alignItems: 'center', gap: 6 }}>
  <Icon name="search" size={17} weight="regular" />
  <Text style={{ fontSize: 17, fontWeight: '400' }}>Search</Text>
</View>
```

And scale symbols with Dynamic Type, per the "meaningful icons grow with text" rule:

```js
const iconSize = 22 * Math.min(PixelRatio.getFontScale(), 2);  // cap so chrome stays usable
```

### Tab bar fill variants

Follow the system's outline/fill convention instead of using one variant everywhere:

```jsx
<Tab.Screen
  options={{
    tabBarIcon: ({ focused, color, size }) => (
      // iOS tab bars use the fill variant; toolbars use outline.
      <Icon name={focused ? 'house.fill' : 'house'} size={size} color={color} />
    ),
  }}
/>
```

### Vector assets

```js
// SVG via react-native-svg — scales cleanly, matching the "use PDF or SVG" rule.
import Logo from './logo.svg';           // with react-native-svg-transformer
<Logo width={120} height={32} />
```

Prefer SVG over multi-resolution PNG for flat artwork. Reserve `@2x`/`@3x` PNG sets for photographic or effect-laden images:

```
assets/hero.png        // @1x
assets/hero@2x.png
assets/hero@3x.png
// RN's asset resolver picks by device scale automatically — you reference './hero.png'.
```

### Optical centering

The HIG advice — bake the offset into the asset as padding — is the right approach in RN too, because it keeps consuming layouts simple:

```jsx
// Preferable: asset already optically centered, so plain centering works.
<View style={{ alignItems: 'center', justifyContent: 'center' }}><Icon name="share" /></View>

// If you must correct at the call site, do it once in the Icon component,
// not sprinkled through screens.
const OPTICAL_OFFSET = { share: { marginTop: -1 } };
```

### Accessibility

```jsx
// Meaningful icon: label it.
<Icon name="share" accessibilityRole="image" accessibilityLabel="Share" />

// Icon inside a button: label the button, hide the icon,
// so VoiceOver announces one thing rather than two.
<Pressable accessibilityRole="button" accessibilityLabel="Share">
  <Icon name="share" accessibilityElementsHidden importantForAccessibility="no-hide-descendants" />
</Pressable>

// Purely decorative: hide it.
<Image source={flourish} accessibilityElementsHidden importantForAccessibility="no-hide-descendants" />
```

### RTL

```jsx
// Directional icons flip; literal-direction and universal marks don't.
// See layout.md for the full rule set.
const FLIPPABLE = new Set(['chevron.left', 'chevron.right', 'arrow.left', 'arrow.right', 'text.alignleft']);
const style = FLIPPABLE.has(symbolName) && I18nManager.isRTL ? { transform: [{ scaleX: -1 }] } : null;
```

SF Symbols already provide RTL variants, so with `expo-symbols` this is often unnecessary — check before adding a manual flip, or you'll double-flip.

### App icon and launch screen

These are native configuration, not RN code. In Expo, `app.json` → `expo.icon` and `expo.splash`; in bare RN, the Xcode asset catalog and `LaunchScreen.storyboard`. Two HIG rules to carry over: **don't put SF Symbols in your app icon** (license), and **keep the launch screen unbranded and minimal** — Expo's `splash` should approximate your app's first screen, not be a logo reveal.

## Review checklist

- [ ] SF Symbols used for interface icons on Apple platforms; standard symbol chosen for each standard action (especially `square.and.arrow.up` for share).
- [ ] No SF Symbols in the app icon, logo, or any trademarked use.
- [ ] Symbol availability checked against the minimum supported OS version.
- [ ] Symbol weight matches adjacent text weight; scale used for emphasis rather than mismatched weights.
- [ ] Outline vs. fill follows the container's convention (fill in iOS tab bars, outline in toolbars); variant not hard-coded where the view decides.
- [ ] Rendering mode verified at the actual size and background it ships on.
- [ ] Variable color used for changing quantities; Hierarchical used for depth — not interchanged.
- [ ] Symbol animations used sparingly and matched to meaning.
- [ ] Custom symbols annotated by layer, drawn as whole shapes (erase layers, not cutouts), tested against all animation presets.
- [ ] Custom icon set consistent in size, detail, stroke weight, and perspective.
- [ ] Asymmetric icons optically centered, with the offset baked into the asset.
- [ ] No selected/unselected variants hand-authored for standard components.
- [ ] Icons are vector (SVG/PDF); multi-resolution PNG reserved for photographic assets.
- [ ] Icon size scales with Dynamic Type where the icon carries information.
- [ ] Icons in buttons hidden from the accessibility tree; the button carries the label.
- [ ] Decorative images hidden from assistive technology.
- [ ] Directional icons flip under RTL without double-flipping symbol built-ins.
- [ ] Text inside icons localized, or replaced with a non-textual design.
- [ ] Color profiles embedded; transparency avoided except for template images.
- [ ] No depictions or replicas of Apple hardware or products.
- [ ] Logo not repeated through the app; launch screen not used for branding.
- [ ] Custom brand font legible at all sizes and honoring Bold Text and Dynamic Type; system font used for body and captions.
- [ ] tvOS layered images have an opaque background layer, subtle depth, and a foreground safe zone.
- [ ] visionOS spatial photos use stereo HEIC, standalone views, and feathered glass behind overlaid text.
