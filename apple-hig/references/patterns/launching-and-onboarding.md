# Launching, Onboarding & Offering Help

Source: HIG › Patterns › [Launching](https://developer.apple.com/design/human-interface-guidelines/launching), [Onboarding](https://developer.apple.com/design/human-interface-guidelines/onboarding), [Offering help](https://developer.apple.com/design/human-interface-guidelines/offering-help)

Read this when building a splash/launch screen, a first-run flow, in-app tips, or state restoration.

## Contents

- [Launching](#launching)
- [Launch screens](#launch-screens)
- [Onboarding](#onboarding)
- [Offering help and tips](#offering-help-and-tips)
- [Platform differences](#platform-differences)
- [React Native mapping](#react-native-mapping)
- [Review checklist](#review-checklist)

## Launching

**Launch instantly.** People don't want to wait more than a couple of seconds before they can interact.

**Restore the previous state on relaunch.** Don't make people retrace steps to get back where they were. Restore granular detail: scroll position, window state and location, in-progress input. This is the highest-value launch behavior and the one most often skipped.

**Provide a launch screen where the platform requires one** — iOS, iPadOS, tvOS. macOS, visionOS, and watchOS don't need one.

**A splash screen, if you need one, belongs at the start of onboarding** — not as the launch screen. If you have no onboarding flow, show it once launching completes.

## Launch screens

The launch screen's only job is to make the app *feel* immediately ready. It is not onboarding, not a splash screen, and not a place for artistic expression.

**Make it nearly identical to your first screen.** Any element that differs produces a visible flash on transition. If your app shows a solid color before its first screen, make the launch screen that solid color and nothing else. Match the current orientation and appearance mode.

**No text.** The launch screen's content is static, so text on it will never be localized.

**No branding.** No logos, no "About"-window look, unless the element is a permanent fixture of your actual first screen.

The rule underneath all three: a launch screen that draws attention to itself defeats its purpose, because people notice it *as a wait*.

## Onboarding

**Teach through interactivity.** People retain what they do far better than what they read. Let them safely try an action, discover a feature, or test a game mechanic.

**Prefer context-specific tips over one upfront flow.** Tips delivered where the relevant task happens let people learn while making progress, and concentrate on one action at a time instead of memorizing a tour. Put instructions near the UI they describe.

**If you must have a prerequisite flow, make it brief and enjoyable.** People finish short, entertaining onboarding; they abandon or forget long, dense onboarding. Trying to teach too much means teaching nothing.

**Make separate tutorials optional and skippable — and don't re-offer them.** If someone skips at first launch, don't present it again on later launches, but keep it findable in help, account, or settings.

**Teach your app, not the device.** People don't need to learn how iOS works.

**Postpone nonessential setup and customization.** Ship reasonable defaults so most people can start immediately with zero configuration.

**Integrate a permission request into onboarding only if the app genuinely can't function without it** — and use the opportunity to show why and what the benefit is. Otherwise ask at the point of use. See [privacy.md](../foundations/privacy.md).

**Don't ask for ratings or purchases during onboarding.** Let people become engaged first; responses are more positive and the request doesn't read as presumptuous.

**Don't let large downloads block onboarding.** Bundle enough content that people can start interacting immediately, whether they take the tour or skip it.

**Keep licensing details out of onboarding.** Let the App Store show agreements and disclaimers pre-download. If you must include them, integrate them without derailing the experience.

## Offering help and tips

**Match the help to the task's complexity.** A one- or two-step task warrants an inline view with a succinct description. A complex multistep task may warrant a tutorial. Tie help to what the person is doing *right now*, and make it easy to dismiss or avoid.

**Don't document standard components.** Explain what a standard element does *in your app*, not how buttons work. Exception: if you introduce a genuinely unique control, or expect a nonstandard use of an input device (holding the Siri Remote rotated 90°), orient people quickly — and prefer animation or graphics over prose.

**Keep language and imagery contextually correct.** Don't show a game controller to someone using a Siri Remote. Don't say "click" on iPhone or "tap" on Mac.

### Tip design

**Pick the right tip type:**

- **Popover tip** — when you want to preserve content flow.
- **Inline tip** — when surrounding information must stay visible.
- **Annotation-style inline** — pointing at a specific UI element.
- **Hint-style** — not tied to a specific element.

**Tips are for simple features.** If a feature needs more than **three actions**, it's too complex for a tip.

**Short, actionable, engaging.** One or two sentences. Action-oriented language: what it does and how to use it. **No promotional content** — nothing that advertises or sells, and nothing about a different feature or flow.

**Define eligibility rules so tips reach only people who benefit.** Someone who already uses a feature shouldn't be told about it. Use parameter- or event-based rules, and set a **display frequency** — e.g. at most one tip per 24 hours — when you have several.

**Include the feature's associated symbol, preferring the filled variant** — a star on a favorites tip does real work.

**Use buttons to route people onward** — to the relevant settings, or to a setup flow or more detail.

### Tooltips (macOS, visionOS)

- **Describe only the control the person indicated interest in** — not neighbors, not the larger task.
- **Start with a verb**: "Restore default settings", "Add or remove a language from the list".
- **Don't repeat the control's name** — it wastes the space and adds nothing.
- **60–75 characters maximum.** Sentence fragments are fine; drop articles. If a control needs more text than that, the interface design is the problem.
- **Sentence case**, no ending punctuation unless your style requires it.
- **Consider context-sensitive tooltips** — different text per control state.

## Platform differences

### iOS / iPadOS

**Launch in the device's current orientation** if you support both. If you support only one, launch in it and let people rotate. A landscape-only interface must work rotated **either** direction.

### tvOS

The launch screen is **static** — unlike the layered, parallax images used elsewhere in a tvOS app.

**In a live-viewing app, consider auto-starting playback** after a few seconds of inactivity — people opened a TV app to watch TV.

### visionOS

**Consider launching into the Shared Space even if your app is fully immersive.** A window in the Shared Space gives context while the app loads and gives you somewhere to put the control that opens the immersive experience. People want to choose when to enter a Full Space, particularly when other apps are running.

## React Native mapping

### Launch screen configuration

Native config, not RN code:

```js
// Expo — app.json. Match your first screen, not your brand.
{
  "expo": {
    "splash": {
      "image": "./assets/splash-minimal.png",   // no text, no logo unless it's on screen 1
      "resizeMode": "contain",
      "backgroundColor": "#FFFFFF",
      "dark": { "backgroundColor": "#000000" }  // match appearance mode
    }
  }
}
```

If your first screen is a plain background, the correct splash is *that background color with no image at all* — which is what eliminates the flash. Bare RN: `LaunchScreen.storyboard` in Xcode.

Hold the splash only until your first frame is genuinely ready, then dismiss it explicitly so you don't add perceived launch time:

```jsx
import * as SplashScreen from 'expo-splash-screen';

SplashScreen.preventAutoHideAsync();   // at module scope

function App() {
  const ready = useAppBootstrap();     // fonts, cached session, restored nav state
  useEffect(() => { if (ready) SplashScreen.hideAsync(); }, [ready]);
  if (!ready) return null;             // splash still covering — nothing to render
  return <Root />;
}
```

Careful: it's easy to turn `preventAutoHideAsync` into an *artificially long* launch by awaiting network calls here. Only block on what the first frame truly needs — fonts and cached state, not fresh data.

### State restoration

This is the launch rule RN apps most commonly miss, and react-navigation supports it directly:

```jsx
import { NavigationContainer } from '@react-navigation/native';
import AsyncStorage from '@react-native-async-storage/async-storage';

const NAV_STATE_KEY = 'nav-state-v1';

function Root() {
  const [initialState, setInitialState] = useState();
  const [restoring, setRestoring] = useState(true);

  useEffect(() => {
    AsyncStorage.getItem(NAV_STATE_KEY)
      .then(saved => { if (saved) setInitialState(JSON.parse(saved)); })
      .finally(() => setRestoring(false));
  }, []);

  if (restoring) return null;

  return (
    <NavigationContainer
      initialState={initialState}
      onStateChange={state => AsyncStorage.setItem(NAV_STATE_KEY, JSON.stringify(state))}
    >
      <Stack />
    </NavigationContainer>
  );
}
```

Version the key (`-v1`) so a navigator restructure doesn't restore into a route that no longer exists.

Restore the finer detail too — that's the "granular details" part of the rule:

```jsx
// Scroll position
const [offset, setOffset] = useState(0);
<FlatList
  ref={listRef}
  onScroll={e => persist('feed.offset', e.nativeEvent.contentOffset.y)}
  onLayout={() => listRef.current?.scrollToOffset({ offset, animated: false })}
/>

// In-progress text — restore drafts rather than discarding them.
<TextInput value={draft} onChangeText={t => { setDraft(t); persistDraft(t); }} />
```

### Onboarding: skippable, once

```jsx
const [seen, setSeen] = useState(null);
useEffect(() => { AsyncStorage.getItem('onboarded').then(v => setSeen(v === '1')); }, []);

// Skipping counts as done — don't re-present on later launches.
function finishOnboarding() { AsyncStorage.setItem('onboarded', '1'); }

// But keep it reachable afterwards.
<SettingsRow title="View Tutorial" onPress={() => navigation.navigate('Onboarding', { manual: true })} />
```

Prefer interactive steps over carousel slides. A step where the person performs the real action on real (or sandboxed) data satisfies "teach through interactivity"; four swipeable illustrations do not.

### Contextual tips instead of a tour

There's no RN binding for TipKit, so implement the eligibility and frequency rules yourself — they're what separate a tip system from nagging:

```js
// Minimal tip engine mirroring TipKit's rules.
async function shouldShowTip(id, { requires = {}, minGapHours = 24 }) {
  const [dismissed, lastAnyTip, usage] = await Promise.all([
    AsyncStorage.getItem(`tip.${id}.dismissed`),
    AsyncStorage.getItem('tip.lastShownAt'),
    getFeatureUsage(requires.feature),
  ]);
  if (dismissed) return false;
  if (usage > 0) return false;                                    // already uses it — no tip
  if (lastAnyTip && Date.now() - +lastAnyTip < minGapHours * 3.6e6) return false;  // cadence
  return true;
}
```

Render it near the feature, with the filled symbol and a routing button:

```jsx
{showTip && (
  <View style={s.tip}>
    <Icon name="star.fill" />{/* filled variant, per the guidance */}
    <Text style={textStyles.subhead}>Tap the star to save a recipe for later.</Text>
    <Pressable onPress={openFavorites}><Text>Show Favorites</Text></Pressable>
    <Pressable onPress={dismissTip} accessibilityLabel="Dismiss tip"><Icon name="xmark" /></Pressable>
  </View>
)}
```

### Tooltips on macOS and visionOS

```jsx
// react-native-macos exposes tooltip; on visionOS hover is a system behavior.
<Pressable tooltip="Restore default settings">{/* verb-first, ≤75 chars, no control name */}
  <Icon name="arrow.counterclockwise" />
</Pressable>
```

On touch platforms there is no hover, so a tooltip is not an option — the label must be visible, or the control must be a recognizable standard symbol.

### Defaults over setup

```js
// Ship working defaults so first launch needs no configuration.
const DEFAULT_PREFS = { units: 'metric', notifications: true, theme: 'system' };
// Then let people change them in Settings — don't ask up front.
```

Note `theme: 'system'` rather than a light/dark choice — the app should follow the OS, per [color-and-materials.md](../foundations/color-and-materials.md).

## Review checklist

- [ ] Launch screen matches the first screen; no text, no logo, no branding.
- [ ] Splash background has light and dark variants and matches the appearance mode.
- [ ] Splash dismissed as soon as the first frame is ready; nothing network-bound blocks it.
- [ ] Navigation state persisted and restored on relaunch, with a versioned storage key.
- [ ] Scroll positions and in-progress text drafts restored, not just the route.
- [ ] Onboarding is skippable, recorded as complete when skipped, and never re-presented.
- [ ] Tutorial remains reachable from settings/help afterwards.
- [ ] Onboarding teaches the app, not the platform; steps are interactive where possible.
- [ ] No rating or purchase prompt during onboarding.
- [ ] No large download required before first interaction.
- [ ] Permission requests deferred to point of use unless the app can't function without them.
- [ ] Nonessential setup postponed; sensible defaults shipped (theme follows the system).
- [ ] Tips are one or two sentences, action-oriented, non-promotional, and cover features of ≤ 3 actions.
- [ ] Tips have eligibility rules (not shown to existing users of the feature) and a frequency cap.
- [ ] Tips appear near the UI they describe and include the feature's filled symbol.
- [ ] Help copy uses platform-correct verbs and depicts the actual input device.
- [ ] Tooltips (macOS/visionOS) start with a verb, omit the control name, stay under ~75 characters, use sentence case.
- [ ] No help content explaining how standard components work.
- [ ] iOS: launches in the current orientation; landscape-only works in both rotations.
- [ ] visionOS: launches into the Shared Space with an explicit control to enter a Full Space.
