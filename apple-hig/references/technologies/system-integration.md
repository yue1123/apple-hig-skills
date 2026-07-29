# Siri, Generative AI, Maps, SharePlay, iCloud & Other Technologies

Source: HIG › Technologies — [Generative AI](https://developer.apple.com/design/human-interface-guidelines/generative-ai), [Machine learning](https://developer.apple.com/design/human-interface-guidelines/machine-learning), [Siri](https://developer.apple.com/design/human-interface-guidelines/siri), [Maps](https://developer.apple.com/design/human-interface-guidelines/maps), [SharePlay](https://developer.apple.com/design/human-interface-guidelines/shareplay), [iCloud](https://developer.apple.com/design/human-interface-guidelines/icloud), [App Clips](https://developer.apple.com/design/human-interface-guidelines/app-clips), [Mac Catalyst](https://developer.apple.com/design/human-interface-guidelines/mac-catalyst), [AirPlay](https://developer.apple.com/design/human-interface-guidelines/airplay), [Augmented reality](https://developer.apple.com/design/human-interface-guidelines/augmented-reality), [HealthKit](https://developer.apple.com/design/human-interface-guidelines/healthkit), [HomeKit](https://developer.apple.com/design/human-interface-guidelines/homekit), [CarPlay](https://developer.apple.com/design/human-interface-guidelines/carplay), [NFC](https://developer.apple.com/design/human-interface-guidelines/nfc), [Live Photos](https://developer.apple.com/design/human-interface-guidelines/live-photos), [Photo editing](https://developer.apple.com/design/human-interface-guidelines/photo-editing), [ShazamKit](https://developer.apple.com/design/human-interface-guidelines/shazamkit), [ResearchKit](https://developer.apple.com/design/human-interface-guidelines/researchkit), [CareKit](https://developer.apple.com/design/human-interface-guidelines/carekit), [iMessage apps and stickers](https://developer.apple.com/design/human-interface-guidelines/imessage-apps-and-stickers), [Always On](https://developer.apple.com/design/human-interface-guidelines/always-on)

## Contents

- [Generative AI](#generative-ai)
- [Machine learning feedback](#machine-learning-feedback)
- [Siri and App Intents](#siri-and-app-intents)
- [Maps](#maps)
- [SharePlay](#shareplay)
- [iCloud](#icloud)
- [App Clips](#app-clips)
- [Mac Catalyst](#mac-catalyst)
- [Shorter notes](#shorter-notes)
- [React Native mapping](#react-native-mapping)
- [Review checklist](#review-checklist)

## Generative AI

**Design responsibly.** Generative AI is easy to prototype and hard to make robust: small input changes — or the *same* input twice — often produce very different outcomes, and you can't anticipate every request or response. Orient the design process around inclusivity, care, and privacy.

**Keep people in control.** Respect their agency: honor in-scope requests with clear expected output, handle sensitive content carefully, let people **dismiss** unwanted content, and let them **revert or retry** transformations they disagree with. **Clearly identify when and where you use AI.**

**Ensure an inclusive experience.** Models favor the most common information in their training data, which produces biases and stereotypes. **Ask people for the information the feature needs rather than inferring personal or cultural characteristics**, and **seek clarity before assuming** things like gender identity or relationship type. Test across a diverse set of people.

**Offer generative features only where they provide clear, specific value** — time savings, improved communication, enhanced creativity.

**Provide a non-AI fallback where possible.** Sometimes AI is essential with no substitute; often it's complementary. Genmoji is fun, but regular emoji still work; Apple Intelligence summarization speeds up notifications, but notifications are still readable without it.

**Communicate where you use AI.** **Never trick someone into thinking they're interacting with a human**, and align disclosure with regional regulations.

**Set clear expectations about capabilities and limits.** A brief tutorial on introduction; **curated suggestions** for open-ended features like a prompt or search bar; up-front disclosure of known limitations, guidance on getting good results, and an explanation of why inferior results occur.

**Choose the model type for the feature's needs *and* privacy.** On-device models keep information local, respond quickly, and work offline. Server models offer more processing and larger context. For any server processing: **process as much locally as possible, minimize what's shared, and be transparent** about what's sent, what's stored off-device, and what's used for training.

**Ask permission before using personal information and usage data.** Use the minimum needed, always offer a clear opt-out, and get **explicit** permission for model improvement or storage. Understand third parties' privacy practices if you share data. **Model outputs can inadvertently contain sensitive information.** Apps for kids have stricter rules.

**Disclose clearly how your app and model use and store personal information** — concise, specific, easy to understand, and explicit about whether personal information is used for training.

## Machine learning feedback

**Request explicit feedback only when necessary.** It requires action from people; prefer **implicit** feedback from how they already interact.

**Always make explicit feedback voluntary.** Communicate that it helps without implying it's mandatory.

**Describe each option and its consequence in simple, direct language.** **Avoid imprecise terms like "dislike"** — they don't convey consequences and translate poorly. Say what will happen instead.

**Add icons to clarify an option**, but **never an icon alone** — it can't convey granularity or consequence.

**Consider multiple, progressively more specific options** so people can clarify their response and remove unwanted suggestions.

**Act immediately on feedback and persist the change.** Hide content people rejected, everywhere. Reacting immediately and remembering is what builds trust that feedback is worth giving.

**Consider feedback about *when and where* to show results** — people may like a result but not in a given context.

**Secure people's information.** Implicit feedback can gather sensitive data.

**Help people control their information.** People are often surprised when behavior in one app affects another, and may assume apps are sharing private information — which costs trust. Explain how you get and share information, and give ways to restrict it.

**Don't let implicit feedback shrink exploration.** Reinforcing current behavior improves the short term and can worsen the long term — matching everything someone is interested in *now* doesn't encourage discovering anything new.

## Siri and App Intents

**Identify your most popular actions, and when and where they occur.** Context — hands-free, a particular device — tells you which actions and entities to expose and how to design the Siri experience.

**Use familiar terms** for content and actions. An audio file could be a track, a song, or a podcast; pick what people will recognize.

**Offer relevant content to Spotlight**, not everything — recent searches, favorites, bookmarks, wishlist items. Some categories (email, messaging) reasonably treat their whole catalog as relevant.

**Don't advertise.** No ads, marketing, or in-app purchase pitches in content Siri delivers.

**Only provide a custom response if built-in ones don't meet your needs.**

**Write clear, descriptive response dialogue** that conveys what happened. Customize follow-up questions for clarity — **"Which soup?" beats "Which one?"**

**Keep responses succinct.** People hear the same response repeatedly across follow-ups and errors. Use conversational context to drop details, and **avoid unnecessary words and attempts at humor** — both become irritating.

**Provide responses Siri can deliver audibly *and* visually**, so it can choose. Asking for weather on iPhone shows the forecast; on AirPods Siri speaks it. **The voice response must stand alone** without depending on visual elements.

**Design inclusive interactions.** Avoid unnecessary pronouns: for "Send a message to my best friend", ask **"Who should I send it to?"** rather than "What's his or her name?"

**Ask an open-ended question when the option list is too long to read** — "What kind of shoes are you interested in?" rather than enumerating.

App Shortcut phrasing and editorial rules are in [components/system-experiences.md](../components/system-experiences.md).

## Maps

**Make your map interactive.** People expect to zoom and pan. **Non-interactive elements that obscure the map interfere with those expectations.**

**Pick a map emphasis style that suits your app** — the two styles differ in how much the map itself competes with your content.

**Help people find places** — search combined with category filters. A mall map's filters might be clothing, housewares, electronics, jewelry, toys.

**Clearly identify selected elements** with distinct styling — an outline plus a color variation.

**Cluster overlapping points of interest.** One pin representing several nearby points, expanding progressively as people zoom in.

**Keep the Apple logo and legal link visible.** Temporary covering by your interface is fine; permanent covering isn't.

**Match annotations to your app's visual style.** The default marker is red with a white pin; you can change the tint and use a string or image icon. **Keep icon strings to two or three characters** for readability.

**Consider making standard map features independently selectable** so Apple-provided points of interest, territories, and physical features get their own custom appearance and information when selected.

**Use overlays for map areas with a specific relationship to your content.**

**Ensure enough contrast between custom controls and the map** — a thin stroke, a light drop shadow, or a blend mode on the map area beneath.

## SharePlay

**Indicate that you support SharePlay** — the `shareplay` SF Symbol on shareable content.

**Help nonsubscribers join.** Temporary or provisional access, a one-time pass from an existing subscriber, or Family Sharing. If people can subscribe *during* a session, **present a streamlined sign-up so others aren't left waiting.**

**Support Picture in Picture** where possible.

**Use the term "SharePlay" correctly.** A noun ("Join SharePlay") and a verb for a direct action ("SharePlay Movie"). **No adjectives** — don't add "virtual" or "spatial" on visionOS. **No variations** — never "SharePlayed", "SharePlays", "SharePlaying".

**Briefly describe each activity** so invitees understand what they're joining — a movie's title, plot summary, and poster. Short enough to avoid truncation.

**Make starting easy.** With no session available, present UI to start a group activity; the system then asks whether to share or continue solo.

**Handle prerequisites before showing the activity.** Login, downloads, or payment come first, as simply as possible.

**Defer app tasks that would delay the shared activity** — ask for a participant's profile when playback pauses or finishes, not at join time.

**Choose a spatial Persona template** on visionOS: side-by-side, surround, or conversational.

**Be prepared to launch directly into the shared activity.** When someone shares on a FaceTime call, the system launches your app for everyone — **avoid displaying any window unrelated to the activity**, and put required input like sign-in in an **auto-dismissible** window.

## iCloud

**Make it easy to use.** People turn iCloud on in Settings and expect apps to work with it automatically. If a choice is genuinely warranted, offer **one simple option on first launch**: all data in iCloud, or none.

**Don't ask which documents to keep in iCloud.** People expect everything available and don't want to manage individual documents. Automate file management.

**Keep content up to date**, balanced against storage and bandwidth. With very large documents, let people control downloads — then **indicate that a newer version exists**, and give subtle feedback if a download takes more than a few seconds.

**Respect iCloud storage space.** It's finite and paid for. Store what people create and understand; **never app resources or regenerable content.** Note that **iCloud backups include every app's Documents folder**, so be selective about what goes there even without iCloud support.

**Behave appropriately when iCloud is unavailable.** No alert needed when someone turns it off or enables Airplane Mode — but unobtrusively noting that changes won't reach other devices yet is helpful.

**Keep app state in iCloud** where it should apply across devices — a magazine app storing the last page read. **Only for settings people want everywhere**; some settings are more useful at work than at home.

**Warn before deleting a document.** Deletion removes it from iCloud and every other device, so **show a warning and get confirmation.**

**Resolve conflicts promptly and easily.** Detect and resolve automatically where possible; otherwise show an unobtrusive notification that makes differentiating and choosing between versions easy. **Earlier is better** — otherwise time is spent in the wrong version.

**Include iCloud content in search results.** People assume their content is universally available.

**For games, consider iCloud-saved progress** — the GameSave framework syncs save data and provides built-in alerts for offline play and conflicts.

## App Clips

**Let people complete a task or demo entirely in the App Clip.** **Never require installing the full app** to finish a demo, task, or game level.

**Focus on essential features.** Reserve advanced or complex features for the app.

**Don't use App Clips for marketing.** They must provide real value. **No advertising services or products, and no ads.**

**Avoid web views.** App Clips use native components for app-quality experience. If only web components are available, **link to your website instead of shipping an App Clip.**

**Design a linear, focused interface.** **No tab bars, complex navigation, or settings.** Minimum screens and entry forms.

**Show the most relevant part on launch**, skipping unnecessary steps.

**Be usable immediately** — all required assets included, **no splash screens**, no waiting on launch.

**Keep it small.** Smaller launches faster, which matters most when bandwidth is limited. Remove unused code and assets, and **avoid downloading additional data** — it removes the feeling of immediacy.

**Make it shareable**, including links to specific points in the App Clip.

**Make paying easy** — support Apple Pay for express checkout and typing-free shipping information.

## Mac Catalyst

Four pieces of iPad work carry over directly, which is why the iPadOS guidance is the prerequisite:

- **Drag and drop** — iPad support gives you Mac support.
- **Keyboard navigation and shortcuts** — appreciated on iPad, **expected** on Mac.
- **Multitasking** — scaling well for Split View, Slide Over, and PiP lays the groundwork for Mac window resizability.
- **Multiple windows** — supporting multiple scenes on iPad gives multiple windows on Mac.

**Adopting the Mac idiom requires a layout audit.** Consider a **separate asset catalog** for the Mac app rather than reusing the iPad one.

**Adjust font sizes.** With the Mac idiom, text renders at **100% of its configured size**, which often looks too large. **Prefer text styles over fixed font sizes.**

**Check views and images.** They also render at 100%, appearing more detailed — assets may need Mac-specific versions.

**Limit appearance customizations** to standard macOS ones; not every iPadOS control customization exists on macOS.

**Keep tab-bar items reachable.** Whether you use a split view or segmented control instead of a tab bar, **list top-level items in the macOS View menu.**

**Offer multiple ways to move between pages** — **Next and Previous buttons** in addition to swipe gestures, for pointer and keyboard users.

## Shorter notes

**AirPlay** — support audio and video rerouting; use the system route picker rather than a custom one.

**Augmented reality** — help people find suitable surfaces, indicate when tracking is limited, keep interactions comfortable, and never require movement into unsafe space.

**HealthKit** — request only the data types you need; be explicit about read vs. write; never surprise people with what you write into Health.

**HomeKit** — mirror the Home app's organizational model (homes, rooms, accessories, scenes) so people's mental model transfers.

**CarPlay** — templates only; you don't design custom screens. Minimize glances and interaction steps.

**NFC** — explain what's about to happen before a scan; provide clear success and failure feedback.

**Live Photos** — indicate that a photo is a Live Photo, and don't autoplay in a way that surprises people.

**Photo editing** — non-destructive edits, with a clear revert path.

**ShazamKit** — indicate listening state clearly; handle no-match gracefully.

**ResearchKit / CareKit** — informed consent must be genuinely informed; medical and research contexts carry legal obligations beyond design guidance.

**iMessage apps and stickers** — keep interactions in the compact presentation where possible; stickers should be recognizable at small sizes.

**Always On** (iPhone and Apple Watch) — redact sensitive information, dim nonessential content, keep layout stable, and transition motion gracefully to rest. → [platforms/watchos.md](../platforms/watchos.md)

## React Native mapping

### Maps

```jsx
import MapView, { Marker, Callout } from 'react-native-maps';

<MapView
  // Apple Maps on iOS by default — correct platform behavior for free.
  style={StyleSheet.absoluteFill}
  // Interactive by default; don't disable zoom/pan.
  showsUserLocation
  // Clustering rather than hundreds of overlapping pins.
  clusterColor={colors.accent}
>
  {points.map(p => (
    <Marker
      key={p.id}
      coordinate={p.coord}
      pinColor={colors.accent}          // tint to match your app
      // Distinct selected styling.
      onPress={() => setSelected(p.id)}
      // Accessible — a map of unlabeled pins is unusable with VoiceOver.
      accessibilityLabel={`${p.name}, ${p.category}`}
    />
  ))}
</MapView>
```

Two things RN developers routinely get wrong here: **covering the Apple logo and legal link** with a bottom sheet that's always up, and **omitting marker accessibility labels**. Both are explicit HIG requirements.

Use `react-native-maps` with `PROVIDER_DEFAULT` on iOS so you get Apple Maps; forcing Google Maps on iOS is a platform-consistency loss (and a different legal attribution requirement).

For clustering at scale, `react-native-map-clustering` or the native clustering in newer `react-native-maps` versions — hand-rolled clustering in JS stalls on large datasets.

### Generative AI features

```jsx
// Disclose AI, keep people in control, and offer retry/revert.
function AISummary({ text }) {
  const [summary, setSummary] = useState(null);
  const [history, setHistory] = useState([]);

  return (
    <View>
      {/* Disclosure — never let this read as human-authored. */}
      <View style={s.aiBadge}>
        <Icon name="sparkles" />
        <Text style={text.caption1}>Summary generated by AI</Text>
      </View>

      <Text style={text.body}>{summary ?? text}</Text>

      <View style={{ flexDirection: 'row', gap: space.md }}>
        {/* Retry and revert are requirements, not conveniences. */}
        <Button title="Try Again" onPress={regenerate} />
        <Button title="Show Original" onPress={() => setSummary(null)} />
        <Button title="Dismiss" role="cancel" onPress={dismiss} />
      </View>
    </View>
  );
}
```

Curated suggestions for open-ended prompts, per the expectation-setting rule:

```jsx
// An empty prompt box teaches nothing. Seed it.
{prompt === '' && (
  <View>
    <Text style={text.footnote}>Try asking:</Text>
    {SUGGESTIONS.map(sug => <Chip key={sug} label={sug} onPress={() => setPrompt(sug)} />)}
  </View>
)}
```

On-device vs. server, and the privacy consequence:

```js
// Prefer on-device where the feature allows it — it removes the data-sharing
// conversation entirely. Foundation Models / Core ML need a native module;
// expo-modules can wrap them.
// If you must call a server, tell people, minimize what's sent, and offer opt-out.
const useOnDevice = await FoundationModels.isAvailable();
if (!useOnDevice) {
  if (!prefs.allowServerProcessing) return showServerConsentSheet();
  // Strip identifiers before sending.
  await server.summarize(redact(text));
}
```

### App Intents / Siri

Intents are Swift. The workable split:

```
// Intents that OPEN the app: Swift AppIntent → posts into the RN bridge → router.
// Intents that COMPLETE without opening: entirely Swift.
```

```js
// Spotlight/Siri entity donation also needs native work (Core Spotlight).
// Only donate relevant content — recents, favorites, bookmarks — not your catalog.
```

### SharePlay

```js
// GroupActivities has no RN binding. A native module is required, and the
// "launch directly into the activity" requirement means your app's cold-start
// path must be able to skip straight to the activity screen.
```

### iCloud

```js
// Key-value app state (last page read, cross-device settings):
import { iCloudStorage } from 'react-native-icloudstore';
await iCloudStorage.setItem('lastReadPage', String(page));

// Documents: CloudKit / NSUbiquitousContainer need native work.
// Don't put regenerable caches in the Documents folder — iCloud backups
// include it whether or not you support iCloud.
```

```jsx
// Confirm before deleting, because it deletes everywhere.
Alert.alert('Delete Document?', 'This removes it from iCloud and all your devices.',
  [{ text: 'Cancel', style: 'cancel' }, { text: 'Delete', style: 'destructive', onPress: del }]);
```

### App Clips

App Clips are a **separate lightweight target** and can't be a React Native bundle in practice — RN's runtime and bundle size fight the "small and instant, no splash screen, no additional downloads" requirements directly. Build App Clips in SwiftUI.

### Mac Catalyst

If you go Catalyst rather than `react-native-macos`, the RN work is entirely the iPadOS work in [platforms/ipados.md](../platforms/ipados.md) — plus:

```jsx
// Text at 100% looks large under the Mac idiom. Use the type scale, not fixed sizes,
// so a Mac-specific scale can be substituted.
import { text } from '@/tokens';   // → rn/design-tokens.md

// Next/Previous buttons alongside swipe, for pointer and keyboard users.
{Platform.OS === 'ios' && isMacCatalyst && <PageControls onPrev={prev} onNext={next} />}
```

## Review checklist

- [ ] AI features are disclosed; nothing implies human authorship.
- [ ] AI output can be dismissed, reverted, and retried.
- [ ] AI feature limitations stated up front, with curated suggestions for open-ended inputs.
- [ ] A non-AI path exists where the feature is complementary rather than essential.
- [ ] Personal characteristics requested rather than inferred; feature tested across diverse people.
- [ ] On-device processing preferred; server processing disclosed, minimized, and opt-out-able.
- [ ] Explicit permission obtained for using personal data in model improvement or storage.
- [ ] ML feedback is voluntary, described by consequence (not "dislike"), acted on immediately, and persisted.
- [ ] Implicit feedback doesn't collapse the experience into only what someone already likes.
- [ ] Siri responses are succinct, stand alone audibly, use familiar terms, and avoid unnecessary pronouns.
- [ ] No advertising in Siri- or Spotlight-delivered content; only relevant content donated.
- [ ] Maps are interactive; Apple logo and legal link not permanently covered.
- [ ] Map markers have accessibility labels; overlapping points clustered; selection visibly distinct.
- [ ] Map annotation icon strings ≤ 3 characters; custom controls have sufficient contrast against the map.
- [ ] Apple Maps used on iOS rather than forcing another provider.
- [ ] SharePlay support indicated with the `shareplay` symbol; the term used without adjectives or variations.
- [ ] Prerequisites (login, download, payment) handled before the shared activity appears.
- [ ] Cold start can go straight to the shared activity; unrelated windows suppressed.
- [ ] iCloud works automatically without per-document management; no regenerable content stored there or in Documents.
- [ ] Document deletion warns that it removes the file from every device.
- [ ] Conflicts resolved automatically where possible, otherwise surfaced unobtrusively and early.
- [ ] iCloud content included in search results.
- [ ] App Clips complete a real task, are native, linear, splash-free, small, and free of ads.
- [ ] Mac Catalyst: layout audited, text scaled via type styles, tab items listed in the View menu, Next/Previous buttons offered.
