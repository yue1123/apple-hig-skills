# Typography, Dynamic Type & UI Text

Source: HIG › Foundations › [Typography](https://developer.apple.com/design/human-interface-guidelines/typography), [Writing](https://developer.apple.com/design/human-interface-guidelines/writing)

Read this when defining a type scale, building `<Text>` wrappers, handling font scaling, or writing button labels, empty states, and error messages.

## Contents

- [Rules that matter most](#rules-that-matter-most)
- [Size floors per platform](#size-floors-per-platform)
- [System typefaces](#system-typefaces)
- [Text styles](#text-styles)
- [iOS/iPadOS Dynamic Type table](#iosipados-dynamic-type-table)
- [Other platform text styles](#other-platform-text-styles)
- [Designing for Dynamic Type](#designing-for-dynamic-type)
- [Tracking](#tracking)
- [Platform differences](#platform-differences)
- [Writing UI text](#writing-ui-text)
- [React Native mapping](#react-native-mapping)
- [Review checklist](#review-checklist)

## Rules that matter most

**Respect the platform's minimum sizes** — for custom fonts too, not just system fonts. If a custom font runs thin, go *above* the minimum, because weight affects legibility as much as size does.

**Avoid light weights.** Prefer Regular, Medium, Semibold, Bold. Ultralight, Thin, and Light are hard to read, and worse at small sizes and low contrast. They look great in a mockup at 2× on a calibrated display and fail in sunlight on a real phone.

**Minimize the number of typefaces**, even in a heavily branded interface. Every extra face muddies hierarchy and makes the app read as internally inconsistent.

**Not all text should scale equally.** When someone bumps text size, they want *the content they came for* to be more readable — not the tab titles, not the transient hit-damage numbers in a game. Scale content aggressively; scale chrome conservatively or not at all.

**Preserve relative hierarchy at every size.** A headline must still out-rank body text at AX5. If you cap sizes independently, hierarchy inverts and the screen becomes unreadable in a different way than it started.

## Size floors per platform

| Platform | Default body size | Absolute minimum |
|---|---|---|
| iOS, iPadOS | 17 pt | 11 pt |
| macOS | 13 pt | 10 pt |
| tvOS | 29 pt | 23 pt |
| visionOS | 17 pt | 12 pt |
| watchOS | 16 pt | 12 pt |

tvOS numbers look enormous because the viewing distance is ~10 feet. visionOS sizes match iOS in points but render at a virtual distance the system manages — don't "compensate" by shrinking them.

## System typefaces

**San Francisco (SF)** — sans serif. Variants: SF Pro, SF Compact, SF Mono, plus SF Arabic, SF Armenian, SF Georgian, SF Hebrew. Rounded variants exist for SF Pro/Compact and the script faces, useful for coordinating with soft or rounded UI shapes.

**New York (NY)** — serif, designed to work standalone and alongside SF.

Both ship as **variable fonts** with **dynamic optical sizing**: the system interpolates glyph structure continuously per point size, so you never need to pick a discrete "Text" vs. "Display" cut except in design tools that can't handle variable fonts.

Weights run Ultralight → Black; SF adds Condensed and Expanded widths. SF Symbols use matching weights, so symbols align optically with adjacent text at any size and weight — this is the reason to prefer SF Symbols over an icon font.

**Don't embed the system fonts in your app.** Reference them through the system API. Downloadable copies from [Apple Fonts](https://developer.apple.com/fonts/) are for design tools.

## Text styles

A *text style* bundles weight + point size + leading for each content size category. Together they form a hierarchy you address semantically (`body`, `headline`, `caption1`) rather than numerically. Using them is what gets you Dynamic Type support for free.

You can modify them with **symbolic traits** — the bold trait adds a hierarchy level; leading traits adjust line spacing:

- **Loose leading** for wide columns or long passages — extra line spacing helps people not lose their place on the return sweep.
- **Tight leading** where height is constrained, e.g. a list row.
- **Never tight leading on three or more lines**, however tight the space. That's where the return-sweep problem bites hardest.

## iOS/iPadOS Dynamic Type table

Size / leading in points. `L` is the default. `AX1`–`AX5` appear only when Larger Accessibility Sizes is on.

| Style | Weight | xS | S | M | **L** | xL | xxL | xxxL | AX1 | AX2 | AX3 | AX4 | AX5 |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| Large Title | Regular | 31/38 | 32/39 | 33/40 | **34/41** | 36/43 | 38/46 | 40/48 | 44/52 | 48/57 | 52/61 | 56/66 | 60/70 |
| Title 1 | Regular | 25/31 | 26/32 | 27/33 | **28/34** | 30/37 | 32/39 | 34/41 | 38/46 | 43/51 | 48/57 | 53/62 | 58/68 |
| Title 2 | Regular | 19/24 | 20/25 | 21/26 | **22/28** | 24/30 | 26/32 | 28/34 | 34/41 | 39/47 | 44/52 | 50/59 | 56/66 |
| Title 3 | Regular | 17/22 | 18/23 | 19/24 | **20/25** | 22/28 | 24/30 | 26/32 | 31/38 | 37/44 | 43/51 | 49/58 | 55/65 |
| Headline | Semibold | 14/19 | 15/20 | 16/21 | **17/22** | 19/24 | 21/26 | 23/29 | 28/34 | 33/40 | 40/48 | 47/56 | 53/62 |
| Body | Regular | 14/19 | 15/20 | 16/21 | **17/22** | 19/24 | 21/26 | 23/29 | 28/34 | 33/40 | 40/48 | 47/56 | 53/62 |
| Callout | Regular | 13/18 | 14/19 | 15/20 | **16/21** | 18/23 | 20/25 | 22/28 | 26/32 | 32/39 | 38/46 | 44/52 | 51/60 |
| Subhead | Regular | 12/16 | 13/18 | 14/19 | **15/20** | 17/22 | 19/24 | 21/28 | 25/31 | 30/37 | 36/43 | 42/50 | 49/58 |
| Footnote | Regular | 12/16 | 12/16 | 12/16 | **13/18** | 15/20 | 17/22 | 19/24 | 23/29 | 27/33 | 33/40 | 38/46 | 44/52 |
| Caption 1 | Regular | 11/13 | 11/13 | 11/13 | **12/16** | 14/19 | 16/21 | 18/23 | 22/28 | 26/32 | 32/39 | 37/44 | 43/51 |
| Caption 2 | Regular | 11/13 | 11/13 | 11/13 | **11/13** | 13/18 | 15/20 | 17/22 | 20/25 | 24/30 | 29/35 | 34/41 | 40/48 |

Emphasized (bold trait) weights: **Bold** for Large Title, Title 1, Title 2; **Semibold** for Title 3 and everything below.

Two things worth internalizing from this table:

1. **Body goes from 17 pt to 53 pt — a 3.1× range.** Any layout that assumes a text height is going to break. Design the largest case first if you want to save yourself the rework.
2. **The small styles clamp at the bottom.** Footnote and Caption barely shrink below `L` but grow a lot above it. Hierarchy compresses at the top of the scale, which is why maintaining *relative* hierarchy needs deliberate attention at AX sizes.

## Other platform text styles

### macOS (no Dynamic Type)

| Style | Weight | Size / line height | Emphasized |
|---|---|---|---|
| Large Title | Regular | 26/32 | Bold |
| Title 1 | Regular | 22/26 | Bold |
| Title 2 | Regular | 17/22 | Bold |
| Title 3 | Regular | 15/20 | Semibold |
| Headline | **Bold** | 13/16 | Heavy |
| Body | Regular | 13/16 | Semibold |
| Callout | Regular | 12/15 | Semibold |
| Subheadline | Regular | 11/14 | Semibold |
| Footnote | Regular | 10/13 | Semibold |
| Caption 1 | Regular | 10/13 | Medium |
| Caption 2 | Medium | 10/13 | Semibold |

Note macOS Headline is Bold by default (iOS is Semibold), and macOS has no Dynamic Type at all — sizes are fixed. AppKit also exposes dynamic font *variants* matched to specific control contexts (`controlContentFont`, `menuFont`, `menuBarFont`, `messageFont`, `titleBarFont`, `toolTipsFont`, `labelFont`, `userFont`, `userFixedPitchFont`). Use them when custom text sits next to standard controls and needs to match.

### tvOS

| Style | Weight | Size / leading | Emphasized |
|---|---|---|---|
| Title 1 | Medium | 76/96 | Bold |
| Title 2 | Medium | 57/66 | Bold |
| Title 3 | Medium | 48/56 | Bold |
| Headline | Medium | 38/46 | Bold |
| Subtitle 1 | Regular | 38/46 | Medium |
| Callout | Medium | 31/38 | Bold |
| Body | Medium | 29/36 | Bold |
| Caption 1 | Medium | 25/32 | Bold |
| Caption 2 | Medium | 23/30 | Bold |

tvOS defaults to **Medium**, not Regular — thin text doesn't survive TV upscaling and viewing distance.

### watchOS

watchOS supports Dynamic Type with size categories xS → xxxL plus AX1–AX3, and the *default* category depends on case size: Small = 38mm, Large = 40/41/42mm, xLarge = 44/45/49mm. Styles are Large Title, Title 1–3, Headline, Body, Caption 1–2, Footnote 1–2. At the Large default: Body 16/18.5, Headline 16/18.5 Semibold, Title 3 19/21.5, Caption 1 15/17.5, Footnote 2 12/14.5. Leadings carry a .5 because watchOS uses half-point line heights.

## Designing for Dynamic Type

Dynamic Type exists on iOS, iPadOS, tvOS, visionOS, watchOS. **Not macOS.**

**Verify at the extremes, not the middle.** Turn on Settings › Accessibility › Display & Text Size › Larger Text and drag to maximum. Most Dynamic Type bugs only appear at AX3+.

**Scale meaningful icons with text.** If an icon carries information, it must grow too. SF Symbols do this automatically; bitmap assets don't.

**Minimize truncation as size grows.** The goal: show as much useful text at the largest accessibility size as at the largest standard size. Let labels take as many lines as they need. Don't truncate inside a scrollable region unless there's a detail view to read the rest.

**Restructure the layout at large sizes, don't just reflow it.** Two specific moves Apple calls out:

- **Horizontal → stacked.** A row of `[icon] [label] [timestamp]` crowds and truncates as text grows. Above a threshold, stack the label above the secondary items.
- **Fewer columns.** Multi-column text becomes unreadable when each column is too narrow for the type size. Drop column count as size increases.

The system exposes a boolean for exactly this decision (`isAccessibilityCategory` in UIKit) — the point is that it's a *layout mode switch*, not a continuous adjustment.

**Keep the information hierarchy stable.** Primary elements stay near the top even at huge sizes, so people don't lose track of them.

**Custom fonts must implement the same behaviors** — Dynamic Type scaling and Bold Text — or they'll silently opt out of accessibility settings the system fonts honor.

## Tracking

The system adjusts tracking (letter spacing) continuously per point size at runtime; you only need explicit values when producing mockups or hand-rendering text. The shape of the curve for SF Pro on iOS/iPadOS/visionOS:

| Size | 8 | 10 | 12 | 14 | 17 | 20 | 24 | 28 | 34 | 48 | 64 | 80+ |
|---|---|---|---|---|---|---|---|---|---|---|---|---|
| Tracking (pt) | +0.21 | +0.12 | 0.0 | −0.15 | **−0.43** | −0.45 | +0.07 | +0.38 | +0.40 | +0.35 | +0.22 | 0 |

Tracking is positive at small sizes (open the letters up so they don't collide), crosses zero at 12 pt, reaches its tightest around 17–20 pt, then goes positive again for display sizes before settling at 0 above 80 pt. SF Pro Rounded stays positive throughout (+0.57 at 8 pt → +0.37 at 17 pt). New York trends negative as size grows (−0.07 at 17 pt → −1.50 at 96 pt). SF Compact on watchOS goes strongly negative at display sizes (−0.96 at 48 pt).

The practical takeaway for RN: if you set `letterSpacing` on a system-font `<Text>`, you are *overriding* this curve, and it will look wrong at some sizes. Leave it unset unless you're matching a specific spec.

## Platform differences

| Platform | System font | Dynamic Type | Notes |
|---|---|---|---|
| iOS, iPadOS | SF Pro (NY available) | Yes, + AX1–AX5 | The reference implementation. |
| macOS | SF Pro (NY via Mac Catalyst) | **No** | Fixed sizes; AppKit control-matched font variants. |
| tvOS | SF Pro (NY available) | Yes | Medium default weight; huge sizes. |
| visionOS | SF Pro (NY needs explicit styles) | Yes | Bolder Body and Title styles than iOS; adds **Extra Large Title 1/2** for editorial layouts. |
| watchOS | **SF Compact** (NY available); SF Compact Rounded in complications | Yes, + AX1–AX3 | Default size category varies by case size. |

**visionOS specifics worth calling out:**

- **Prefer 2D text.** Visual depth makes characters harder to read. A little 3D text as an accent is fine; anything people must actually read should be flat.
- **Default to white text** — it contrasts best with the standard glass material. Test any other color across environments.
- **Bold text that has no background behind it**, and *don't* add shadows to boost contrast — there may be no surface to cast onto, and you can't predict what shadow density works in an unknown environment.
- **Billboard text anchored to a point in space** so it always faces the wearer. Without y-axis rotation, walking around an object makes its label edge-on and unreadable.
- Test legibility at multiple scales, since people can scale windows.

## Writing UI text

Copy is part of the interface, and it's the part most often written last and reviewed least.

**Establish a voice, then vary tone by situation.** Voice is constant — a banking app conveys trust, a game conveys excitement. Tone shifts with context: direct and serious for a payment failure, light and congratulatory for hitting a goal. Keep a term list so vocabulary stays consistent.

**Be action-oriented.** Use active voice. Label buttons and links with a **verb**: "Send" beats "Let's do it!". For links, describe the destination — "Learn more about UX writing", never "Click here". Screen reader users hear link labels out of context, so a descriptive label isn't just style.

**Pick a capitalization convention per element type and hold it.** Title case reads formal, sentence case casual. Choose one for alerts, one for headlines, and apply consistently. (Buttons have their own guidance — title-style capitalization, starting with a verb.)

**Use consistent step language in multi-screen flows.** "Get Started" to enter, "Continue"/"Next" between steps — pick one and don't alternate — "Done" to signal completion.

**Use possessive pronouns sparingly, and avoid "we" entirely.** "Favorites" says everything "Your Favorites" does. "We're having trouble loading this content" raises the question of who "we" is; "Unable to load content" doesn't.

**Match vocabulary to the device.** Don't say "click" on a touch device. Adjust for context of use, too: iPhone and Watch demand brevity from small screens; tvOS demands brevity because text must be readable across a room *and* because a living room is shared space — think about who else can see it.

**Make empty states do work.** An empty screen is daunting when the next step isn't obvious. Say what people can do and give them a button to do it. Don't put crucial information there — empty states are temporary by definition.

**Write error messages that fix the problem.** Place them next to the cause, don't assign blame, and state the remedy: "Choose a password with at least 8 characters" instead of "That password is too short". Skip "Oops!" and "Uh-oh" — they read as insincere when someone is already frustrated. If copy alone can't rescue an error that hits lots of people, that's a signal to redesign the interaction rather than reword the message.

**Label settings by what turning them on does.** People infer the off state. If the label isn't self-evident, add a short explanation. To point someone at a setting, link or button them there — don't describe where to tap.

**Give text fields real hints.** Label every field, and use placeholder text that shows the expected format (`name@example.com`) or names the content ("Your name"). Show errors adjacent to the field, phrased as instructions: "Use only letters for your name" beats "Don't use numbers or symbols", and both beat "Invalid name".

**Match delivery to urgency.** Weigh how urgent and important the message is, whether it needs immediate action, and how much supporting detail is required, then choose between a notification, an alert, an action sheet, or inline text.

## React Native mapping

### The scale factor

`PixelRatio.getFontScale()` returns the user's text scale multiplier. On iOS it tracks the Dynamic Type category (roughly 0.82 at xS through ~3.1 at AX5, per the Body row above); on Android it tracks Settings › Display › Font size.

```js
import { PixelRatio, StyleSheet } from 'react-native';

const scale = PixelRatio.getFontScale();
const isAccessibilitySize = scale >= 1.35; // ≈ xxxL and up — the layout-mode switch
```

`getFontScale()` is read at call time and does **not** trigger a re-render when the user changes it. On iOS, appearance/text-size changes while your app is backgrounded are common. Either read it during render of a component that remounts, or subscribe:

```js
import { useWindowDimensions } from 'react-native';

function useFontScale() {
  useWindowDimensions();               // re-renders on metric changes
  return PixelRatio.getFontScale();
}
```

### Define the type scale semantically

Mirror the HIG text styles rather than inventing sizes. Multiply by the OS scale yourself only where you need the number for layout — `<Text>` already scales by default.

```js
// Base = the "L" (default) column of the Dynamic Type table.
export const textStyles = StyleSheet.create({
  largeTitle: { fontSize: 34, lineHeight: 41, fontWeight: '400' },
  title1:     { fontSize: 28, lineHeight: 34, fontWeight: '400' },
  title2:     { fontSize: 22, lineHeight: 28, fontWeight: '400' },
  title3:     { fontSize: 20, lineHeight: 25, fontWeight: '400' },
  headline:   { fontSize: 17, lineHeight: 22, fontWeight: '600' },
  body:       { fontSize: 17, lineHeight: 22, fontWeight: '400' },
  callout:    { fontSize: 16, lineHeight: 21, fontWeight: '400' },
  subhead:    { fontSize: 15, lineHeight: 20, fontWeight: '400' },
  footnote:   { fontSize: 13, lineHeight: 18, fontWeight: '400' },
  caption1:   { fontSize: 12, lineHeight: 16, fontWeight: '400' },
  caption2:   { fontSize: 11, lineHeight: 13, fontWeight: '400' },
});
```

Use the system font by omitting `fontFamily` — RN then resolves to SF on iOS/macOS/tvOS and Roboto on Android, which is the correct behavior on both. Naming a family like `'SF Pro'` is worse: it breaks Android, and it opts out of dynamic optical sizing.

**Set `lineHeight` explicitly.** RN's default line height differs from the HIG leading values, and the difference compounds visibly in multi-line body text.

### Let text scale, but choose where

`allowFontScaling` defaults to `true` and should usually stay that way — that *is* Dynamic Type support.

```js
// Content: scale freely (the default).
<Text style={textStyles.body}>{article.body}</Text>

// Chrome: cap the growth so tab labels don't blow up the bar.
// maxFontSizeMultiplier keeps scaling but bounds it — better than allowFontScaling={false},
// which opts out entirely and is what the "not all text scales equally" rule warns against.
<Text style={textStyles.caption2} maxFontSizeMultiplier={1.2}>{tab.title}</Text>
```

Reach for `allowFontScaling={false}` only for text that genuinely must not move — a fixed-geometry chart axis, a numeric keypad glyph. Every use is an accessibility tradeoff you should be able to justify.

To apply a policy app-wide instead of per-call site, set the static defaults once at startup:

```js
import { Text, TextInput } from 'react-native';

Text.defaultProps = Text.defaultProps || {};
Text.defaultProps.maxFontSizeMultiplier = 2.5;
TextInput.defaultProps = TextInput.defaultProps || {};
TextInput.defaultProps.maxFontSizeMultiplier = 2.0;
```

### Switch layout mode, don't just reflow

This is the RN version of the horizontal → stacked rule:

```js
function ListRow({ title, timestamp }) {
  const stacked = useFontScale() >= 1.35;
  return (
    <View style={stacked ? s.rowStacked : s.rowInline}>
      <Text style={textStyles.body}>{title}</Text>
      <Text style={textStyles.footnote}>{timestamp}</Text>
    </View>
  );
}

const s = StyleSheet.create({
  rowInline:  { flexDirection: 'row', alignItems: 'center', justifyContent: 'space-between', gap: 8 },
  rowStacked: { flexDirection: 'column', alignItems: 'flex-start', gap: 2 },
});
```

Give rows `minHeight` rather than fixed `height`, and avoid `numberOfLines={1}` on anything load-bearing — those two habits cause most Dynamic Type truncation bugs in RN.

### Bold Text and other text a11y settings

```js
import { AccessibilityInfo } from 'react-native';

// iOS "Bold Text" setting — bump weight, don't just leave it to the OS,
// because RN doesn't apply it to custom-weighted text automatically.
const [boldText, setBoldText] = useState(false);
useEffect(() => {
  AccessibilityInfo.isBoldTextEnabled().then(setBoldText);
  const sub = AccessibilityInfo.addEventListener('boldTextChanged', setBoldText);
  return () => sub.remove();
}, []);

const weight = boldText ? '600' : '400';
```

### Platform-specific type

```js
const platformType = Platform.select({
  ios:     { body: 17, min: 11 },
  macos:   { body: 13, min: 10 },
  tvos:    { body: 29, min: 23, weight: '500' }, // tvOS defaults to Medium
  visionos:{ body: 17, min: 12 },
  android: { body: 16, min: 12 },
});
```

On tvOS remember the default weight change; on macOS remember there is no Dynamic Type, so `getFontScale()` is effectively 1 and the scaling machinery above is inert (harmless, but don't build layout logic that depends on it varying).

## Review checklist

- [ ] Type scale mirrors HIG text styles semantically; no ad-hoc font sizes.
- [ ] `fontFamily` omitted for system font, not hard-coded to `'SF Pro'`.
- [ ] `lineHeight` set explicitly to the HIG leading value.
- [ ] No font sizes below the platform minimum (11 pt iOS, 10 pt macOS, 23 pt tvOS, 12 pt visionOS/watchOS).
- [ ] No Ultralight/Thin/Light weights in UI text.
- [ ] At most one or two typefaces total.
- [ ] `allowFontScaling` left on for content; capped via `maxFontSizeMultiplier` for chrome; disabled only with a stated reason.
- [ ] Layout verified at maximum Larger Accessibility Text (≈3.1× body).
- [ ] Rows use `minHeight`, not fixed `height`; no `numberOfLines={1}` on load-bearing text.
- [ ] Layout switches from inline to stacked, and reduces columns, at accessibility sizes.
- [ ] Relative hierarchy holds at AX5 — headline still out-ranks body.
- [ ] Meaningful icons scale with text (SF Symbols, or explicitly sized from font scale).
- [ ] Bold Text setting honored.
- [ ] `letterSpacing` not overriding the system tracking curve without a reason.
- [ ] Button labels start with a verb; no "Click here" links.
- [ ] Capitalization convention consistent per element type.
- [ ] Error messages state the remedy, sit next to the cause, and contain no "Oops"/"We".
- [ ] Empty states name a next action and offer a control for it.
- [ ] Text fields have labels plus format-demonstrating placeholders.
- [ ] No "click" wording on touch platforms.
- [ ] tvOS text at Medium weight or heavier.
- [ ] visionOS text is 2D, defaults to white, bolded when backgroundless, billboarded when spatially anchored.
