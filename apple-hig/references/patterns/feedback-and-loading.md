# Feedback, Loading, Undo & Ratings

Source: HIG › Patterns › [Feedback](https://developer.apple.com/design/human-interface-guidelines/feedback), [Loading](https://developer.apple.com/design/human-interface-guidelines/loading), [Undo and redo](https://developer.apple.com/design/human-interface-guidelines/undo-and-redo), [Ratings and reviews](https://developer.apple.com/design/human-interface-guidelines/ratings-and-reviews)

Read this when handling async state, showing errors or confirmations, building destructive actions, or asking for an App Store rating.

## Contents

- [Feedback](#feedback)
- [Loading](#loading)
- [Undo and redo](#undo-and-redo)
- [Ratings and reviews](#ratings-and-reviews)
- [Platform differences](#platform-differences)
- [React Native mapping](#react-native-mapping)
- [Review checklist](#review-checklist)

## Feedback

Feedback tells people what's happening, what they can do next, the result of what they did, and how to avoid mistakes. Four things worth feedback:

- Current status of something
- Success or failure of an important task
- A warning about an action with negative consequences
- An opportunity to correct a mistake

**Make all feedback multi-modal.** Color, text, sound, and haptics together mean the message lands whether someone silenced their device, looked away, or uses VoiceOver. Any single channel excludes somebody.

**Integrate status feedback into the interface, near what it describes.** Mail shows the last-updated time and unread count in the mailbox toolbar — unobtrusive, always available, no action required. Compare that with a toast that appears and vanishes: the information exists for two seconds and then only in memory.

**Reserve alerts for critical, ideally actionable information.** Alerts interrupt by design, so the importance must match the interruption. Overuse burns their impact — an app whose alerts are usually ignorable has no way left to say something urgent.

**Warn only about unexpected, irreversible data loss.** Don't warn when loss is the *expected* result: the Finder doesn't ask before you trash a file. A confirmation on an obvious action teaches people to dismiss confirmations reflexively, which is exactly what breaks the one that matters.

**Confirm completion only for significant actions.** People expect success, so they mainly need to hear about *failure*. Apple Pay's confirmation is worth it; a "Saved!" toast on every autosave is not.

**When a command can't be carried out, say why.** Maps doesn't just fail on a directions request with no destination — it explains that it can't route to and from the same place.

## Loading

**The best loading experience finishes before people notice it.** Everything below is mitigation.

**Show something as soon as possible.** If nothing appears until loading finishes, people read the blankness as a bug. Show placeholder text, graphics, or animations and replace them as content arrives.

**Let people do other things while content loads.** Load in the background so other actions remain available.

**If the wait is unavoidably long, give people something worth looking at** — gameplay hints, tips, new-feature introductions. Gauge the remaining time reasonably: too little and the content flashes by, too much and you have to loop it.

**Download large assets in the background** — right after install, during updates, or at other nondisruptive times — rather than during launch.

**Determinate vs. indeterminate:** determinate when you know how long it will take, indeterminate when you don't. Prefer determinate wherever you can compute progress, because an indeterminate spinner gives people no basis for deciding whether to wait.

**Custom loading views make sense in games**, where a system progress indicator can feel foreign.

## Undo and redo

**Help people predict the result.** On iPhone, describe the operation in the shake-to-undo alert. In menus, label items specifically: "Undo Typing", "Redo Bold". The alert title automatically prefixes `"Undo "` / `"Redo "` (with the trailing space) — you supply a word or two after it.

**Show the result of the undo.** If the affected content has scrolled out of view, an undo appears to do nothing, and people will repeat it — compounding the confusion. Scroll to the restored paragraph, highlight the change.

**Allow multiple undos.** People expect to undo everything back to a logical checkpoint like opening or saving the document. Arbitrary limits feel broken.

**Consider batched undo** for a run of related changes — incremental adjustments to one property — so people don't step through each one. Also consider "revert all changes since open/save".

**Provide undo/redo buttons only when necessary.** The expected entry points are system ones: the Edit menu on macOS, keyboard shortcuts on Mac and iPad, shake on iPhone. If you do add buttons, use the standard symbols (`arrow.uturn.backward` / `arrow.uturn.forward`) in a toolbar.

**Don't redefine the standard gestures** — three-finger swipe, shake — for anything else.

## Ratings and reviews

**Ask only after demonstrated engagement** — a completed level, a finished significant task. **Never on first launch or during onboarding**: people have no opinion yet, and being asked prematurely makes negative feedback *more* likely.

**Don't interrupt a task.** Find natural breaks and stopping points.

**Don't pester.** At least a week or two between requests, and only after further engagement.

**Use the system prompt.** It checks for previous feedback, allows a rating plus optional written review in one tap, lets people opt out globally, and **automatically caps display at three times per app per 365 days**. A custom prompt has none of that and can't write to the App Store anyway.

**Think twice before resetting your summary rating** on a new version. It makes ratings reflect the current build, but leaves you with fewer total ratings, which discourages downloads.

## Platform differences

### watchOS

**Avoid indeterminate progress indicators entirely.** An animated indicator implies people should keep watching the screen, which contradicts how a watch is used. Instead, tell people they'll get a **notification** when the process completes.

**Still prefer a loading indicator over a blank screen** for one-to-two-second waits — the rule is against *indeterminate, attention-demanding* indicators, not against all feedback.

### macOS

Undo and redo belong at the top of the **Edit menu**, with ⌘Z and ⇧⌘Z supported. Mac users will look there first and nowhere else.

### tvOS / visionOS help content

Match help imagery to the actual input device — don't show a game controller when someone is using the Siri Remote. And use platform-correct verbs: never "click" on iPhone, never "tap" on Mac.

## React Native mapping

### Never render a bare spinner as the whole screen

```jsx
// Wrong: blank until loaded — reads as broken.
if (isLoading) return <ActivityIndicator />;

// Right: skeleton in the shape of the real content, replaced progressively.
function Feed({ items, isLoading }) {
  const data = isLoading ? SKELETON_ROWS : items;
  return (
    <FlatList
      data={data}
      renderItem={({ item }) => (item.skeleton ? <RowSkeleton /> : <Row {...item} />)}
    />
  );
}
```

Skeletons in the layout of the real content also prevent the layout shift that happens when a centered spinner is swapped for a list.

Under Reduce Motion, stop the skeleton shimmer — it's a repetitive animation:

```jsx
const reduced = useReducedMotion();
<RowSkeleton animated={!reduced} />
```

### Prefer determinate progress

```jsx
// If the API gives you total bytes, show real progress.
const task = FileSystem.createDownloadResumable(url, dest, {}, ({ totalBytesWritten, totalBytesExpectedToWrite }) => {
  setProgress(totalBytesWritten / totalBytesExpectedToWrite);
});

<Progress value={progress} accessibilityRole="progressbar"
  accessibilityValue={{ min: 0, max: 100, now: Math.round(progress * 100) }} />
```

Note the `accessibilityValue` — progress is exactly the case where VoiceOver users otherwise get nothing at all.

### Status in place, not in a toast

```jsx
// The Mail pattern: status lives next to what it describes and persists.
<View style={s.listHeader}>
  <Text style={textStyles.footnote}>
    {isRefreshing ? 'Updating…' : `Updated ${formatRelative(lastSync)}`}
  </Text>
</View>
```

Use `RefreshControl` for pull-to-refresh rather than a custom spinner — it's the gesture and appearance people already know:

```jsx
<FlatList refreshControl={<RefreshControl refreshing={isRefreshing} onRefresh={refetch} />} />
```

### Multi-modal feedback

```js
import * as Haptics from 'expo-haptics';
import { AccessibilityInfo } from 'react-native';

async function reportSuccess(message) {
  Haptics.notificationAsync(Haptics.NotificationFeedbackType.Success);  // felt
  setBanner({ tone: 'success', message });                              // seen (text + color)
  AccessibilityInfo.announceForAccessibility(message);                  // heard
}

async function reportFailure(message) {
  Haptics.notificationAsync(Haptics.NotificationFeedbackType.Error);
  setBanner({ tone: 'error', message, icon: 'exclamationmark.triangle' }); // icon, not just red
  AccessibilityInfo.announceForAccessibility(message);
}
```

The icon on the error banner is the "never color alone" rule; the announce call is what makes the banner exist for VoiceOver users, who otherwise never learn it appeared.

Haptics are iOS-strong and Android-weak — check availability rather than assuming, and never make haptics the only channel.

### Alerts, sparingly and natively

```js
import { Alert } from 'react-native';

// Destructive + irreversible + unexpected → confirm.
Alert.alert(
  'Delete Draft?',                       // title names the consequence
  'This draft hasn\'t been saved and can\'t be recovered.',
  [
    { text: 'Cancel', style: 'cancel' },
    { text: 'Delete', style: 'destructive', onPress: onDelete },
  ],
);
```

`style: 'destructive'` renders red on iOS — use it, because a destructive action styled as ordinary is a real hazard. And note what *doesn't* get an alert: deleting an item the user explicitly swiped to delete is the expected result, so it gets an undo affordance instead.

For choices rather than warnings, use an action sheet (`ActionSheetIOS` or `@expo/react-native-action-sheet`), not an alert.

### Undo instead of confirm

For expected-but-reversible destruction, undo beats confirmation — fewer interruptions, same safety:

```jsx
function useUndoableDelete() {
  const pending = useRef(null);

  function remove(item) {
    setItems(prev => prev.filter(i => i.id !== item.id));      // optimistic
    // Show the result AND the way back, per "show the results of an undo".
    setSnackbar({ message: `Deleted "${item.title}"`, actionLabel: 'Undo', onAction: undo });
    pending.current = setTimeout(() => commitDelete(item), 5000);
  }

  function undo() {
    clearTimeout(pending.current);
    setItems(prev => restore(prev, item));
    // Scroll the restored row into view — otherwise undo looks like it did nothing.
    listRef.current?.scrollToItem({ item });
    setSnackbar(null);
  }
  return { remove, undo };
}
```

Note the `scrollToItem`: that's the HIG rule about showing the undo's effect, and it's what stops people undoing repeatedly.

For a real edit history, keep a stack rather than a single slot, so multiple undos work:

```js
const [past, setPast] = useState([]);      // no arbitrary depth cap
const [future, setFuture] = useState([]);
```

RN has no shake-to-undo or Edit-menu integration out of the box. On iPad/macOS, wire the standard keys:

```jsx
// react-native-keyevent, or native-stack's keyboard shortcut APIs.
useKeyCommand({ input: 'z', modifiers: ['command'] }, undo);
useKeyCommand({ input: 'z', modifiers: ['command', 'shift'] }, redo);
```

### Ratings via the system prompt

```js
import * as StoreReview from 'expo-store-review';

// Gate on engagement, not on launch count alone.
async function maybeRequestReview() {
  if (completedTasks < 3) return;                       // demonstrated engagement
  if (Date.now() - lastAsked < 14 * 24 * 3600 * 1000) return;  // ≥ 2 weeks
  if (isMidTask) return;                                // natural break only
  if (!(await StoreReview.hasAction())) return;
  await StoreReview.requestReview();                    // system prompt; OS caps to 3/year
  setLastAsked(Date.now());
}
```

`requestReview()` may silently do nothing — the OS decides. So never gate app flow on it, never show your own "did you rate us?" follow-up, and never route people to the App Store manually as a fallback.

### Explaining failures

```jsx
// Don't just fail — say why, and offer the fix. See typography.md for copy rules.
{error && (
  <View style={s.inlineError}>
    <Icon name="exclamationmark.triangle" />
    <Text>{error.userMessage}</Text>{/* "No destination set. Choose a destination to get directions." */}
    {error.retryable && <Pressable onPress={retry}><Text>Try Again</Text></Pressable>}
  </View>
)}
```

Place it next to the cause, not at the top of the screen — the same rule that governs form field errors.

## Review checklist

- [ ] No screen renders a bare centered spinner as its only loading state; skeletons match real content shape.
- [ ] Determinate progress used wherever progress is computable; `accessibilityValue` set.
- [ ] Skeleton/shimmer animation stops under Reduce Motion.
- [ ] People can still act while background loading proceeds.
- [ ] Status feedback lives next to what it describes and persists, rather than only flashing as a toast.
- [ ] Feedback available through at least two channels (visual + haptic/announced).
- [ ] Errors and successes announced to VoiceOver.
- [ ] Alerts reserved for critical, actionable information; action sheets used for choices.
- [ ] Destructive actions use `style: 'destructive'`.
- [ ] Confirmations only for unexpected irreversible loss; expected deletions offer undo instead.
- [ ] Failures explain the cause and the remedy, positioned next to the cause.
- [ ] Success confirmations only for significant actions.
- [ ] Undo supports multiple steps with no arbitrary cap; result scrolled into view.
- [ ] Undo/redo labels name the operation; standard symbols used if buttons exist.
- [ ] Standard undo gestures and ⌘Z / ⇧⌘Z not redefined; wired up on iPad/macOS.
- [ ] Review prompt uses the system API, gated on engagement, ≥ 2 weeks apart, never on first launch or during onboarding or mid-task.
- [ ] No app flow depends on the review prompt actually appearing.
- [ ] watchOS: no indeterminate spinners; completion notification promised instead.
- [ ] Help and error copy uses platform-correct verbs and matches the actual input device.
