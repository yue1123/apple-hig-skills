# Modality, Full Screen & Multitasking

Source: HIG › Patterns › [Modality](https://developer.apple.com/design/human-interface-guidelines/modality), [Going full screen](https://developer.apple.com/design/human-interface-guidelines/going-full-screen), [Multitasking](https://developer.apple.com/design/human-interface-guidelines/multitasking)

Read this when presenting a sheet, modal, or full-screen view, or when handling backgrounding, audio interruption, and app-switch lifecycle.

## Contents

- [When modality is justified](#when-modality-is-justified)
- [Modality rules](#modality-rules)
- [Full screen](#full-screen)
- [Multitasking and lifecycle](#multitasking-and-lifecycle)
- [Platform differences](#platform-differences)
- [React Native mapping](#react-native-mapping)
- [Review checklist](#review-checklist)

## When modality is justified

Modality presents content in a dedicated mode that blocks interaction with the parent view and requires an explicit dismissal. Four legitimate reasons:

- Ensure people receive critical information, and can act on it if needed
- Offer options to confirm or modify their most recent action
- Help them perform a **distinct, narrowly scoped** task without losing their previous context
- Give an immersive experience, or help them concentrate on a complex task

**Present modally only when there's a clear benefit.** A modal takes people out of their context and demands an action to leave. If it isn't helping them focus or making a choice that affects their content or device, it's a cost with no return.

## Modality rules

**Keep modal tasks simple, short, streamlined.** A complicated modal makes people lose track of the task they suspended to enter it — especially when the modal obscures the context they came from.

**Never let a modal feel like an app inside your app.** A hierarchy of views inside a modal is the specific failure mode: people forget how to retrace their steps. If subviews are unavoidable, provide **a single path** through the hierarchy, and avoid buttons that could be mistaken for the dismiss button.

**Always provide an obvious dismissal, following platform convention:**

| Platform | Expected dismissal |
|---|---|
| iOS, iPadOS, watchOS | Button in the top toolbar, or swipe down |
| macOS, tvOS | Button in the main content view |

**Confirm before closing if user content would be lost** — whether they used the gesture or the button. Explain the situation and offer resolutions (on iOS, an action sheet with a save option). But **don't warn when data loss is the expected result** of the action: the Finder doesn't ask every time you trash a file.

**Title the modal after its task.** People arrive from somewhere else and may not return immediately; a task-naming title (plus descriptive text where useful) is how they keep their place.

**One modal at a time.** Dismiss the current one before presenting another. Stacked modals create clutter, and a modal hiding another modal adds real cognitive load. Alerts are the single exception — an alert may appear over anything, including other modals — but **never show two alerts at once**.

**Consider full-screen modal style for in-depth content or complex tasks** — video, photos, camera views, document markup, photo editing. On visionOS, a full-screen modal fills the window in the Shared Space and can become genuinely immersive in a Full Space.

## Full screen

Available on iPhone, iPad, and Mac. Support it when people want to concentrate or be immersed: games, media viewing, photo slideshows, in-depth focused tasks.

**Adjust the layout, don't programmatically resize the window.** With more space, keep essential content prominent and use the extra room well — adjusting proportions rather than adding or removing items. Keep the adjustment subtle enough that the transition between modes isn't jarring.

**Keep essential features and controls reachable** so the task can be completed without exiting. A full-screen media experience needs playback controls persistently available or trivially revealable.

**Let people choose when to exit.** Don't end full-screen mode automatically when they switch away or finish an activity.

**Pause on switch-away and resume on return.** A game or slideshow must pause so nothing is missed.

**Hiding chrome for content focus is fine** — toolbars and navigation controls can hide when content is primary (full-screen photos, reading). Restore them on a familiar gesture: tap, swipe down, or moving the cursor to the top of the screen. Keep controls visible when they're essential for navigation or the task itself.

## Multitasking and lifecycle

**Pause anything requiring attention or active participation when people switch away.** On return, let them continue as if they never left.

**Respond correctly to audio interruptions** — and the correct response depends on the interruption's kind:

- **Primary audio interruptions** (music, podcasts, audiobooks starting) → **pause indefinitely**.
- **Short interruptions** (GPS directions, a brief notification) → **duck the volume or pause briefly, then restore** the original volume/playback when it ends.

Getting this backwards — resuming after a music app takes over, or permanently stopping for a 2-second navigation prompt — is immediately noticeable.

**Finish user-initiated tasks in the background.** A download or video processing job that needs no further input should complete before suspension, not restart when the user comes back.

**Use notifications sparingly.** Notify on completion of *important or time-sensitive* tasks the person started and switched away from. Don't notify for routine or secondary work — let them find it done when they return.

## Platform differences

### iOS / iPadOS full screen

**Defer system gestures rather than fighting them.** By default the Home Screen indicator auto-hides shortly after switching to your app, reappearing when someone interacts with the bottom of the screen so one swipe exits. **Retain that default** — it's what people expect. Only if it causes genuine accidental exits should you require two swipes.

### iPadOS multitasking

Windows are freely resizable and apps get **no indication of the multitasking configuration** people choose — so you can't branch on it. Everything must be driven by the size you're actually given. See [layout.md](../foundations/layout.md).

### macOS

- **Use the system full-screen experience** rather than a custom one; the system handles cases like the camera housing at the top-center of some Macs automatically.
- **Let people enter full screen via the standard affordances**: the window's Enter Full Screen button, the View menu, or ⌃⌘F. Don't offer a custom menu of window modes.
- **In a game, don't change the display mode** when players go full screen. People expect to control their display mode, and switching it doesn't improve performance.
- **Let people reveal the Dock** in full-screen mode except in games (where you can defer the initial bottom-edge swipe, or hide the Dock outright).

### visionOS

- **Don't interfere with the system multitasking feedback.** When someone looks from one window to another, visionOS applies a feathered mask to the window they looked away from. Don't modify window edge appearance, or you'll break that cue.
- **Don't pause video when people look away.** As on macOS, playback started in one window continues while attention is elsewhere.
- **Expect your audio to duck** whenever you're not the Now Playing app and someone looks at another app.
- Closing the Now Playing app's window pauses playback automatically; people can resume from Control Center without reopening it.

### watchOS

- **Avoid indeterminate progress indicators.** An animated spinner suggests people should keep watching the display, which is the wrong interaction model for a watch. Instead, reassure them they'll get a notification when the process completes.
- Prefer showing content immediately. If loading needs a second or two, a loading indicator still beats a blank screen.

## React Native mapping

### Sheets, not full-screen modals, for most tasks

```jsx
// react-navigation: a modal group with the iOS sheet presentation.
<Stack.Group screenOptions={{ presentation: 'modal' }}>
  <Stack.Screen name="EditProfile" component={EditProfile} />
</Stack.Group>

// For a genuinely full-screen task (camera, video, markup):
<Stack.Group screenOptions={{ presentation: 'fullScreenModal' }}>
  <Stack.Screen name="Camera" component={Camera} />
</Stack.Group>
```

With `@react-navigation/native-stack` (which wraps `UIViewController` presentation) you get the platform-correct sheet animation, the drag-to-dismiss gesture, and the stacked-card appearance for free. `@react-navigation/stack` (the JS implementation) does not, and its modal transition is a recognizable approximation.

For detented bottom sheets, `react-native-bottom-sheet` or the native-stack `sheetAllowedDetents` option:

```jsx
<Stack.Screen
  name="Filters"
  component={Filters}
  options={{
    presentation: 'formSheet',
    sheetAllowedDetents: [0.4, 0.9],   // medium + large, like UISheetPresentationController
    sheetGrabberVisible: true,          // the drag indicator people look for
    sheetCornerRadius: 16,
  }}
/>
```

### Platform-correct dismissal placement

The HIG table above translates directly into where the button goes:

```jsx
function ModalHeader({ title, onDone, onCancel }) {
  // iOS/iPadOS: buttons in the top toolbar. macOS/tvOS: in the content view.
  if (Platform.OS === 'macos' || Platform.OS === 'tvos') return <Text>{title}</Text>;
  return (
    <View style={s.toolbar}>
      <Pressable onPress={onCancel} accessibilityRole="button"><Text>Cancel</Text></Pressable>
      <Text style={textStyles.headline}>{title}</Text>{/* names the task */}
      <Pressable onPress={onDone} accessibilityRole="button"><Text>Done</Text></Pressable>
    </View>
  );
}
```

Note the title is not decorative here — it's the "make it easy to identify the modal's task" rule.

### Guard the dismiss gesture when content would be lost

The swipe-down gesture bypasses your Cancel button, so unsaved-changes handling has to hook the gesture too:

```jsx
import { useNavigation } from '@react-navigation/native';

function EditProfile({ isDirty }) {
  const navigation = useNavigation();

  useEffect(() => {
    // Fires for the back button AND the sheet drag-dismiss.
    const unsub = navigation.addListener('beforeRemove', e => {
      if (!isDirty) return;
      e.preventDefault();
      // Action sheet with a save option, per the HIG guidance —
      // not a bare "Discard?" alert.
      showActionSheet({
        options: ['Save Changes', 'Discard Changes', 'Cancel'],
        destructiveButtonIndex: 1,
        cancelButtonIndex: 2,
        onSelect: i => {
          if (i === 0) save().then(() => navigation.dispatch(e.data.action));
          if (i === 1) navigation.dispatch(e.data.action);
        },
      });
    });
    return unsub;
  }, [navigation, isDirty]);
}
```

Also set `gestureEnabled: false` on native-stack modals where an accidental swipe would be costly — but only alongside a visible Cancel button, never as a substitute for one.

### Don't stack modals

```jsx
// Wrong: a second modal presented from inside the first.
// Right: dismiss, then present — or better, present the second from the parent
// after the first reports its result.
navigation.goBack();
requestAnimationFrame(() => navigation.navigate('SecondSheet'));
```

RN's `<Modal>` component makes stacking easy and it's worth resisting: on iOS, two `<Modal>`s produce the exact visual clutter the HIG warns about. Prefer navigator-managed presentation, which serializes it for you.

### Keep the modal shallow

```jsx
// Nested navigator inside a modal → "an app within your app".
// If you truly need steps, use a single linear flow with no branching,
// and make Back visually distinct from Cancel.
<Stack.Screen name="Onboarding" options={{ presentation: 'modal' }}>
  {() => (
    <WizardStack initialRouteName="Step1">{/* Step1 → Step2 → Step3, one path */}</WizardStack>
  )}
</Stack.Screen>
```

### App lifecycle: pause and resume

```jsx
import { AppState } from 'react-native';

useEffect(() => {
  const sub = AppState.addEventListener('change', next => {
    if (next === 'background' || next === 'inactive') {
      videoRef.current?.pauseAsync();     // don't let people miss anything
      persistDraft();                     // so they resume where they left off
    }
    if (next === 'active') restoreDraft();
  });
  return () => sub.remove();
}, []);
```

`'inactive'` fires on iOS for transient states — the app switcher, an incoming call, Control Center. Treat it as "pause" but not as "tear down", or you'll reset state when someone merely peeks at Control Center.

### Audio interruptions — the two-kind rule

```js
import { Audio } from 'expo-av';

await Audio.setAudioModeAsync({
  // Ducking = the short-interruption behavior: lower volume, then restore.
  interruptionModeIOS: Audio.INTERRUPTION_MODE_IOS_DUCK_OTHERS,
  playsInSilentModeIOS: true,
  staysActiveInBackground: true,
});

// Primary interruptions (another music app took over) must pause indefinitely —
// don't auto-resume. Track why you paused so resume is only for ducked cases.
const pauseReason = useRef(null);
```

`react-native-track-player` exposes `RemoteDuck` with a `permanent` flag, which maps onto the distinction directly: `permanent: true` → pause and stay paused; `permanent: false` → duck or pause briefly and restore.

### Background completion

```js
// iOS grants a short window on backgrounding — finish or checkpoint, don't assume
// unlimited time. For real background work use expo-task-manager /
// react-native-background-fetch, or hand the transfer to the OS:
import * as FileSystem from 'expo-file-system';
const task = FileSystem.createDownloadResumable(url, dest, {}, onProgress);
await task.downloadAsync();
// Persist task.savable() so the download survives suspension and resumes on relaunch.
```

### Full screen

```jsx
import * as NavigationBar from 'expo-navigation-bar';
import * as StatusBar from 'expo-status-bar';

// Hide chrome for content focus, restore on tap — keep the gesture familiar.
const [chromeVisible, setChromeVisible] = useState(true);
<Pressable onPress={() => setChromeVisible(v => !v)} style={StyleSheet.absoluteFill}>
  <Video ... />
  {chromeVisible && <PlaybackControls />}{/* essential controls stay reachable */}
</Pressable>
```

Keep the iOS home-indicator default behavior — don't reach for gesture deferral unless you measure real accidental exits. On tvOS and macOS, use the platform's own full-screen affordances rather than a custom toggle.

## Review checklist

- [ ] Each modal has one of the four justifications; nothing modal purely for layout convenience.
- [ ] Modal task is short and shallow; no nested navigator producing an app-within-an-app.
- [ ] If steps exist, there's exactly one path through them, and Back is visually distinct from Cancel.
- [ ] Modal has a title naming its task.
- [ ] Dismissal follows platform convention (toolbar button/swipe on iOS; content-view button on macOS/tvOS).
- [ ] Swipe-to-dismiss intercepted when unsaved content exists; action sheet offers a save option.
- [ ] No warning shown when data loss is the expected outcome.
- [ ] Only one modal presented at a time; never two simultaneous alerts.
- [ ] Native-stack presentation used so sheet animation and drag gesture are the real ones.
- [ ] Media and games pause on `background`/`inactive` and resume on `active`.
- [ ] Draft state persisted on backgrounding; `inactive` doesn't tear down state.
- [ ] Primary audio interruptions pause indefinitely; short interruptions duck and restore.
- [ ] User-initiated transfers continue or checkpoint in the background.
- [ ] Notifications sent only for important, time-sensitive completions.
- [ ] Full screen: layout adjusted not resized, essential controls reachable, exit under user control, chrome restorable by a familiar gesture.
- [ ] iOS: home-indicator default swipe behavior preserved.
- [ ] macOS: system full-screen support used; standard entry affordances; display mode unchanged in games.
- [ ] visionOS: window edge appearance untouched; video keeps playing when looked away from.
- [ ] watchOS: no indeterminate spinners; notification promised instead.
