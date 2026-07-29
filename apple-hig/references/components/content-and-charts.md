# Charts, Image Views, Text Views & Web Views

Source: HIG › Components › Content — [Charts](https://developer.apple.com/design/human-interface-guidelines/charts), [Image views](https://developer.apple.com/design/human-interface-guidelines/image-views), [Text views](https://developer.apple.com/design/human-interface-guidelines/text-views), [Web views](https://developer.apple.com/design/human-interface-guidelines/web-views); Patterns › [Charting data](https://developer.apple.com/design/human-interface-guidelines/charting-data)

## Contents

- [Charting data](#charting-data)
- [Chart anatomy](#chart-anatomy)
- [Chart accessibility](#chart-accessibility)
- [Image views](#image-views)
- [Text views](#text-views)
- [Web views](#web-views)
- [React Native mapping](#react-native-mapping)
- [Review checklist](#review-checklist)

## Charting data

Charts suit three purposes: analyzing trends over historical or predicted values, visualizing the current state of something changing over time, and comparing items across categories.

**Use a chart to highlight important information.** Charts are visually prominent and draw attention — earn that attention by making clear what people can learn from the data.

**Keep charts simple; reveal detail on demand.** Resist packing in as much data as possible. Too much data is visually overwhelming and obscures the very relationships you're trying to show. Let people choose different levels of detail or subsets of data. To teach an interactive chart, you can offer several versions, each with more functionality than the last.

**Prefer common chart types** — bar, line — because people already know how to read them.

**If you must present data novelly, teach people how to read it.** When a Watch pairs with iPhone, Activity introduces its rings by animating each one individually, showing how each maps to Move, Exercise, and Stand.

**Examine the data at multiple levels to find what's worth showing.** Macro level suggests summaries like totals and averages; mid level suggests useful subsets; individual data points suggest values worth drawing attention to.

**Add descriptive text.** Titles, subtitles, and annotations emphasize the most important information and highlight actionable takeaways. A brief headline or summary lets people grasp the essentials at a glance — Weather puts "Chance of light rain in the next hour" above the hourly forecast list. **A descriptive headline doesn't replace accessibility labels.**

**Match the chart's size to its functionality, topic, and detail level.** Large enough to read details and descriptive text comfortably, and expansive enough for the interactivity you support. A small chart works for glanceable single-item information or as a preview of a larger version.

**Prefer consistency across multiple charts, deviating only to highlight differences.** Charts serving a similar purpose shouldn't look unrelated. Consistency also lets people transfer what they learned about one chart to another.

**Maintain continuity across charts using the same data.** One chart type, consistent colors, annotations, layouts, and descriptive text signal that the dataset is the same. Health Trends uses a specific visual style per small trend chart, and the expanded version reuses the same style, colors, marks, and annotations.

## Chart anatomy

### Marks

**Choose the mark type by what you want to communicate** — bar, line, point, area, and others.

**Combine mark types when it adds clarity** — point marks on a line chart highlight individual values while the line carries the trend.

### Axes

**Fixed vs. dynamic range.** Use a **fixed** range when specific minimum and maximum values are meaningful across all data — battery charge is always 0–100%. Use **dynamic** when bounds should follow the data.

**Choose the lower bound based on mark type and use.** Zero works well for **bar** charts, because relative bar heights then give a reasonable estimate of values. But zero can **hide** meaningful differences: a heart rate chart anchored at zero obscures the difference between resting and active readings, since those differences occur far from zero.

**Prefer familiar tick sequences.** 0, 5, 10 is instantly readable; 1, 6, 11 follows the same rule but costs people a moment of thought.

**Tune grid line density and weight to the use case.** Too many grid lines overwhelm and distract from the data; too few make estimating values hard. If people can inspect individual points by interacting, you can use fewer grid lines and lighter label colors so the data stays prominent.

### Hierarchy and layout

**Data most prominent**, with descriptions and axes providing context without competing.

**In a compact environment, maximize the plot area width.** Keep vertical axis labels as short as possible without losing clarity; describe units elsewhere (in the title), and put a long category label **inside** the plot area where it won't obscure anything.

**Align the chart with surrounding interface elements.** Align its leading edge with other views. To keep a clean leading edge: put each vertical grid line's label on its **trailing** side, and consider shifting the **Y axis to the trailing side** so tick labels don't protrude past the chart's leading edge. If a label ends up looking unattached, anchor it to a grid line with a tick.

### Interaction

**Let people interact where it makes sense, but never require interaction to reveal critical information.** Stocks shows the performance line for the chosen period directly, and lets people drag a vertical indicator to read individual values.

**Make interaction physically easy.** Chart marks are often too small to target with a finger or pointer. **Expand the hit target to the entire plot area** and let people scrub across it.

**Support keyboard and Switch Control navigation.** By default these visit elements in a linear sequence, which may not match the chart's logic. Two approaches: specify a logical, predictable path (navigating along the X axis rather than jumping around), or — for very large datasets — let people move focus among **subsets** of values rather than every point. Both also improve VoiceOver, even in a non-interactive chart.

**Help people notice changes.** Unnoticed changes to marks or axes cause misreading. Animate them — but **also** highlight changes in other ways, so VoiceOver users and people with animations off learn about them too.

### Color

**Never rely on color alone.** Supplement with **different shapes or patterns**: Health uses two different point-mark shapes, not just two colors, for the two components of blood pressure.

**Add visual separation between contiguous color areas.** In a stacked bar with a different color per mark, separators between marks help distinguish them.

## Chart accessibility

**Every chart must be fully accessible.** This is the part most often skipped, and a chart without it is simply unavailable to some people.

**Consider Audio Graphs.** It provides chart data to VoiceOver, which renders a set of tones representing values and their trend, and it accepts high-level text summaries. **If you don't use Audio Graphs**, you must provide an overview yourself: identify the chart **type** (bar, line), explain **what each axis represents**, and describe the **upper and lower bounds**.

**Write accessibility labels that support the chart's purpose** — and note this determines their *granularity*. Maps' cycling elevation chart exists to convey the overall terrain, so it labels **summaries of elevation change over route portions**. Health's Steps chart exists to give actual counts, so it labels **each bar**. Labeling every point in the Maps case would be noise; summarizing in the Health case would withhold the data.

Guidelines for the labels themselves:

- **Prioritize clarity and comprehensiveness.** A bare value is rarely enough — include context like the date or location. But don't repeat what Audio Graphs or your overview already provides (like the axis name). Context first, then a succinct description of details.
- **Avoid subjective terms** — "rapidly", "gradually", "almost" communicate your interpretation. Use actual values so people form their own.
- **Avoid ambiguous formats and abbreviations.** "June 6" beats "6/6"; "60 minutes" or "60 meters" beats "60m".
- **Describe what details represent, not what they look like.** Identify what each series *is*; don't describe that one is red and one is blue.
- **Be consistent about axis order** throughout the app, so people don't have to work out which axis a description refers to.

**Hide visible axis and tick labels from assistive technologies.** They exist for visually assessing trends and estimating values; VoiceOver users get that through accessibility labels and Audio Graphs, so exposing the visible labels is redundant.

**watchOS:** avoid requiring complex chart interactions. Prefer glanceable information with simple interactions where they add value. If you have a version on another platform, use that for detail — Heart Rate on watchOS shows the current day, while Health on iPhone shows multiple periods and individual marks.

## Image views

**Use an image view when the view's purpose is simply to display an image.** For a rare interactive image, configure a **system button** to display the image rather than adding button behavior to an image view — the button brings the correct states and accessibility with it.

**For an icon, use a symbol or interface icon, not an image view.** SF Symbols are vector-based and render with various colors and opacities; an interface icon is typically a bitmap whose non-transparent pixels can receive color. Both can use people's chosen accent colors.

**Take care overlaying text on images.** Compositing reduces both image clarity and text legibility. Ensure contrast, and consider a text shadow or background layer.

**Use a consistent size for all images in an animated sequence.** Pre-scaling to fit the view means the system doesn't scale at all; where it must, uniform sizes and shapes perform better.

**macOS:** an editable image view is an **image well** (copy, paste, drag, Delete to clear). A clickable image is an **image button**, not an image view.

## Text views

**Use a text view for text that's long, editable, or specially formatted.** Small non-editable text → label. Small editable text → text field.

**Keep text legible.** Multiple fonts, colors, and alignments are available, but readability comes first. **Adopt Dynamic Type**, and test with accessibility options like Bold Text on.

**Make useful text selectable** — error messages, serial numbers, IP addresses.

**Show the appropriate keyboard type** on iOS/iPadOS.

## Web views

**Support forward and back navigation when appropriate.** Web views support it but **not by default** — enable it and provide the corresponding controls if people are likely to visit multiple pages.

**Don't build a web browser.** A web view for brief access to a website without leaving your app's context is fine. Replicating Safari is unnecessary and discouraged.

## React Native mapping

### Charts

RN has no Swift Charts equivalent. The realistic options and their tradeoffs:

```jsx
// react-native-skia + d3-scale/d3-shape: full control, GPU-accelerated,
// best fit for custom marks and interaction. Most work.
// victory-native (v40+, Skia-based): declarative, reasonable defaults.
// react-native-gifted-charts / react-native-chart-kit: quickest, least control.
// A WebView + a JS charting library: avoid — it breaks accessibility entirely
// and can't participate in Dynamic Type or the accessibility tree.
```

Whichever you pick, the accessibility work is yours to do, and it's the part libraries do not supply:

```jsx
// One accessible element for the chart as a whole, at the right granularity.
// Maps-style (summary) vs. Health-style (per-mark) — decide from the chart's purpose.
<View
  accessible
  accessibilityRole="image"
  // The overview Audio Graphs would otherwise provide: type, axes, bounds.
  accessibilityLabel={
    `Bar chart of daily steps. Horizontal axis: days, June 1 to June 7. ` +
    `Vertical axis: steps, 0 to 12,000. Average 8,240 steps.`
  }
>
  <StepsChart data={data} />
</View>
```

For a per-mark chart (Health-style), expose each mark as its own element with a contextualized label:

```jsx
{data.map(d => (
  <View
    key={d.date}
    accessible
    accessibilityRole="text"
    // Context first, then detail. Absolute values, no subjective terms,
    // unabbreviated units and dates.
    accessibilityLabel={`${formatFullDate(d.date)}, ${d.steps.toLocaleString()} steps`}
  >
    <Bar height={scale(d.steps)} />
  </View>
))}
```

And hide the visible axis labels, since that information is already in the labels above:

```jsx
<Text accessibilityElementsHidden importantForAccessibility="no-hide-descendants">
  {tickLabel}
</Text>
```

Shape or pattern in addition to color:

```jsx
// Two series distinguished by dash pattern and point shape, not only hue.
<Path stroke={theme.series1} strokeDasharray={undefined} />
<Path stroke={theme.series2} strokeDasharray={[6, 4]} />
{/* systolic = circle, diastolic = square — the Health blood-pressure approach */}
```

Interaction with a plot-area-wide hit target:

```jsx
// Scrubbing across the whole plot area, not tapping tiny marks.
const pan = Gesture.Pan()
  .onBegin(e => setActiveIndex(indexFromX(e.x)))
  .onUpdate(e => setActiveIndex(indexFromX(e.x)));

<GestureDetector gesture={pan}>
  <View style={{ width: plotWidth, height: plotHeight }}>{/* full plot area is the target */}
    {marks}
    {activeIndex != null && <Indicator index={activeIndex} />}
  </View>
</GestureDetector>
```

Critical values must be visible without scrubbing — the indicator adds detail, it doesn't gate the message.

Axis bounds:

```js
// Fixed range where min/max are inherently meaningful.
const batteryScale = scaleLinear().domain([0, 100]).range([h, 0]);

// Dynamic range with padding — and NOT anchored at zero where that would
// flatten the signal (heart rate, weight, temperature).
const hr = extent(data, d => d.bpm);
const hrScale = scaleLinear().domain([hr[0] - 5, hr[1] + 5]).range([h, 0]);

// Bars DO anchor at zero, so heights compare correctly.
const barScale = scaleLinear().domain([0, max(data, d => d.steps)]).range([h, 0]);
```

For palettes and mark styling that stay consistent and accessible across light and dark, load the `dataviz` skill rather than picking colors ad hoc.

Animate mark changes, but announce them too:

```js
// Animation alone is invisible to VoiceOver users and to Reduce Motion users.
AccessibilityInfo.announceForAccessibility(`Chart updated. ${summary}`);
```

### Image views

```jsx
// Non-interactive: an Image with a label (or hidden, if decorative).
<Image source={photo} accessibilityRole="image" accessibilityLabel="Bridge at sunset"
  resizeMode="cover" style={{ width: '100%', aspectRatio: 3 / 2 }} />

// Interactive: a Pressable that contains the image — the button owns the semantics.
<Pressable onPress={open} accessibilityRole="button" accessibilityLabel="Open photo">
  <Image source={photo} accessibilityElementsHidden importantForAccessibility="no-hide-descendants" />
</Pressable>
```

For icons, use SF Symbols rather than an `<Image>` — see [icons-and-images.md](../foundations/icons-and-images.md).

Text over images needs a legibility layer, not just a color choice:

```jsx
<View>
  <Image source={hero} style={StyleSheet.absoluteFill} />
  {/* Gradient scrim, so contrast holds regardless of the image. */}
  <LinearGradient colors={['transparent', 'rgba(0,0,0,0.6)']} style={StyleSheet.absoluteFill} />
  <Text style={{ color: '#fff' }}>{title}</Text>
</View>
```

Use `expo-image` over the core `Image` for caching, `contentFit`, and cheap transitions — and keep animated sequences at a uniform size so no scaling happens at runtime.

### Text views

```jsx
// Long editable text.
<TextInput
  multiline
  scrollEnabled
  textAlignVertical="top"
  // Dynamic Type by default; don't disable scaling on body content.
  style={textStyles.body}
/>

// Long non-editable text people may want to copy.
<Text selectable style={textStyles.body}>{article.body}</Text>
```

### Web views

```jsx
import { WebView } from 'react-native-webview';

<WebView
  source={{ uri: url }}
  // Enable back/forward only if you also provide the controls.
  onNavigationStateChange={s => { setCanGoBack(s.canGoBack); setCanGoForward(s.canGoForward); }}
  ref={webRef}
/>
{canGoBack && <Pressable onPress={() => webRef.current.goBack()}><Icon name="chevron.left" /></Pressable>}
```

For opening an external link, prefer the system in-app browser over a `WebView` — it's Safari's engine with Safari's UI, sharing, and Reader, and it satisfies "don't build a browser" by construction:

```js
import * as WebBrowser from 'expo-web-browser';
await WebBrowser.openBrowserAsync(url);   // SFSafariViewController
```

Reserve `WebView` for content that's genuinely part of your app's UI.

## Review checklist

- [ ] Chart has a descriptive title and, where useful, a summary of its main message.
- [ ] Data is the most prominent element; axes and descriptions don't compete.
- [ ] Common chart type used, or novel encoding is explicitly taught.
- [ ] Mark type suits the message; combined marks used only where they add clarity.
- [ ] Axis range fixed or dynamic per the data's meaning; bar charts anchored at zero, non-comparative series not forced to zero.
- [ ] Tick values follow a familiar sequence; grid line density tuned to the use case.
- [ ] Multiple charts of the same data share type, colors, annotations, and layout.
- [ ] Chart leading edge aligns with surrounding views; axis labels don't protrude.
- [ ] Critical information visible without interaction.
- [ ] Hit target spans the whole plot area; scrubbing supported.
- [ ] Keyboard/Switch Control path through the chart is logical, or focus moves among subsets for large datasets.
- [ ] Series distinguished by shape or pattern as well as color; contiguous color areas separated.
- [ ] Chart exposes an overview (type, axes, bounds) to VoiceOver, via Audio Graphs or an explicit label.
- [ ] Accessibility label granularity matches the chart's purpose (summary vs. per-mark).
- [ ] Labels give context plus value, use absolute values, avoid subjective terms and ambiguous abbreviations, and describe meaning not appearance.
- [ ] Axis order referred to consistently across the app.
- [ ] Visible axis and tick labels hidden from assistive technology.
- [ ] Mark/axis changes announced, not only animated.
- [ ] Charts not rendered in a WebView.
- [ ] watchOS charts glanceable, with detail deferred to another platform.
- [ ] Interactive images are Pressables containing an image, with the image hidden from the accessibility tree.
- [ ] Icons use SF Symbols, not image views.
- [ ] Text over images has a scrim or shadow ensuring contrast.
- [ ] Animated image sequences use a uniform size.
- [ ] Long text uses a multiline input or selectable `Text`; Dynamic Type not disabled on body content.
- [ ] Web views offer back/forward controls if multi-page browsing is likely.
- [ ] External links open in the system in-app browser rather than a custom WebView browser.
