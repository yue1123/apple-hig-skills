# Accessibility, VoiceOver & Inclusion

Source: HIG › Foundations › [Accessibility](https://developer.apple.com/design/human-interface-guidelines/accessibility), [Inclusion](https://developer.apple.com/design/human-interface-guidelines/inclusion), Technologies › [VoiceOver](https://developer.apple.com/design/human-interface-guidelines/voiceover)

Read this whenever you build an interactive component, and before shipping any screen. Most of it is mechanically checkable, which makes it the highest-leverage reference here.

## Contents

- [What accessible means](#what-accessible-means)
- [Control sizes and spacing](#control-sizes-and-spacing)
- [Contrast requirements](#contrast-requirements)
- [Vision](#vision)
- [Hearing](#hearing)
- [Mobility](#mobility)
- [Speech](#speech)
- [Cognitive](#cognitive)
- [VoiceOver](#voiceover)
- [Inclusion](#inclusion)
- [Platform differences](#platform-differences)
- [React Native mapping](#react-native-mapping)
- [Review checklist](#review-checklist)

## What accessible means

Three properties, and they're a useful diagnostic when something feels off:

- **Intuitive** — familiar, consistent interactions; tasks are straightforward.
- **Perceivable** — no single sense is required. Information is available through sight, hearing, *or* touch.
- **Adaptable** — the interface bends to how people want to use their device, honoring system settings and offering its own where useful.

Audit with **Accessibility Inspector**, which shows how your app represents itself to assistive technology. Apple also exposes **Accessibility Nutrition Labels** on the App Store, so supported features are now a public, declared property of your app rather than an internal quality bar.

## Control sizes and spacing

| Platform | Default control size | Absolute minimum |
|---|---|---|
| iOS, iPadOS | **44 × 44 pt** | 28 × 28 pt |
| macOS | 28 × 28 pt | 20 × 20 pt |
| tvOS | 66 × 66 pt | 56 × 56 pt |
| visionOS | **60 × 60 pt** | 28 × 28 pt |
| watchOS | 44 × 44 pt | 28 × 28 pt |

**Spacing matters as much as size.** Concrete numbers Apple gives:

- **~12 pt of padding** around elements that have a bezel.
- **~24 pt of padding** around the visible edges of elements without a bezel.

The asymmetry has a reason: a bezel visually delimits the target, so people aim more accurately. A borderless icon button gives no such cue, so it needs more slack. Treat the 44 pt figure as the *hit region*, not the glyph size — a 24 pt icon with a 44 pt touch target is correct and common.

## Contrast requirements

Accessibility Inspector checks against WCAG AA:

| Text size | Weight | Minimum ratio |
|---|---|---|
| Up to 17 pt | Any | **4.5:1** |
| 18 pt and above | Any | 3:1 |
| Any | Bold | 3:1 |

If your default palette can't hit these, it must at least supply a higher-contrast scheme when **Increase Contrast** is on. Check both appearances if you support Dark Mode — passing in light and failing in dark is the usual outcome of picking colors in one mode only.

## Vision

**Support at least 200% text enlargement** (140% on watchOS), via Dynamic Type or your own control. See [typography.md](typography.md) for the mechanics.

**Prefer system colors** — they carry accessible variants that adapt to Increase Contrast and appearance changes automatically.

**Never rely on color alone.** Red-green and blue-orange are the classic problem pairs. Add distinct shapes or icons alongside color. Where color *is* the data — chart series, game characters — let people customize the scheme.

## Hearing

Dialogue and crucial information must not be audio-only. Four distinct text affordances, each with a different job:

- **Captions** — text equivalent of audible information, synchronized live. For cutscenes and video clips.
- **Subtitles** — onscreen dialogue in the viewer's preferred language. For TV and film.
- **Audio descriptions** — spoken narration of visual-only information, placed in natural pauses.
- **Transcripts** — complete textual description of both audio and visual content. For long-form media like podcasts and audiobooks, where people want to review or follow along.

Let people customize how that text is presented, not just whether it appears.

**Pair audio cues with haptics.** A success chime, an error sound, game feedback — anyone with the volume off or hearing loss gets nothing from sound alone.

**Pair audio cues with visual ones**, especially in games and spatial apps where the thing being signaled may be off-screen. If sound is directing attention somewhere, something visible must point there too.

## Mobility

**Prefer the simplest gesture that works, for anything done frequently.** Avoid custom multi-finger and multi-hand gestures. Complex gestures are hard for many people, disability or not, and they compound with repetition.

**Always provide a non-gesture alternative to core functionality.** If a swipe dismisses a view, a button must also dismiss it. This is the single most commonly skipped rule, and it's what locks out switch and assistive-device users entirely.

**Label elements properly so Voice Control works.** Voice Control lets people operate the device entirely by speaking; it addresses controls by their labels, so unlabeled controls are unreachable.

**Support the mobility assistive technologies**: VoiceOver, AssistiveTouch, Full Keyboard Access, Pointer Control, Switch Control. Test with them on — reading the API docs isn't the same as navigating your own app by switch.

## Speech

**Make the app fully keyboard-navigable.** Full Keyboard Access lets people drive apps from a physical keyboard. Don't override system-defined keyboard shortcuts.

**Support Switch Control** — hardware switches, game controllers, or sounds like a click or pop — for selecting, tapping, typing, and drawing.

## Cognitive

**Keep actions simple and consistent.** Prefer system gestures people already know over custom ones they must learn and retain.

**Minimize time-boxed UI.** Anything that auto-dismisses on a timer penalizes people who need longer to read, and people whose assistive technology takes longer to traverse the screen. Prefer explicit dismissal.

**Never autoplay audio or video without controls** to start and stop it. Make those controls discoverable and easy to hit, and consider a global opt-out.

**Respond to Dim Flashing Lights** if you play video. The system can detect, mitigate, and warn about flashing sequences.

**Respond to Reduce Motion.** Excess fast or blinking animation is distracting at best and can trigger dizziness or seizures. When Reduce Motion is on, Apple's specific prescriptions:

- Tighten animation springs to remove bounce.
- Track animations directly to the person's gesture instead of playing them autonomously.
- Don't animate depth changes on the z-axis.
- Replace x/y/z translations with **fades**.
- Don't animate into and out of blurs.

Note that "reduce" is not "remove" — a cross-fade still communicates the transition. Cutting all animation to zero often makes state changes harder to follow, not easier.

**Optimize for Assistive Access** (iOS/iPadOS), a streamlined mode for people with cognitive disabilities. When it's on:

- Strip to core functionality; drop noncritical workflows and UI.
- Break multistep flows into one interaction per screen.
- **Confirm twice** for anything hard to recover from, like deleting a file.

**Offer difficulty accommodations in games** — relaxed success criteria, longer reaction windows, control assistance.

## VoiceOver

### Descriptions

**Label all key interface elements.** System controls have generic default labels; replace them with ones that convey your app's actual functionality. Custom elements have none at all. Labels must stay in sync as the interface changes — stale labels are worse than missing ones because they actively mislead.

**Describe meaningful images**, but describe *only what the image itself conveys*. VoiceOver already reads surrounding context like captions, so repeating the caption in the image label wastes the listener's time.

**Make charts and infographics fully accessible.** Give each a concise description of what it conveys. If sighted users can interact with it for more detail, VoiceOver users must be able to as well.

**Hide purely decorative images.** Excluding them respects people's time and lowers cognitive load. A screen where VoiceOver announces twelve decorative separators is unusable even though every element is "labeled".

### Navigation

**Titles and headings carry the hierarchy.** The title is the *first* thing announced on arriving at a screen — make it unique and descriptive. Use accurate section headings so people can build a mental model of the page.

**Declare grouping, order, and linkage.** Sighted people infer relationships from proximity and alignment; VoiceOver can't. Look for places where a relationship is *visual only* and encode it.

The canonical example: a grid of images with captions. Ungrouped, VoiceOver reads every image and then every caption — the pairing is lost. Grouped, it reads each image with its own caption. Same pixels, completely different experience.

VoiceOver reads in the reading order of the active language and locale, so grouping also determines what "next" means.

**Notify VoiceOver when content or layout changes.** An unannounced change invalidates the user's mental map without telling them.

**Support the rotor.** People navigate by headings, links, and content types via the VoiceOver rotor. Identifying those elements makes long content navigable instead of linear.

## Inclusion

Inclusion is broader than accessibility and mostly about assumptions. Note that an *inoffensive* app isn't automatically an inclusive one — screening for offense is a lower bar than designing for welcome.

**Language:**

- Address people as **you/your**. "The user" or "the player" reads distant. Reserve **we/our** for your company or software — otherwise it implies a personal relationship that can land as condescending.
- Don't use specialized or technical terms without defining them, and prefer plain language even when the audience would understand the jargon — plain sentences are easier to read *and* to translate.
- Replace colloquial expressions. They're culture-bound, hard to translate, and some carry exclusionary histories (Apple names "peanut gallery" and "grandfathered in").
- Be cautious with humor — subjective, poorly translatable, and grating on repeat encounters.
- Watch tone. An academic register can signal that only the highly educated are welcome.

**Gender:** avoid unnecessary gender references. Rewriting "a subscriber can post his or her recipes" as "subscribers can post recipes" removes the gendered pronouns *and* survives localization into languages with gendered grammar. Prefer non-gendered avatars, emoji, glyphs, and characters, or let people customize them. Use non-gendered human figures for generic people (SF Symbols has `figure` and `person` variants). If you genuinely need gender for health or legal reasons, offer *nonbinary*, *self-identify*, and *decline to state* — and consider letting people state their pronouns.

**Representation:** portray a range of racial backgrounds, body types, ages, and physical capabilities. Avoid occupational stereotypes (only male doctors, only female nurses). Review the settings and objects you depict too — high affluence reads as out of touch in many contexts.

**Assumptions:** the security-question example is the sharpest illustration. "What was your favorite subject in college?", "What was the make of your first car?", "How did you feel when you first saw a rainbow?" all presuppose experiences not everyone has — and a question you can't answer isn't a security question, it's a lockout. Universal alternatives: "What's your favorite activity?", "What was the name of your first friend?", "What quality describes you best?"

**Disability framing:** each disability is a spectrum (visual ranges from color blindness through low vision to blindness), and everyone experiences **temporary** disabilities (an ear infection) and **situational** ones (a noisy train). Include people with disabilities in your representation of people; don't use disability as a metaphor for a negative quality; take a people-first approach in writing.

**Localization:** plain language, no unnecessary gender references, and SF Symbols all reduce localization cost later. Be careful with color — white means purity in some cultures and death or grief in others. If color communicates, verify it communicates the same thing in every locale you ship.

## Platform differences

Only visionOS adds material considerations, and they're about **physical comfort** rather than perception:

- **Keep elements within the field of view.** Prefer horizontal layouts over vertical ones (neck strain) and don't demand attention in rapidly alternating locations.
- **Reduce the speed and intensity of animated objects**, especially in peripheral vision.
- **Be gentle with camera and video motion.** Avoid anything that feels like the world is moving without the person's consent.
- **Don't anchor content to the wearer's head.** It feels confining and it blocks assistive technologies like Pointer Control.
- **Minimize large, repetitive gestures** — tiring, and sometimes impossible depending on physical surroundings.

visionOS also offers head and hand **Pointer Control** and a **Zoom** feature. And a VoiceOver interaction worth knowing: **when VoiceOver is on in visionOS, apps with custom gestures don't receive hand input by default**, so people can explore by voice without the app reacting simultaneously. People can opt into Direct Gesture mode, which disables standard VoiceOver gestures. If your app leans on custom hand gestures, it is effectively inert under default VoiceOver — which is the strongest possible argument for the "always offer a non-gesture alternative" rule.

## React Native mapping

### The core props

```jsx
<Pressable
  // Role — maps to UIAccessibilityTraits / Android class name.
  accessibilityRole="button"
  // Label — what VoiceOver announces. Replaces the visible text, so keep it
  // meaningful and don't include the word "button" (the role says that already).
  accessibilityLabel="Add to favorites"
  // Hint — what happens on activation. Optional; VoiceOver reads it after a pause.
  // Don't restate the label here.
  accessibilityHint="Saves this recipe to your favorites list"
  // State — selected/disabled/checked/busy/expanded.
  accessibilityState={{ selected: isFavorite, disabled: isSaving }}
  // Value — for sliders, progress, and anything with a magnitude.
  accessibilityValue={{ min: 0, max: 100, now: progress, text: `${progress} percent` }}
  // Hit region ≥ 44×44 even when the glyph is smaller.
  hitSlop={10}
  style={{ width: 24, height: 24 }}
/>
```

Useful roles: `button`, `link`, `header`, `image`, `imagebutton`, `search`, `switch`, `checkbox`, `radio`, `tab`, `tablist`, `menuitem`, `progressbar`, `slider`, `summary`, `alert`, `text`, `adjustable`.

`accessibilityRole="header"` is how you satisfy the "use headings to convey hierarchy" rule and how the VoiceOver rotor finds section headings.

### Grouping — the image/caption problem

This is the RN version of the grid example above:

```jsx
// Wrong: VoiceOver reads every image, then every caption.
<View>
  <Image source={photo} accessibilityLabel={caption} />
  <Text>{caption}</Text>
</View>

// Right: one focusable element per logical item; label composed once.
<View accessible accessibilityRole="image" accessibilityLabel={`${caption}. ${date}`}>
  <Image source={photo} />
  <Text>{caption}</Text>
  <Text>{date}</Text>
</View>
```

`accessible={true}` collapses a subtree into a single focus stop. Use it for list rows, cards, and anything read as one unit; avoid it on containers holding several independently interactive controls, since it makes the inner controls unreachable.

### Hiding decoration

```jsx
// Decorative — excluded from the accessibility tree.
<Image source={divider} accessibilityElementsHidden importantForAccessibility="no-hide-descendants" />
```

`accessibilityElementsHidden` is iOS, `importantForAccessibility="no-hide-descendants"` is Android — set both. An `<Image>` with no `accessibilityLabel` is *not* automatically hidden on all platforms, so be explicit.

### Reacting to system settings

```js
import { AccessibilityInfo } from 'react-native';

function useA11ySettings() {
  const [s, setS] = useState({
    reduceMotion: false, screenReader: false, boldText: false,
    reduceTransparency: false, invertColors: false, grayscale: false,
  });

  useEffect(() => {
    const read = () => Promise.all([
      AccessibilityInfo.isReduceMotionEnabled(),
      AccessibilityInfo.isScreenReaderEnabled(),
      AccessibilityInfo.isBoldTextEnabled(),
      AccessibilityInfo.isReduceTransparencyEnabled(),
      AccessibilityInfo.isInvertColorsEnabled(),
      AccessibilityInfo.isGrayscaleEnabled(),
    ]).then(([reduceMotion, screenReader, boldText, reduceTransparency, invertColors, grayscale]) =>
      setS({ reduceMotion, screenReader, boldText, reduceTransparency, invertColors, grayscale }));

    read();
    const subs = [
      ['reduceMotionChanged', v => setS(p => ({ ...p, reduceMotion: v }))],
      ['screenReaderChanged', v => setS(p => ({ ...p, screenReader: v }))],
      ['boldTextChanged', v => setS(p => ({ ...p, boldText: v }))],
      ['reduceTransparencyChanged', v => setS(p => ({ ...p, reduceTransparency: v }))],
      ['invertColorsChanged', v => setS(p => ({ ...p, invertColors: v }))],
      ['grayscaleChanged', v => setS(p => ({ ...p, grayscale: v }))],
    ].map(([e, cb]) => AccessibilityInfo.addEventListener(e, cb));
    return () => subs.forEach(sub => sub.remove());
  }, []);

  return s;
}
```

These are live settings, not launch-time constants — people toggle Reduce Motion mid-session precisely because something made them uncomfortable.

### Announcing changes

```js
// Report content/layout changes so VoiceOver users can update their mental map.
AccessibilityInfo.announceForAccessibility(`${results.length} results found`);

// Move focus deliberately — e.g. to a newly presented sheet's title.
import { findNodeHandle, AccessibilityInfo } from 'react-native';
const tag = findNodeHandle(titleRef.current);
if (tag) AccessibilityInfo.setAccessibilityFocus(tag);
```

For modals, mark them so VoiceOver can't wander into the content behind:

```jsx
<View accessibilityViewIsModal style={StyleSheet.absoluteFill}>{/* sheet content */}</View>
```

### Reduce Motion, implemented per Apple's prescriptions

```jsx
const { reduceMotion } = useA11ySettings();

// Translation → fade. Not "no animation at all".
const style = useAnimatedStyle(() => reduceMotion
  ? { opacity: progress.value }
  : { opacity: progress.value, transform: [{ translateY: (1 - progress.value) * 24 }] });

// Tighten springs rather than removing them.
const spring = reduceMotion
  ? { damping: 100, stiffness: 400 }   // effectively no bounce
  : { damping: 15,  stiffness: 180 };

// LayoutAnimation on Android / legacy paths
LayoutAnimation.configureNext({
  duration: reduceMotion ? 0 : 250,
  update: { type: reduceMotion ? 'linear' : 'spring', springDamping: 0.7 },
});
```

Also disable autoplaying carousels, looping video, and parallax when this is on.

### Always give the gesture an alternative

```jsx
// A swipe-to-dismiss sheet must also have a button.
<View>
  <PanGestureHandler onGestureEvent={onSwipeDismiss}>{/* … */}</PanGestureHandler>
  <Pressable onPress={onDismiss} accessibilityRole="button" accessibilityLabel="Close">
    <Icon name="xmark" />
  </Pressable>
</View>
```

Same for swipe-to-delete rows (offer a long-press menu or an edit mode), pinch-to-zoom (offer +/− buttons), and drag-to-reorder (offer "Move up"/"Move down" as `accessibilityActions`):

```jsx
<View
  accessibilityActions={[{ name: 'moveUp', label: 'Move up' }, { name: 'moveDown', label: 'Move down' }]}
  onAccessibilityAction={({ nativeEvent }) => {
    if (nativeEvent.actionName === 'moveUp') moveUp(index);
    if (nativeEvent.actionName === 'moveDown') moveDown(index);
  }}
/>
```

### Minimum sizes as a reusable constant

```js
const MIN_TARGET = Platform.select({ ios: 44, android: 48, macos: 28, tvos: 66, visionos: 60, default: 44 });

// Prefer hitSlop over inflating visual size — keeps the design tight and the target large.
const iconButton = { width: 24, height: 24 };
const iconButtonHitSlop = (MIN_TARGET - 24) / 2; // 10 on iOS
```

Android's 48 dp comes from Material rather than HIG, and it's the stricter number — using it as the shared floor satisfies both.

### What to test

Turn these on and actually use the app: VoiceOver (iOS) / TalkBack (Android), Reduce Motion, Bold Text, largest Larger Accessibility Text, Increase Contrast, Reduce Transparency, and Full Keyboard Access on iPad/macOS. `eslint-plugin-react-native-a11y` catches missing labels and roles statically; it can't catch a wrong label or a broken focus order, which is where most real failures live.

## Review checklist

- [ ] Every interactive element has an `accessibilityRole` and a meaningful `accessibilityLabel`.
- [ ] Labels don't contain the role ("Add" not "Add button") and hints don't restate labels.
- [ ] `accessibilityState` reflects selected/disabled/checked; `accessibilityValue` set on sliders and progress.
- [ ] Hit regions ≥ 44 × 44 pt (iOS) via `hitSlop` where the glyph is smaller.
- [ ] ~12 pt padding around bezeled controls, ~24 pt around borderless ones.
- [ ] Related content grouped into single focus stops; decorative images hidden on both platforms.
- [ ] Screen has a unique descriptive title; sections use `accessibilityRole="header"`.
- [ ] Content and layout changes announced; focus moved deliberately on modal presentation.
- [ ] Modals marked `accessibilityViewIsModal`.
- [ ] Every gesture-only interaction has a button or `accessibilityActions` equivalent.
- [ ] No custom multi-finger gestures for frequent actions.
- [ ] Reduce Motion: translations become fades, springs tightened, no z-axis or blur animation, autoplay stopped.
- [ ] Bold Text, Reduce Transparency, Invert Colors, Grayscale handled where relevant.
- [ ] Text contrast ≥ 4.5:1 (≥ 3:1 at 18 pt+ or bold), verified in light **and** dark.
- [ ] No information carried by color alone.
- [ ] Text scales to ≥ 200% without loss of function.
- [ ] Audio cues paired with haptics and visual cues; media has captions/subtitles/transcripts as appropriate.
- [ ] No autoplaying media without controls; no timer-only dismissal of important views.
- [ ] Destructive actions confirmed (twice under Assistive Access).
- [ ] Copy uses "you", avoids jargon, colloquialisms, unnecessary gender, and untranslatable humor.
- [ ] Any security questions or personal prompts assume no specific cultural experience.
- [ ] Tested by actually navigating with VoiceOver/TalkBack, not just by linting.
- [ ] visionOS: no head-anchored content, no large repetitive gestures, horizontal layouts preferred, non-gesture paths exist for all custom hand gestures.
