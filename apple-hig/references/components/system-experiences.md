# Widgets, Live Activities, Notifications, Controls & App Shortcuts

Source: HIG › Components › System experiences — [Widgets](https://developer.apple.com/design/human-interface-guidelines/widgets), [Live Activities](https://developer.apple.com/design/human-interface-guidelines/live-activities), [Notifications](https://developer.apple.com/design/human-interface-guidelines/notifications), [Controls](https://developer.apple.com/design/human-interface-guidelines/controls), [App Shortcuts](https://developer.apple.com/design/human-interface-guidelines/app-shortcuts), [Snippets](https://developer.apple.com/design/human-interface-guidelines/snippets), [Status bars](https://developer.apple.com/design/human-interface-guidelines/status-bars)

Top Shelf is in [platforms/tvos.md](../platforms/tvos.md); watch faces and complications are in [platforms/watchos.md](../platforms/watchos.md). Notification *delivery* strategy (interruption levels, Focus) is in [patterns/notifications-sharing-files.md](../patterns/notifications-sharing-files.md).

**Note for React Native:** everything on this page except status bars requires a **native extension** — WidgetKit, ActivityKit, App Intents, and ControlWidget are Swift/SwiftUI targets that cannot be authored in JavaScript. The design guidance still governs what you build, and the RN section below covers the bridge.

## Contents

- [Widgets](#widgets)
- [Live Activities](#live-activities)
- [Notifications](#notifications)
- [Controls](#controls)
- [App Shortcuts](#app-shortcuts)
- [Snippets](#snippets)
- [Status bars](#status-bars)
- [React Native mapping](#react-native-mapping)
- [Review checklist](#review-checklist)

## Widgets

**Choose simple ideas tied to your app's main purpose**, with timely content and relevant functionality. Weather widgets lead with current high/low and conditions because that's what people want.

**Give quick access to content people want.** **Replicating your app icon adds no value** and makes people less likely to keep the widget.

**Prefer dynamic information that changes through the day.** A widget whose content never appears to change loses its prominent position. Widgets don't update minute to minute, so find ways to keep content fresh enough to invite frequent viewing.

**Look for opportunities to delight** — a unique visual treatment on birthdays or holidays.

**Offer multiple sizes only when they add value.** Small widgets typically carry one piece of information; larger ones support additional layers and actions. **Don't stretch a small widget's content to fill a larger area.** One widget in the size that best represents the content beats all sizes done poorly.

**Balance information density.** Sparse layouts make the widget seem unnecessary; dense layouts stop being glanceable. Aim for essentials at a glance with additional detail available on a longer look. If a layout is too dense, use a larger size or **replace text with graphics**.

**Show only information related to the widget's main purpose.** Calendar widgets stay centered on upcoming events at every size, expanding the range rather than changing the subject.

**Use brand elements thoughtfully.** Brand colors, typefaces, and stylized glyphs make the widget recognizable without overpowering useful information. **You rarely need a logo or app icon** — if the widget shows content from multiple sources, a **small logo in the top-right corner** is sufficient.

**Choose automatic content vs. user configuration** deliberately. Stocks requires picking symbols; Podcasts shows recent content automatically.

**Don't mirror your widget's appearance inside your app.** An element that looks like a widget but doesn't behave like one confuses people, and it makes them less likely to try the interactions your app actually offers.

**Say when authentication adds value** — "Sign in to view reservations" when signed out.

### Updating content

**Match update frequency to how often the data changes and when people need it.** A tidal conditions widget is useful hourly even though conditions change continuously. **If people are likely to check more often than you can update, display when the data was last updated.**

**Let the system refresh dates and times** so you preserve your limited update opportunities for actual data.

**Show content quickly rather than hiding stale data behind placeholders.**

**Use animated transitions — up to two seconds — to draw attention to data updates.**

### Interactivity

**Offer simple, relevant functionality; reserve complexity for the app.**

**Deep link to the right location.** Tapping a medium Stocks widget opens Stocks to that symbol's page — not the app's root.

**Stay glanceable.** Multiple interaction targets (links, buttons, toggles) can make sense, but **avoid app-like layouts**. Check target sizes so people can hit them confidently without accidental activation. **Inline accessory widgets have only one tap target.**

### Margins

**Standard margin is 16 pt** for most widgets. **11 pt** works for tighter groupings — graphics, buttons, background shapes. Widgets on the **Mac desktop** and on the **Lock Screen (including StandBy)** use smaller margins.

## Live Activities

**For tasks and events with a defined beginning and end**, ideally **not exceeding eight hours**.

**Focus on glanceable, important information.** It doesn't need to show everything — people tap through to the app for detail.

**No ads or promotions.** Only information related to the ongoing event or task.

**Avoid sensitive information.** Live Activities are prominently visible on the Lock Screen and Always-On display, where casual observers see them. Show an innocuous summary and let people tap through, or **redact** sensitive views and let people configure whether to show them.

**Match your app's visual aesthetic in both light and dark appearances**, so people recognize the Live Activity as yours.

**Include a logo mark without a container**, and **never the whole app icon**.

**Don't add elements to your app that draw attention to the Dynamic Island.** Your Live Activity appears there while your app isn't in use, and other things appear there when it is.

**Use large, medium-weight-or-heavier text.** Small text sparingly, and key information legible at a glance.

### Layout

**Adapt to different screen sizes and presentations** — compact, minimal, expanded, Lock Screen, StandBy. Actual on-screen size varies.

**Use only the space the content needs**, adapting element size and placement so they fit together.

**Use familiar layouts.** Templates with default margins and recommended text sizes are in Apple Design Resources — they keep the Live Activity legible and consistent with its surroundings, like the Smart Stack on Apple Watch.

**Use consistent margins and concentric placement.** Match a rounded rectangle's corner radius to the Live Activity's outer radius **minus the margin**, so nothing pokes into the rounded shape and creates visual tension.

**Separate content blocks with an inset container shape or a thick line** — **don't draw content to the edge of the Dynamic Island.**

**Change height dynamically** on the Lock Screen and in the expanded presentation. A rideshare app is compact while locating a driver, then grows as pickup time and driver details arrive.

### Color

**Background color can't be customized** for compact, minimal, or expanded presentations. It **can** for the **Lock Screen** presentation — and there, ensure sufficient contrast, especially for tint colors on Always-On displays with reduced luminance.

**Use bold colors for text and objects** to convey personality and make the Live Activity recognizable and distinguishable from others. The Dynamic Island background is black and opaque.

**Tint the key line color to match your content.** On a dark background a key line appears around the Dynamic Island; keep it consistent with your other elements.

## Notifications

**Be concise and informative.**

**Never send multiple notifications for the same thing**, even if someone hasn't responded. Filling Notification Center is how you get all your notifications turned off.

**Don't tell people to perform tasks in your app.** Offer **notification actions** for simple tasks instead. Instructions are hard to remember after the notification is dismissed.

**Use an alert, not a notification, for an error message.**

**Handle foreground arrival gracefully.** Your notifications don't display while your app is frontmost, but you still receive the information. Present it **discoverably but not distractingly** — increment a badge, subtly insert new data. Mail just adds a new message to the unread list rather than notifying about it.

**Never include sensitive, personal, or confidential information.** You can't predict who's looking at the screen.

### Content

**Short title, if it provides context.** Use the prominent title area for something useful — a headline, event name, email subject. **If all you can offer is a generic title like "New Document", let the system show your app name instead.** Title-style capitalization, no ending punctuation. Keep it brief especially for Apple Watch.

**Succinct body text** — complete sentences, sentence case, proper punctuation. **Don't truncate yourself**; the system does it when needed.

**Provide hidden-preview placeholder text.** When people hide previews, the system shows only your icon and the default title *Notification*. Supply body text that gives enough context to decide whether to look — "Friend request", "New comment", "Reminder", "Shipment" — in sentence case.

**Don't include your app name or icon.** The system shows a large version of your icon at the leading edge; communication notifications show the sender's contact image badged with a small version of yours.

**Consider a sound.** Short, distinctive, professionally produced, or a system alert sound. **Never rely on it to communicate important information** — people may not hear it. You can't provide an accompanying vibration programmatically.

### Actions

**Provide beneficial actions that save time and avoid opening the app.** Short **title-case** term or phrase describing the result. **No app name or extraneous information**, brief to avoid truncation, written with localization in mind.

**Don't provide an action that merely opens your app** — tapping the notification already does that, so the button clutters the detail view and confuses.

**Prefer nondestructive actions.** If you must include a destructive one, give enough context to avoid unintended consequences; the system styles identified destructive actions distinctly.

**Give each action a recognizable interface icon**, displayed on the **trailing** side of the action title.

### Badging

**Only for the count of unread notifications.** Not for weather data, dates, times, stock prices, or game scores.

**Never make badging the only way you communicate essential information** — people can turn it off.

**Keep badges current.** Update as soon as people open the corresponding notifications. Note that **reducing the count to zero removes all related notifications from Notification Center.**

**Don't mimic a badge with a custom image or component.** People who turned badges off will be frustrated to see what looks like one.

## Controls

Controls appear in Control Center, on the Lock Screen, and on the Action button.

**Offer controls for actions that give the most benefit without launching your app.** Launching a Live Activity from a control is a good pattern — progress becomes visible without navigating to the app.

**Update the control** when someone interacts with it, when an action completes, or remotely via push. Reflect state accurately, including in-progress.

**Choose a descriptive symbol.** Depending on placement, the title and value may not display, so **the symbol must carry the meaning alone**. For toggles, provide **both on and off symbols** — `door.garage.open` / `door.garage.closed`.

**Animate symbol state changes.** Toggles animate the on/off transition; buttons whose actions have a duration animate **indefinitely while running** and stop on completion.

**Select a tint color matching your brand.** The system applies it to a toggle's symbol in its on state, and to the value and symbol in the Dynamic Island when the action runs from the Action button.

**Prompt for configuration when needed** — choosing which light to control — at the time people add the control, and allow reconfiguration later.

**Provide Action button hint text**, constructed with verbs, so people understand what press-and-hold will do.

**Include a placeholder** if the title or value is situational, so the controls gallery and Action button assignment screen show something meaningful.

**Hide sensitive information when the device is locked.** Have the system redact the title and value, and specify whether to redact the symbol state too (if so, the symbol shows in its off state).

**Require authentication for security-affecting actions** — locking or unlocking a door, starting a car.

**Camera experiences on a locked device:** use the **same camera UI as your app**, so the transition into the app after capture is seamless. **Provide instructions for adding the control.**

## App Shortcuts

**Consider app schemas first.** Apps in common domain areas can adopt **App schema domains** to make actions and content available to Apple Intelligence, letting Siri and system experiences surface features contextually without individual App Shortcuts.

**Offer App Shortcuts for your most common and important tasks.** Straightforward tasks completable without leaving the current context work best, though opening your app is fine for multistep tasks.

**A shortcut can include a single optional parameter.** "Start [morning, daily, sleep] meditation." **Use predictable, familiar values** — people won't have the list in front of them.

**Ask for clarification when optional information is missing.** Suggest the most recently used option, or one based on time of day. If one option is most likely, make it the default with a short list of alternatives.

**Keep voice interactions simple.** If the phrase feels complicated when spoken aloud, it's too hard to remember or say. "Start [sleep] meditation with nature sounds" reads as two parameters — ask for the second in a subsequent step.

**Make App Shortcuts discoverable in your app.** Show occasional tips when people perform the corresponding action manually.

**Provide enough detail for audio-only devices.** People get responses on AirPods and HomePod with no screen, so **all critical information must be in the full dialogue text**.

### Editorial

**Brief, memorable activation phrases plus natural variants.** Your app name must be included, but you can be creative — Keynote accepts both "Create a Keynote" and "Add a new presentation in Keynote".

**"App Shortcuts" and "Shortcuts" (the app): title case, always plural.** Individual shortcuts: **lowercase** — "Run a shortcut by asking Siri".

**iOS/iPadOS: order shortcuts by importance.** Your order sets the initial appearance in Spotlight and the Shortcuts app; the system then reprioritizes by usage.

## Snippets

**Ensure legibility** — sufficient contrast against the system background in both appearances, consistent margins.

**Keep content concise.** Snippets are for lightweight, quick interactions. **Maximum height 400 points**, and remember fonts draw at various sizes per the person's text size setting. For a result snippet needing more detail, **deep-link into your app** rather than expanding the view.

**Choose a descriptive label for a confirmation snippet's primary button.** "Order" is clearer than "OK" or "Proceed" for a coffee order. The system default is "Continue".

**Communicate the purpose visually.** Spoken dialogue is essential when nobody's looking at the screen, but **prefer omitting it from the visual representation** and letting the custom view carry the information.

## Status bars

**Obscure content under the status bar.** The background is transparent by default, so content shows through and can make status bar information hard to read. Worse, **visible controls behind it invite interaction that can't happen.** **Prefer a scroll edge effect** to place a blurred view behind it.

**Consider temporarily hiding it for full-screen media** — Photos hides the status bar and other chrome for full-screen browsing.

**Never permanently hide it.** Without it, people must leave your app to check the time or Wi-Fi. **Let a simple, discoverable gesture bring it back** — a single tap in Photos.

## React Native mapping

### The extension boundary

Widgets, Live Activities, Controls, App Shortcuts, and Snippets are **SwiftUI-only**. There is no JS authoring path, and this is a fixed constraint rather than a tooling gap. The realistic architecture:

```
ios/
├── MyApp/                    # RN app
├── MyAppWidget/              # WidgetKit extension — SwiftUI views
│   ├── MyAppWidget.swift
│   └── Provider.swift        # TimelineProvider
└── Shared/
    └── SharedData.swift      # reads the App Group container
```

Data flows from RN to the extension through an **App Group** shared container:

```js
// JS side — write the data the widget will render.
import { SharedGroupPreferences } from 'react-native-shared-group-preferences';
// or expo-shared-group-preferences / a small custom native module.

await SharedGroupPreferences.setItem(
  'widgetData',
  { high: 24, low: 12, condition: 'partly-cloudy', updatedAt: Date.now() },
  'group.com.example.myapp',
);

// Then ask WidgetKit to reload — otherwise the widget shows stale data
// until its next scheduled timeline entry.
import { reloadAllTimelines } from 'react-native-widgetkit';
reloadAllTimelines();
```

Note `updatedAt`: the HIG rule about showing when data was last updated matters more in RN, because your update opportunities depend on the app running.

For Expo, `expo-apple-targets` (a config plugin) generates the widget target and keeps it through prebuild — without it, `npx expo prebuild --clean` deletes hand-added Xcode targets, which is the most common way RN widget work gets lost.

Live Activities follow the same shape via `react-native-live-activity` or a custom module wrapping ActivityKit:

```js
// Start/update/end from JS; the views themselves are SwiftUI.
await LiveActivity.start({ orderId, eta, driverName });
await LiveActivity.update({ eta: newEta });
await LiveActivity.end();   // Always end it — an activity that outlives its
                            // event (or exceeds ~8 hours) is stale on the Lock Screen.
```

App Shortcuts / App Intents can be defined in Swift and call back into RN, but the intent handler itself must be native. `expo-apple-targets` plus a small `AppIntent` that posts a notification into the RN bridge is the workable pattern for intents that open the app; intents that complete *without* opening the app must be implemented entirely in Swift.

### Notifications

This part is fully reachable from JS:

```js
// Content per the HIG rules.
await Notifications.scheduleNotificationAsync({
  content: {
    title: 'Package delivered',              // useful, title case, no period
    body: 'Left at the front door.',         // sentence case, punctuated, not truncated
    // No app name or icon — the system supplies those.
    sound: 'delivery.wav',                   // short, distinctive; never load-bearing
    badge: unreadCount,                      // unread notifications only
    categoryIdentifier: 'delivery',          // carries the actions
    data: { deepLink: '/orders/123' },       // tapping goes to the right place
  },
  trigger: null,
});
```

Actions, with the "no action that merely opens the app" rule respected:

```js
await Notifications.setNotificationCategoryAsync('delivery', [
  { identifier: 'view-photo', buttonTitle: 'View Photo', options: { opensAppToForeground: true } },
  { identifier: 'report',     buttonTitle: 'Report Issue', options: { opensAppToForeground: true } },
  // NOT: { identifier: 'open', buttonTitle: 'Open App' } — tapping already does that.
]);
```

Hidden-preview placeholder text needs the native category setting; `expo-notifications` doesn't expose `hiddenPreviewsBodyPlaceholder`, so it requires a small native addition if you want it — worth doing, because without it people who hide previews get only "Notification".

Foreground handling, per the "handle gracefully" rule:

```js
Notifications.setNotificationHandler({
  handleNotification: async () => ({
    // Don't interrupt while the person is already looking at the app.
    shouldShowBanner: false,
    shouldPlaySound: false,
    shouldSetBadge: true,      // increment the badge instead
  }),
});
// …and insert the new data into the current view.
```

Badge hygiene:

```js
// Update as soon as the notification is opened; zero clears Notification Center.
await Notifications.setBadgeCountAsync(remainingUnread);
```

Never render a custom badge-looking view — people who disabled badges will see it and be confused.

### Status bars

```jsx
import { StatusBar } from 'expo-status-bar';

// Style follows appearance; translucent so content passes under it.
<StatusBar style="auto" />
```

Never hide it permanently:

```jsx
// Full-screen media only, with a tap to restore — the Photos pattern.
const [chrome, setChrome] = useState(true);
<StatusBar hidden={!chrome} animated />
<Pressable onPress={() => setChrome(c => !c)} style={StyleSheet.absoluteFill}>
  <PhotoViewer />
</Pressable>
```

And make sure content under it is legible — a native transparent header with `headerBlurEffect` gives you the scroll edge effect; a plain colored `<View>` behind the status bar does not:

```jsx
<Stack.Screen options={{ headerTransparent: true, headerBlurEffect: 'systemMaterial' }} />
```

Don't place interactive controls in the region behind the status bar — they'll look tappable and won't be.

## Review checklist

- [ ] Widget shows dynamic, purpose-related content, not a restyled app icon.
- [ ] Widget sizes offered only where each adds value; small content not stretched to fill large.
- [ ] Information density glanceable; graphics used where text would be too dense.
- [ ] Brand present through color/type, not a logo — small top-right logo only for multi-source content.
- [ ] Widget appearance not mirrored inside the app.
- [ ] Widget states "sign in" value where authentication unlocks functionality.
- [ ] Update cadence matches data change rate; last-updated time shown when checks outpace updates.
- [ ] Dates/times refreshed by the system, not by consuming update budget.
- [ ] Stale data shown rather than placeholders; updates animated ≤ 2 s.
- [ ] Widget taps deep-link to the relevant location, not the app root.
- [ ] Widget margins 16 pt (11 pt for tight groupings); smaller on Mac desktop and Lock Screen.
- [ ] Live Activity models an event with a real start and end, ≤ 8 hours, and is explicitly ended.
- [ ] Live Activity carries no ads and no sensitive information (redacted or summarized).
- [ ] Live Activity supports light and dark; logo without a container; no whole app icon.
- [ ] Live Activity text medium weight or heavier; corner radii concentric with the container.
- [ ] Content not drawn to the Dynamic Island edge; height adapts to available information.
- [ ] Lock Screen background contrast verified against reduced-luminance Always-On.
- [ ] One notification per event; no repeats for unanswered ones.
- [ ] Notification titles useful and title-cased; bodies sentence-cased and untruncated.
- [ ] Hidden-preview placeholder text provided.
- [ ] No app name or icon in notification content; no error messages sent as notifications.
- [ ] No sensitive information in notification content.
- [ ] Foreground arrivals handled inline (badge/insert), not shown as banners.
- [ ] Notification actions are title-case, iconed, non-destructive by default, and none merely opens the app.
- [ ] Badge counts unread notifications only, kept current, and never the sole channel; no fake badge components.
- [ ] Controls have distinct on/off symbols that carry meaning without a title, animated on state change.
- [ ] Controls redact sensitive title/value when locked and require authentication for security actions.
- [ ] Controls provide Action button hint text and placeholder title/value.
- [ ] App Shortcut phrases are brief, include the app name, and use at most one parameter with familiar values.
- [ ] Missing parameters prompt for clarification with a sensible default.
- [ ] Full dialogue text carries all critical information for audio-only devices.
- [ ] Snippets ≤ 400 pt tall, legible in both appearances, with a descriptive primary button label and purpose conveyed visually.
- [ ] Status bar never permanently hidden; hidden only for full-screen media with a tap to restore.
- [ ] Content behind the status bar obscured via a scroll edge effect; no interactive controls placed there.
