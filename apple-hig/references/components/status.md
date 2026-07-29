# Progress Indicators, Gauges, Activity Rings & Rating Indicators

Source: HIG › Components › Status — [Progress indicators](https://developer.apple.com/design/human-interface-guidelines/progress-indicators), [Gauges](https://developer.apple.com/design/human-interface-guidelines/gauges), [Activity rings](https://developer.apple.com/design/human-interface-guidelines/activity-rings), [Rating indicators](https://developer.apple.com/design/human-interface-guidelines/rating-indicators)

Loading strategy overall is in [patterns/feedback-and-loading.md](../patterns/feedback-and-loading.md).

## Contents

- [Progress indicators](#progress-indicators)
- [Gauges](#gauges)
- [Activity rings](#activity-rings)
- [Rating indicators](#rating-indicators)
- [React Native mapping](#react-native-mapping)
- [Review checklist](#review-checklist)

## Progress indicators

**Prefer determinate.** An indeterminate indicator shows something is happening but gives no basis for a decision. A determinate one lets people decide whether to do something else, come back later, or abandon the task.

**Report advancement accurately.** Consider **evening out the pace** so people can trust the estimate. 90% in five seconds followed by the last 10% in five minutes makes people wonder whether the app is working — and reads as deceptive.

**Keep the indicator moving.** A stationary indicator reads as a stalled process or a frozen app. If a process genuinely stalls, provide feedback explaining the problem and what people can do.

**Switch indeterminate → determinate** as soon as you can compute duration. **Never switch circular → bar** — spinners and bars are different shapes and sizes, so transitioning disrupts the layout and confuses.

**Add a description only if it provides context**, and be accurate and succinct. **"Loading" and "authenticating" rarely add value** — they say nothing the indicator didn't.

**Keep the indicator in a consistent location** so people can reliably find operation status across screens and platforms.

**Let people halt processing where feasible.** A **Cancel** button if interruption has no negative side effects. Add a **Pause** button too if interruption would cost something — like the downloaded portion of a file.

**Warn when halting has a consequence.** If cancelling loses progress, present an alert offering confirmation or resumption.

### Platform notes

- **iOS/iPadOS** — **refresh automatically as well as on demand.** Pull-to-refresh is appreciated, but people also expect periodic automatic refreshes; don't make them responsible for every update. A refresh control **can** have a title, but usually shouldn't — the animation already says content is loading. If you include one, make it **valuable information about the content**, not instructions: Podcasts states when the last update occurred.
- **macOS** — **prefer a spinner** for background operations and constrained spaces: retrieving messages from a server, or progress within a text field or beside a button. **Don't label a spinner** — it appears in response to something the person just initiated, so a label is redundant.

## Gauges

A gauge shows a current value within a range.

**Write succinct labels for the current value and both endpoints.** Not every gauge style displays all of them, but **VoiceOver reads the visible labels**, which is how the gauge becomes usable without sight.

**Consider a gradient fill that communicates the gauge's purpose** — a temperature gauge running red to blue for hot to cold.

### macOS

**Use the continuous style for large ranges** — a discrete capacity indicator's segments become too small to be useful.

**Consider changing fill color at significant thresholds.** The default for both capacity indicator styles is green; you can change it at very low, very high, or past-the-middle values, either for the whole indicator or using the **tiered** state to show several colors in one.

## Activity rings

Activity rings are a system element with strict usage rules — this is one of the few components where Apple is explicit about what constitutes misuse.

**Display them when relevant to your app's purpose** — health or fitness apps, especially those contributing to HealthKit. Good placements: a workout metrics screen so people track progress during a session, or a post-workout summary so they can check daily goal progress.

**Use them only for Move, Exercise, and Stand.** Never replicate or modify them for other purposes. Never show other data in them. **Never show Move/Exercise/Stand progress in another ring-like element** — the mapping is bidirectional.

**One person only.** Never represent more than one person's data, and make whose progress it is obvious with a label, photo, or avatar.

**Keep the visual appearance identical everywhere** you display them.

**Use the matching ring colors** for any label or value directly associated with a ring — for the *Move*, *Exercise*, *Stand* labels themselves and for current/goal values.

**Maintain the outer margin** — no less than the distance between rings. Nothing may crop, obstruct, or encroach on that margin or the rings.

**Differentiate other ring-like elements.** Mixing ring styles is visually confusing. Separate them with padding, lines, or labels; color and scale help too.

**Don't duplicate system notifications.** The system already sends Move/Exercise/Stand progress updates, so repeating them is confusing. **Don't show Activity rings in your notifications** — referencing Activity progress is fine if it's done in a way unique to your app.

**Never use them for decoration** — not in labels, not in background graphics.

**Never use them for branding** — not in your app icon, not in marketing.

## Rating indicators

**Let people change rankings inline.** In a list of ranked items, allow adjusting an individual item's rank without navigating to a separate editing screen.

**If you replace the star, make the purpose clear.** The star is a highly recognizable ranking symbol; other symbols may not read as a rating scale at all.

## React Native mapping

### Determinate progress

```jsx
// Prefer this whenever progress is computable.
<View
  accessibilityRole="progressbar"
  accessibilityLabel="Downloading"
  accessibilityValue={{ min: 0, max: 100, now: Math.round(progress * 100),
                        text: `${Math.round(progress * 100)} percent` }}
  style={s.track}
>
  <View style={[s.fill, { width: `${progress * 100}%` }]} />
</View>
```

`accessibilityValue` is what makes progress perceivable at all to VoiceOver users — a bare animated bar tells them nothing.

Even out the pace so the estimate is trustworthy:

```js
// Bytes-based progress often jumps. Smooth it rather than reporting raw jumps,
// and never let it appear to move backwards.
const smoothed = useRef(0);
smoothed.current = Math.max(smoothed.current, raw * 0.3 + smoothed.current * 0.7);
```

Don't fake progress for an unknown duration — an invented bar that stalls at 95% is exactly the deceptive pattern the guidance warns against. Use an indeterminate indicator instead, and switch once you know the duration:

```jsx
{total == null
  ? <ActivityIndicator />                   // unknown duration
  : <ProgressBar value={loaded / total} />} // switch as soon as total is known
```

Keep the shape stable — never swap `ActivityIndicator` for a bar mid-operation; render the bar from the start with an indeterminate animation if you need one shape throughout.

### Cancel and pause

```jsx
// Cancel when interruption is free; add Pause when it costs progress.
<View style={{ flexDirection: 'row', gap: 12 }}>
  <ProgressBar value={progress} />
  {isResumable && <Pressable onPress={pause} accessibilityLabel="Pause download"><Icon name="pause" /></Pressable>}
  <Pressable onPress={confirmCancel} accessibilityLabel="Cancel download"><Icon name="xmark" /></Pressable>
</View>
```

```js
// Warn when cancelling loses work.
function confirmCancel() {
  if (progress > 0.05) {
    Alert.alert('Cancel Download?', 'The part already downloaded will be discarded.',
      [{ text: 'Keep Downloading', style: 'cancel' }, { text: 'Cancel Download', style: 'destructive', onPress: cancel }]);
  } else cancel();
}
```

### Pull-to-refresh plus automatic refresh

```jsx
<FlatList
  refreshControl={
    <RefreshControl
      refreshing={isRefreshing}
      onRefresh={refetch}
      // Title only if it carries information — not instructions.
      title={Platform.OS === 'ios' ? `Updated ${formatRelative(lastSync)}` : undefined}
    />
  }
/>
```

And refresh on your own schedule too, so people aren't the only trigger:

```js
// Refetch on focus and on an interval — don't make manual pull the only path.
useFocusEffect(useCallback(() => { refetch(); }, []));
useInterval(refetch, 5 * 60 * 1000);
```

### Spinners

```jsx
// macOS/inline: unlabelled, small, near the thing it describes.
<View style={{ flexDirection: 'row', alignItems: 'center', gap: 8 }}>
  <ActivityIndicator size="small" />
  {/* No "Loading…" label — the spinner already says that. */}
</View>
```

Never render a full-screen `ActivityIndicator` as a screen's loading state — use skeletons, per [patterns/feedback-and-loading.md](../patterns/feedback-and-loading.md).

### Gauges and rings

```jsx
// react-native-svg for arc gauges; label current value and both endpoints
// so VoiceOver has something to read.
<View accessible accessibilityLabel={`Temperature ${value}°, range ${min}° to ${max}°`}>
  <Svg>
    <Defs><LinearGradient id="temp"><Stop offset="0" stopColor="#0A84FF" /><Stop offset="1" stopColor="#FF453A" /></LinearGradient></Defs>
    <Path d={arcPath} stroke="url(#temp)" />
  </Svg>
  <Text>{min}°</Text><Text>{value}°</Text><Text>{max}°</Text>
</View>
```

**Activity rings cannot be reimplemented.** The rules above prohibit replicating or modifying them, and prohibit showing Move/Exercise/Stand data in any other ring-like element. In RN that means:

```jsx
// Don't do this — a hand-drawn three-ring component showing HealthKit
// Move/Exercise/Stand data violates the guidance either way it's read.

// Instead: read the values via a HealthKit bridge and present them in a form
// that's clearly your own — bars, numbers, a distinct chart.
<StepsSummary move={move} exercise={exercise} stand={stand} />
```

If you genuinely need the system rings, that's a native SwiftUI view (`ActivityRingsView` / `HKActivityRingView`) embedded via a native component — and if you do embed them, give surrounding elements the required outer margin and don't place other rings nearby.

### Rating indicators

```jsx
// Inline adjustment — no separate edit screen.
<View accessibilityRole="adjustable" accessibilityLabel="Rating"
      accessibilityValue={{ min: 0, max: 5, now: rating, text: `${rating} of 5 stars` }}>
  {[1, 2, 3, 4, 5].map(n => (
    <Pressable key={n} onPress={() => setRating(n)} hitSlop={8}>
      {/* Keep the star — a custom symbol may not read as a rating. */}
      <Icon name={n <= rating ? 'star.fill' : 'star'} color={PlatformColor('systemYellow')} />
    </Pressable>
  ))}
</View>
```

Note `hitSlop` — five stars in a row are individually far below 44 pt, so the touch targets need expanding.

App Store rating prompts are a different thing entirely; see [patterns/feedback-and-loading.md](../patterns/feedback-and-loading.md).

## Review checklist

- [ ] Determinate progress used wherever duration is computable; indeterminate only when it genuinely isn't.
- [ ] Progress never fabricated or allowed to move backwards; pace smoothed.
- [ ] Indicator shape stays constant — no circular↔bar switch mid-operation.
- [ ] Indicator keeps moving; stalls produce an explanatory message.
- [ ] `accessibilityRole="progressbar"` with min/max/now on every progress element.
- [ ] Descriptions omitted unless informative — no "Loading…" beside a spinner.
- [ ] Progress appears in a consistent location across the app.
- [ ] Cancel offered where interruption is safe; Pause added where it costs progress.
- [ ] Cancelling meaningful progress asks for confirmation.
- [ ] Pull-to-refresh supplemented by automatic refresh; refresh title carries information, not instructions.
- [ ] No full-screen spinner as a screen's loading state.
- [ ] Gauges label the current value and both endpoints; gradient fills communicate meaning.
- [ ] Activity rings not reimplemented, modified, or used for anything other than Move/Exercise/Stand.
- [ ] Move/Exercise/Stand data not shown in any other ring-like element.
- [ ] Activity rings show one person, clearly identified, with the required outer margin preserved.
- [ ] No Activity rings in notifications, decoration, app icon, or marketing.
- [ ] Notifications don't duplicate the system's Activity progress updates.
- [ ] Rating adjustable inline; star retained unless a substitute is unmistakably a rating.
- [ ] Rating stars have expanded hit targets and an `adjustable` accessibility role.
