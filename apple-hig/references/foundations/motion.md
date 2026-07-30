# Motion & Animation

Source: HIG › Foundations › [Motion](https://developer.apple.com/design/human-interface-guidelines/motion)

Read this before adding any custom animation, transition, or gesture-driven interaction.

## Contents

- [Rules that matter most](#rules-that-matter-most)
- [Feedback motion](#feedback-motion)
- [Animated symbols](#animated-symbols)
- [Frame rate and power](#frame-rate-and-power)
- [Platform differences](#platform-differences)
- [React Native mapping](#react-native-mapping)
- [Review checklist](#review-checklist)

## Rules that matter most

**System components already animate correctly** — and they adapt. Liquid Glass, for instance, responds with *greater* emphasis to direct touch (reinforcing tactility) and a *more subdued* effect for trackpad input. You get that adaptation for free by using standard components, and you forfeit it the moment you hand-roll a replacement. That tradeoff is worth making consciously.

**Add motion purposefully.** Gratuitous animation distracts, and can make people feel disconnected or physically uncomfortable. "It looks nice in the demo" is not a purpose.

**Motion must be optional.** Never make animation the only carrier of important information. Supplement with haptics and audio so the message survives Reduce Motion, a glance away, or a person who simply can't perceive it.

**Don't animate frequent interactions.** The system already provides subtle animation for standard elements. For a custom element used dozens of times per session, extra motion means extra time spent watching instead of doing — every single time.

**Let people cancel or skip motion.** Never make someone wait for an animation to finish before they can act — especially an animation they'll see repeatedly. An unskippable 400 ms transition is a 400 ms tax charged on every navigation.

## Feedback motion

**Match motion to the gesture that caused it.** If a view is revealed by sliding down from the top, it should dismiss upward — not sideways. Directional mismatch is disorienting in a way people feel before they can name it.

**Be brief and precise.** Short, precisely-timed feedback feels lightweight and often communicates *better* than prominent animation. Apple's examples: a succinct animation tied exactly to a successful game action delivers the message without pulling the player out of gameplay; tapping a panorama in visionOS Photos expands it quickly and smoothly, so people can track the transition without waiting to enjoy the content.

The pattern in both: the animation's job is to make a change **trackable**, not to be noticed itself. If people are watching your animation rather than the result, it's too long or too elaborate.

## Animated symbols

SF Symbols 5 and later support animation on both system and custom symbols — bounce, pulse, variable color, replace, scale, appear/disappear. Preferring an animated symbol over a hand-built animation gets you correct timing, correct weight matching with adjacent text, and automatic Reduce Motion behavior. See [icons-and-images.md](icons-and-images.md).

## Frame rate and power

**30–60 fps** is the range Apple cites for a smooth experience in games. Use each device's graphics capability to set good defaults so people don't have to configure anything before they can enjoy the app.

**Let people trade visual fidelity for performance or battery.** For example, switch power modes when an external power source is detected.

## Platform differences

No additional considerations for iOS, iPadOS, macOS, or tvOS.

### visionOS

Motion is a large part of a spatial experience *and* the main source of physical discomfort, so the guidance here is about the body, not aesthetics.

- **Avoid motion at the edges of the field of view.** People are highly sensitive to peripheral motion — it's distracting, and it can make them feel that they or their surroundings are moving. If something must move in the periphery, keep its brightness similar to the rest of the visible content.
- **Large moving objects need mitigation.** An object big enough to fill much of the field of view and occlude passthrough gets perceived as *part of the surroundings* — so when it moves, people feel like *they* moved. Increase its translucency or lower its contrast so the motion registers as the object's, not the world's. This applies even when the person is the one moving it (a window, say) — also consider keeping windows fairly small.
- **Use fades to relocate objects.** People instinctively track movement. If the movement itself communicates nothing, fade out, move, fade in.
- **Don't let people rotate a virtual world.** World rotation upsets the sense of stability even when it's user-controlled and subtle. Use instantaneous directional changes behind a quick fade-out instead.
- **Give a stationary frame of reference.** Movement contained inside a non-moving area is far more tolerable than movement of the entire surround — which is why automatic movement-through-space in games makes people unwell.
- **Avoid sustained oscillation, especially near 0.2 Hz** — people are unusually sensitive to that frequency. If you must oscillate, keep amplitude low and consider making the content translucent.

### watchOS

All layout- and appearance-based animations include built-in easing at the start and end. **You can't turn it off or customize it**, so don't design timing that depends on a linear curve.

## React Native mapping

### Reduce Motion is the first branch, not a polish pass

Every animation below should read the setting. See [accessibility.md](accessibility.md) for the `useA11ySettings` hook; the prescriptions Apple gives translate directly:

| Apple's guidance | Reanimated equivalent |
|---|---|
| Tighten springs to reduce bounce | raise `damping`, raise `stiffness` |
| Track animations to gestures | `useAnimatedGestureHandler` / shared value driven by the gesture, not `withTiming` |
| Don't animate z-axis depth | drop `scale`/perspective transitions |
| Replace x/y/z motion with fades | animate `opacity` only |
| Don't animate into/out of blurs | swap `BlurView` intensity changes for a cross-fade |

```js
import { useReducedMotion, withSpring, withTiming, Easing } from 'react-native-reanimated';

// Reanimated ships this hook — it already tracks the OS setting.
const reduced = useReducedMotion();

const SPRING = reduced
  ? { damping: 100, stiffness: 500, mass: 1 }   // critically damped: settles, no overshoot
  : { damping: 18,  stiffness: 220, mass: 1 };  // a little life

const enter = () => {
  opacity.value = withTiming(1, { duration: reduced ? 150 : 250, easing: Easing.out(Easing.cubic) });
  translateY.value = reduced
    ? 0                                        // fade only — no translation
    : withSpring(0, SPRING);
};
```

Note `reduced ? 0` assigns directly rather than animating to 0 — that's the "replace translation with a fade" rule, not "make the translation faster".

**Reanimated's built-in Reduce Motion handling is not the same as the HIG's.** Every animation
already defaults to `ReduceMotion.System`, and layout animations accept `.reduceMotion(...)`:

```js
import { ReduceMotion, ReducedMotionConfig, withTiming, BounceIn, FadeIn } from 'react-native-reanimated';

withTiming(v, { duration, reduceMotion: ReduceMotion.System });  // System (default) | Always | Never
BounceIn.reduceMotion(ReduceMotion.System);                       // layout animations
<ReducedMotionConfig mode={ReduceMotion.Never} />                  // app-wide override
```

What `System` does when the setting is on is **finish the animation instantly** (and skip exiting
animations and shared transitions). That is not what Apple asks for: the prescription is to *replace*
translation with a fade, not to remove the transition. So relying on the default gives you a
compliant-looking library setting and a non-compliant UI — an element that should cross-fade instead
snaps into place.

Treat the default as a safety net for animations you forgot, and branch explicitly on
`useReducedMotion()` for anything a person will actually notice. Where an entering animation should
become a fade, swap the animation rather than damping it:

```js
const reduced = useReducedMotion();
const entering = reduced ? FadeIn : BounceIn;
```

`ReducedMotionConfig` with `ReduceMotion.Never` disables the whole mechanism app-wide. It exists for
apps that handle the setting entirely by hand; reaching for it to "fix" animations that look wrong
under Reduce Motion is how apps ship ignoring the setting.

### Match the gesture's direction

```jsx
// A sheet dragged down must dismiss downward. Deriving the exit from the
// gesture axis prevents the mismatch Apple warns about.
const dismiss = () => {
  translateY.value = withSpring(SCREEN_HEIGHT, SPRING); // same axis it came from
};
```

Prefer gesture-tracked animation over fire-and-forget for anything the user is physically manipulating — it's both the realistic-feedback rule and the Reduce Motion prescription at once:

```jsx
const pan = Gesture.Pan()
  .onUpdate(e => { translateY.value = Math.max(0, e.translationY); })  // 1:1 with the finger
  .onEnd(e => {
    // Velocity-aware: a fast flick dismisses even from a short distance.
    const shouldDismiss = e.translationY > SHEET_HEIGHT * 0.4 || e.velocityY > 800;
    translateY.value = withSpring(shouldDismiss ? SHEET_HEIGHT : 0, { ...SPRING, velocity: e.velocityY });
  });
```

Passing `velocity` into the spring is what makes the release feel continuous with the throw rather than like a separate animation starting from rest.

### Make motion cancellable

```jsx
// Reanimated springs are interruptible by default — assigning a new target
// mid-flight retargets from the current position and velocity.
// This is what "let people cancel motion" means in practice, and it's why
// you should avoid gating interaction behind an animation callback:

// Bad: input is dead until the animation reports back.
const [animating, setAnimating] = useState(false);
<Pressable disabled={animating} onPress={() => { setAnimating(true); animate(() => setAnimating(false)); }} />

// Good: the next press simply retargets the running animation.
<Pressable onPress={() => { open.value = withSpring(open.value > 0.5 ? 0 : 1, SPRING); }} />
```

### Don't animate high-frequency interactions

```jsx
// A list row's press state should be instant feedback, not a choreographed transition.
// Pressable's built-in style callback is the right tool — no Reanimated needed.
<Pressable style={({ pressed }) => [s.row, pressed && s.rowPressed]} />
```

If you find yourself writing a spring for something that happens on every list tap, that's the "avoid motion on frequent interactions" rule telling you to stop.

### Run animations off the JS thread

None of the above survives a busy JS thread. Use Reanimated worklets or `useNativeDriver: true` with the legacy `Animated` API — a dropped-frame animation reads as broken regardless of how correct the timing values are.

```js
Animated.timing(value, { toValue: 1, duration: 250, useNativeDriver: true }).start();
// useNativeDriver can't animate layout props (width/height/top/left).
// Animate transform and opacity instead — which is also cheaper on the GPU.
```

### Practical starting values

Not HIG specifications — Apple doesn't publish spring constants — but these land close to system feel and are a reasonable default before tuning:

| Purpose | Values |
|---|---|
| Screen/modal transition | spring `damping: 20, stiffness: 200` |
| Sheet present/dismiss | spring `damping: 22, stiffness: 240`, seeded with gesture velocity |
| Small state change (toggle, checkmark) | timing `150–200 ms`, `Easing.out(Easing.cubic)` |
| Press feedback | `≤ 100 ms`, or no animation at all |
| Loading/indeterminate | continuous, and stopped entirely under Reduce Motion |

Anything over ~350 ms for a routine transition will feel slow on repeat use, whatever it looks like in isolation.

### Frame rate

```js
// Verify with the RN dev menu's Perf Monitor, or Reanimated's frame callback in dev.
// Target 60 fps; on 120 Hz ProMotion displays, iOS drives Reanimated animations
// at the higher rate automatically — don't hard-code 16.67 ms frame assumptions.
```

## Review checklist

- [ ] Every custom animation checks Reduce Motion (`useReducedMotion` or `AccessibilityInfo`).
- [ ] Under Reduce Motion: translations become fades, springs are critically damped, no z-axis or blur animation, autoplay/looping stopped.
- [ ] No information conveyed by motion alone; haptics or audio supplement it.
- [ ] Exit direction matches entry direction / the gesture axis.
- [ ] Gesture-driven interactions track the finger 1:1 and carry release velocity into the spring.
- [ ] Animations are interruptible; no input disabled while animating.
- [ ] No custom animation on high-frequency interactions that the system already handles.
- [ ] Routine transitions ≤ ~350 ms.
- [ ] All animations run on the native/UI thread (`useNativeDriver` or Reanimated worklets).
- [ ] Animating `transform`/`opacity` rather than layout properties.
- [ ] Animated SF Symbols preferred over hand-built symbol animation.
- [ ] visionOS: no peripheral motion, no world rotation, large moving objects made translucent or low-contrast, fades used for relocation, no sustained ~0.2 Hz oscillation, stationary frame of reference provided.
- [ ] watchOS: timing doesn't assume a linear curve (built-in easing can't be removed).
