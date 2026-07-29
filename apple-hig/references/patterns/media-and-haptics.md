# Audio, Video & Haptics

Source: HIG › Patterns › [Playing audio](https://developer.apple.com/design/human-interface-guidelines/playing-audio), [Playing video](https://developer.apple.com/design/human-interface-guidelines/playing-video), [Playing haptics](https://developer.apple.com/design/human-interface-guidelines/playing-haptics)

Read this when playing sound, building a video player, or adding haptic feedback.

## Contents

- [Audio](#audio)
- [Audio session categories](#audio-session-categories)
- [Audio interruptions](#audio-interruptions)
- [Video](#video)
- [Haptics](#haptics)
- [Platform differences](#platform-differences)
- [React Native mapping](#react-native-mapping)
- [Review checklist](#review-checklist)

## Audio

Three expectations the system sets on your behalf:

- **Silent mode** means *nonessential* sound stops — keyboard clicks, sound effects, game soundtracks, audible feedback. Only explicitly initiated audio continues: media playback, alarms, audio/video messaging.
- **Volume** applies to all system sound including music and in-app effects, however the person adjusts it. (The iPhone ringer volume is the one separately adjustable exception.)
- **Headphones** — connecting reroutes automatically without interruption; **disconnecting pauses playback immediately**. That second half is what people notice when it's missing.

**Adjust relative levels, never overall volume.** Mix your own sources however you like; system volume governs the final output.

**Permit audio rerouting** — living room stereo, car radio, Apple TV — unless there's a compelling reason not to. Use the system volume view, which includes both the level slider and the output picker.

**Respond to external audio controls only when it makes sense.** Control Center and headphone controls reach you whether you're foreground or background. Respond if you're actively playing, in a clear audio context, or connected via Bluetooth/AirPlay. Otherwise, don't — halting another app's audio because someone pressed play is a genuinely bad outcome.

**Never repurpose audio controls.** If you don't support a control, don't respond to it. Build custom player controls only for commands the system doesn't have — custom skip increments, or content related to what's playing like a sports score.

**Signal when you finish temporary audio** so other apps know they can resume.

## Audio session categories

Pick the category that matches how you actually use sound. Choosing wrong is how apps end up silencing someone's music for no reason.

| Category | Use when | Silent switch | Mixes | Background |
|---|---|---|---|---|
| **Solo ambient** | Sound isn't essential but should silence other audio — a game with a soundtrack | Responds | No | No |
| **Ambient** | Sound isn't essential and shouldn't silence others — a game where people play their own music | Responds | Yes | No |
| **Playback** | Sound is essential — audiobook, language learning; may continue after leaving the app | Ignores | Maybe | Yes |
| **Record** | Recording — a note-taking app's audio mode | Ignores | No | Yes (record) |
| **Play and record** | Both, possibly simultaneously — audio messaging, video calling | Ignores | Maybe | Yes |

The distinction that matters most in practice is **Ambient vs. Solo ambient**: use Ambient unless your sound genuinely needs exclusivity.

## Audio interruptions

**Decide deliberately how to respond.** A recording app can ask the system not to interrupt for an incoming call unless the person accepts it. A VoIP app must end a call when the iPad Smart Folio closes — closing it mutes the microphone, and *restarting the session when the Folio reopens would unmute the mic without the person's knowledge*, which is a privacy violation. Inspect the interruption rather than reacting uniformly.

**Resume automatically only when appropriate.** Interruptions are **resumable** (incoming phone call) or **nonresumable** (someone started a new playlist). Combine the type with your app's nature:

- A **media playback app** that was actively playing should check the interruption is resumable before continuing.
- A **game** can just resume, because its audio wasn't an explicit user choice in the first place.

## Video

**Use the system video player.** It gives consistent interactions people already know. If you truly need a custom player, mirror the system player's behavior and interface — a custom player that diverges *slightly* is worse than one that diverges obviously, because people can't tell which of their habits still work.

**Always display video at its original aspect ratio.** Video with letterbox/pillarbox padding **baked into the frame** can't be scaled correctly by the system: it appears smaller in both full-screen and fit-to-screen modes, and breaks edge-to-edge non-full-screen contexts like Picture in Picture on iPad.

The two scaling modes and their defaults:

- **Aspect-fill** (fills the display, edges may crop) — default for **wide** video, 2:1 through 2.40:1.
- **Aspect-fit** (whole frame visible, letterbox/pillarbox as needed) — default for **standard** video (4:3, 16:9, up to 2:1) and **ultrawide** (above 2.40:1).

**Provide additional metadata when it adds value** — image, title, description — without obscuring playback.

**Support the expected input for every device.** **Space** plays/pauses on a connected keyboard across Vision Pro, Mac, iPhone, iPad, and Apple TV. Siri Remote gestures on Apple TV.

**Don't let audio from different sources mix.** The canonical failure: watching full-screen video → move to PiP (system mutes it) → start a game with background music → return to the PiP window and unmute. If the game mishandles secondary audio, both play at once. Handle the secondary-audio hint.

### Loading and exiting

- **Avoid loading screens** if content loads fast. Past **two seconds**, show a **black** screen with a centered spinner and nothing else.
- **Start playback as soon as enough content has buffered**; keep loading the rest in the background.
- **Keep loading screens minimal** — the black background is what makes the transition to playback seamless.
- **On exit, show a contextually relevant screen** — a detail view for what they were watching, with a resume option. Failing that, a menu listing the content, or the main menu.
- **Prepare the exit view immediately** after a playback notification, in case someone exits right after playback starts.

### Integrating with the TV app (tvOS)

- **The TV app fades to black and skips your launch screen.** Present your own black screen immediately, then go straight to content.
- **No splash screens, detail screens, or intro animations** before the selected media. If an interstitial is unavoidable, Select steps through it and Play skips it.
- **Don't ask whether to resume** — just resume, from the previous end time.
- **Switch to the profile the TV app specifies** before playing. If none is specified, ask once and remember.

## Haptics

**Use system haptic patterns per their documented meanings.** People learn these from standard controls. If a pattern's documented use doesn't fit your case, use a generic pattern or a custom one — don't redefine a standard pattern's meaning.

**Be consistent within your app.** Each haptic needs a clear causal relationship with the action that triggered it. Apple's example: if a failed mission plays pattern X, people associate X with failure — reusing X for a level completion is actively confusing.

**Complement other feedback, don't replace it.** Match a haptic's intensity and sharpness to the animation it accompanies; synchronize with sound where relevant. Visual + auditory + tactile in harmony is what makes an interaction feel physical.

**Don't overuse.** A haptic that feels perfect occasionally becomes tiresome at high frequency. The target Apple describes is worth remembering: *the best haptic experience is one people aren't conscious of, but miss when it's off.*

**Prefer short haptics tied to discrete events** in apps. Long-running haptics suit gameplay flow but dilute meaning in an app. On Apple Pencil Pro specifically, continuous haptics don't clarify writing or drawing and make holding the pencil less pleasant.

**Make haptics optional**, and make sure the app is fully enjoyable without them.

**Watch for interference.** Haptics produce real physical force — make sure they don't disrupt the camera, gyroscope, or microphone.

### Custom haptics

- **Transient** events — brief and compact, like a tap or impulse (tapping the Flashlight button).
- **Continuous** events — sustained vibration (the lasers effect in Messages).

## Platform differences

### iOS

Standard components — toggles, sliders, pickers — already play Apple-designed haptics. Beyond those, use a feedback generator with the predefined **notification**, **impact**, and **selection** categories rather than composing custom patterns.

Use the system sound services for short sounds and vibrations.

### macOS

Three trackpad haptic patterns:

| Pattern | Meaning |
|---|---|
| **Alignment** | A dragged item aligned — snapping to another shape, fitting dimensions, reaching a scrubber's start/end or min/max |
| **Level change** | Movement between discrete pressure levels — e.g. fast-forward speed steps |
| **Generic** | General feedback when neither of the above applies |

### watchOS

A named vocabulary, each with a specific meaning — using them correctly is what makes watchOS haptics legible:

| Haptic | Meaning |
|---|---|
| **Notification** | Something significant or unusual needs attention (same haptic as an arriving notification) |
| **Up** | An important value rose above a significant threshold |
| **Down** | An important value fell below a significant threshold |
| **Success** | An action completed successfully |
| **Failure** | An action failed |
| **Retry** | An action failed but can be retried |
| **Start** | An activity started (a timer) — usually paired with Stop |
| **Stop** | An activity stopped |
| **Click** | A dial clicking — progress at predefined increments. Overuse diminishes it, and overlapping clicks are confusing |

Media encoding: **64 kbps HE-AAC** for audio. Video: **H.264 High Profile, 160 kbps at up to 30 fps**, 208×260 px full-screen portrait or 320×180 px for 16:9. **Keep clips under 30 seconds** — long clips cost disk space and require holding the wrist up, which is tiring. Don't scale video clips.

Poster images: represent the clip's actual content, and **don't make them look like a system control** — people should understand it's a movie to tap, not a button.

Consider a **Now Playing view** so people can control current or recent audio without leaving your app; it also shows the current source, which may be another app on the Watch or iPhone.

### tvOS

**Defer to content with overlays.** Small unobtrusive logos or countdown timers are fine; large distracting overlays aren't. Some displays are prone to **image retention**, so keep overlays short and prefer translucent SDR graphics over bright opaque ones.

**Interactive overlays** (quizzes, surveys, check-ins): implement a **minimum 0.5 s delay** before pausing media and showing the overlay, and give a clear way to dismiss and resume.

### visionOS

**Prefer playing sound.** People keep sound audible while wearing the device, so a silent app — especially in an immersive moment — feels lifeless or broken. But *never* convey important information by sound alone.

**Design custom sounds for custom UI elements.** System elements play sounds that help locate them and confirm interaction; your custom elements need the same.

**Use Spatial Audio.** *Ambient audio* gives pervasive sound that anchors people in a virtual world; an *audio source* sounds like it comes from a specific object. Use both. Sound follows its object — move a window that's playing audio and the sound moves with it.

**Fixed vs. tracked sound:** *fixed* is perceived as pointed at the wearer regardless of where they look; *tracked* is perceived as coming from an object, so distance changes what they hear. Prefer tracked for realism; fixed suits enveloping experiences (Mindfulness).

**Vary repetitive sounds.** The system subtly randomizes the virtual keyboard's pitch and volume, mimicking how a physical keyboard varies with typing speed and force. Randomizing pitch and volume at playback is cheaper than authoring multiple files.

**Video comfort:** let people choose when to start; use a **small, resizable** window; keep their surroundings visible. **Never auto-start a fully immersive video experience.** In full immersion, the system places the player at a predictable position — don't let virtual content occlude the playback or transport controls in the ornament near the bottom.

**Don't expand an inline player to fill the window.** Inline video should stay 2D with window content visible around it, so people don't expect a more immersive experience. Supply a **thumbnail track** for scrubbing, at **160 px wide** per thumbnail. For video in a splash or transitional view, use a **RealityKit** video player — no playback controls needed, correct aspect ratio for 2D and 3D, closed caption support.

## React Native mapping

### Audio session category

This is the single most consequential audio setting in an RN app, and the default is usually wrong:

```js
import { Audio } from 'expo-av';

// A game or app with incidental sound → Ambient: respects the silent switch,
// and does NOT stop the user's music.
await Audio.setAudioModeAsync({
  playsInSilentModeIOS: false,        // respects silent switch  → "ambient"
  staysActiveInBackground: false,
  interruptionModeIOS: Audio.INTERRUPTION_MODE_IOS_MIX_WITH_OTHERS,
  shouldDuckAndroid: true,
});

// A podcast/audiobook player → Playback: essential audio, ignores silent switch,
// continues in the background.
await Audio.setAudioModeAsync({
  playsInSilentModeIOS: true,
  staysActiveInBackground: true,
  interruptionModeIOS: Audio.INTERRUPTION_MODE_IOS_DO_NOT_MIX,
});
```

`playsInSilentModeIOS: true` on a game's sound effects is the common mistake — it makes silent mode not silence you, which is exactly what people set it for.

For real media apps use `react-native-track-player`, which owns the session, lock-screen controls, and remote events properly:

```js
await TrackPlayer.updateOptions({
  capabilities: [Capability.Play, Capability.Pause, Capability.SkipToNext, Capability.SkipToPrevious, Capability.SeekTo],
  // Only declare capabilities you actually support — per "if you don't support a
  // control, don't respond to it".
});
```

### Interruptions and route changes

```js
// The two-kind rule: resumable vs. nonresumable.
TrackPlayer.addEventListener(Event.RemoteDuck, ({ paused, permanent }) => {
  if (permanent) {
    TrackPlayer.pause();          // nonresumable — stay paused
    wasDucked.current = false;
  } else if (paused) {
    TrackPlayer.pause();
    wasDucked.current = true;     // resumable — remember to come back
  } else if (wasDucked.current) {
    TrackPlayer.play();
    wasDucked.current = false;
  }
});
```

Headphone disconnect must pause immediately:

```js
// react-native-track-player emits this; with expo-av, listen for route changes natively.
TrackPlayer.addEventListener(Event.RemoteDuck, /* … */);
// Android: AudioManager.ACTION_AUDIO_BECOMING_NOISY. iOS: AVAudioSession route change
// with reason .oldDeviceUnavailable. Both need a native module with expo-av.
```

### Video

```jsx
import { VideoView, useVideoPlayer } from 'expo-video';

const player = useVideoPlayer(source, p => { p.loop = false; });

<VideoView
  player={player}
  // Match the HIG defaults to the source aspect ratio.
  contentFit={aspectRatio > 2.0 && aspectRatio <= 2.4 ? 'cover' : 'contain'}
  nativeControls                   // the system player UI — prefer it
  allowsPictureInPicture
  allowsFullscreen
/>
```

`nativeControls` is the "use the system video player" rule. A custom control overlay should only exist for commands the system player lacks.

Never re-encode or crop to change aspect ratio, and never ship source video with baked-in letterboxing — see the PiP consequence above.

Loading, per the two-second rule:

```jsx
{!isReady && elapsed > 2000 && (
  <View style={[StyleSheet.absoluteFill, { backgroundColor: '#000', alignItems: 'center', justifyContent: 'center' }]}>
    <ActivityIndicator color="#fff" />
  </View>
)}
```

Black background specifically — it's what makes the cut to playback invisible.

### Haptics

```js
import * as Haptics from 'expo-haptics';

// Notification category — semantic, matches the documented meanings.
Haptics.notificationAsync(Haptics.NotificationFeedbackType.Success);
Haptics.notificationAsync(Haptics.NotificationFeedbackType.Warning);
Haptics.notificationAsync(Haptics.NotificationFeedbackType.Error);

// Impact — physical collision or a UI element landing.
Haptics.impactAsync(Haptics.ImpactFeedbackStyle.Light);   // Light | Medium | Heavy | Rigid | Soft

// Selection — discrete value changes, e.g. a picker passing each item.
Haptics.selectionAsync();
```

Keep the mapping in one place so consistency is enforceable rather than aspirational:

```js
// One table, one meaning each — this is the "clear causal relationship" rule made structural.
export const haptics = {
  actionSucceeded: () => Haptics.notificationAsync(Haptics.NotificationFeedbackType.Success),
  actionFailed:    () => Haptics.notificationAsync(Haptics.NotificationFeedbackType.Error),
  itemSelected:    () => Haptics.selectionAsync(),
  thresholdPassed: () => Haptics.impactAsync(Haptics.ImpactFeedbackStyle.Rigid),
};
```

Two guards that are easy to forget:

```js
// 1. Make haptics optional and respect the user setting.
if (prefs.hapticsEnabled) haptics.actionSucceeded();

// 2. Android's haptic support is much weaker and its API surface differs.
//    Never make haptics the only feedback channel — see feedback-and-loading.md.
```

Don't call haptics in a scroll or gesture handler on every frame — that's the overuse failure, and on Android it also stutters.

### Spatial Audio on visionOS

`react-native-visionos` doesn't expose Spatial Audio or RealityKit audio from JS. If sound is central to your visionOS experience, budget a native module — a visionOS app with flat stereo audio reads as unfinished per the guidance above.

## Review checklist

- [ ] Audio session category matches actual use; incidental sound uses Ambient and respects the silent switch.
- [ ] `playsInSilentModeIOS` true only for essential, explicitly initiated audio.
- [ ] Only supported remote/lock-screen capabilities declared; unsupported controls not handled.
- [ ] Resumable interruptions resume; nonresumable ones stay paused.
- [ ] Headphone disconnect pauses playback immediately.
- [ ] Audio rerouting/output picking supported; system volume never overridden.
- [ ] Other apps notified when temporary audio ends.
- [ ] Secondary audio handled so sources can't mix (PiP + game case).
- [ ] Video uses the system player; custom controls exist only for commands the system lacks.
- [ ] Video shown at original aspect ratio; no baked-in letterboxing in source assets.
- [ ] `contentFit` follows the HIG default for the source aspect ratio.
- [ ] Loading screen only past ~2 s, black with a centered spinner and nothing else.
- [ ] Playback starts on partial buffer; rest loads in background.
- [ ] Exit returns to a relevant detail view with a resume option; exit view prepared early.
- [ ] Space bar plays/pauses with a connected keyboard.
- [ ] Haptics mapped from a single semantic table; each pattern has one meaning.
- [ ] System haptic patterns used per documented meaning, not repurposed.
- [ ] Haptics complement visual/audio feedback and are never the only channel.
- [ ] Haptics user-disableable; not fired per frame in gestures or scroll.
- [ ] Haptics don't interfere with camera, gyroscope, or microphone use.
- [ ] tvOS: overlays small, short, translucent SDR; interactive overlays delay ≥ 0.5 s before pausing.
- [ ] tvOS TV app integration: immediate black screen, no splash/interstitial, resume without prompting.
- [ ] visionOS: sound present and spatial; no auto-started immersive video; inline player stays 2D and windowed; scrub thumbnails 160 px.
- [ ] watchOS: clips ≤ 30 s, recommended encodings used, poster images not shaped like controls.
