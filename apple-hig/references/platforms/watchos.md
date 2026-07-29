# watchOS (Apple Watch)

Source: HIG › Getting started › [Designing for watchOS](https://developer.apple.com/design/human-interface-guidelines/designing-for-watchos), [Digital Crown](https://developer.apple.com/design/human-interface-guidelines/digital-crown), [Complications](https://developer.apple.com/design/human-interface-guidelines/complications), [Watch faces](https://developer.apple.com/design/human-interface-guidelines/watch-faces), [Always On](https://developer.apple.com/design/human-interface-guidelines/always-on), [Workouts](https://developer.apple.com/design/human-interface-guidelines/workouts), plus the watchOS sections of every other page.

## React Native support: none

**There is no React Native runtime for watchOS**, and no supported path to one. WatchKit/SwiftUI is the only way to build a watchOS app.

This page is included because your product may still need a watch component, and because several watchOS rules affect the iOS app you *do* build in RN:

- **Notifications** you send from your RN app appear on the watch and are governed by watchOS notification behavior (short titles, per-notification Mute/Turn off Time Sensitive options).
- **A watch app is a separate Swift target** in the same Xcode project. `expo-apple-targets` can scaffold it, and it communicates with your RN iOS app through an **App Group** container or `WCSession` — the same shape as widgets. → [components/system-experiences.md](../components/system-experiences.md)
- **HealthKit, Activity rings, and workout data** surfaced in your RN app follow the rules below, particularly the Activity ring restrictions. → [components/status.md](../components/status.md)

If you need a watch app, plan a Swift target from the start rather than looking for a JS route.

## Contents

- [Device characteristics](#device-characteristics)
- [Best practices](#best-practices)
- [The Digital Crown](#the-digital-crown)
- [Layout and type](#layout-and-type)
- [Always On](#always-on)
- [Complications and watch faces](#complications-and-watch-faces)
- [Workouts](#workouts)
- [The watchOS rules that matter most](#the-watchos-rules-that-matter-most)
- [Key metrics](#key-metrics)
- [Review checklist](#review-checklist)

## Device characteristics

**Display** — small, on the wrist, high-resolution and easy to read.

**Ergonomics** — **no more than a foot away**, viewed by raising the wrist and operated with the opposite hand. The **Always On display** shows information on the watch face when the wrist drops.

**Inputs** — the **Digital Crown** for vertical navigation and data inspection (consistent across the watch face, Home Screen, and apps), standard gestures usable while in motion, the **Action button** for an essential action without looking, and shortcuts. Plus device data: GPS, blood oxygen and heart sensors, altimeter, accelerometer, gyroscope.

**App interactions** — glanced at many times a day, with interactions **often under a minute**. Critically: **people frequently use a watch app's *related* experiences — complications, notifications, Siri — more than the app itself.** That reorders your priorities.

## Best practices

- **Support quick, glanceable, single-screen interactions** delivering critical information succinctly, with targeted actions in a gesture or two.
- **Minimize hierarchy depth**, using the Digital Crown for vertical navigation.
- **Personalize** by anticipating needs and using on-device data for content relevant now or very soon.
- **Use complications** for relevant, dynamic data on the watch face, tappable straight into your app.
- **Use notifications** for timely, high-value information with actions that avoid opening the app.
- **Use background color to convey supporting information** and **materials to show hierarchy and place**.
- **Design the app to function independently**, complementing notifications and complications with additional detail.

## The Digital Crown

**Anchor navigation to the Digital Crown.** Since watchOS 10, turning it is the main way people navigate within and between apps. List, tab, and scroll views are **vertically oriented** so the Crown moves between important elements. **Always back Crown interactions with corresponding touchscreen interactions.**

**Use it to inspect data** where navigation isn't needed — World Clock advances the time of day at a selected location so people can compare it to their own.

**Always provide visual feedback.** Without it, people assume the Crown does nothing in your app.

**Match your interface update rate to turn speed.** People expect precise control. Don't update so fast that selecting a value becomes difficult.

**Use the default haptic detents** where they make sense. Turn them off if they don't match your animation. For tables with **significantly different row heights**, linear detents give a more consistent feel than row-based ones.

## Layout and type

**Extend content edge to edge.** The physical bezel provides visual padding, so internal padding wastes scarce space. Minimize padding between elements. → [layout.md](../foundations/layout.md)

**No more than two or three controls side by side** — at most three glyph buttons or two text buttons per row. Text buttons generally want **full width**; two short-labeled buttons side by side work if the screen doesn't scroll.

**Use a toolbar for corner buttons.** The system moves the time and title to accommodate them and applies **Liquid Glass** for distinction from content.

**Support autorotation** for content people might show someone — an image, a QR code — instead of sleeping on a wrist turn.

**Prefer vertical scrolling**, driven by the Crown. Use **tab views for page-by-page** content: stacked vertically, the Crown moves between pages and the page indicator **expands into a scroll indicator** when a page exceeds the screen height. Prefer **one screen height per page** for glanceability; use variable-height pages sparingly and place them **after** fixed-height ones. → [components/presentation.md](../components/presentation.md)

**Type:** system font is **SF Compact** (SF Compact Rounded in complications). Body is **16 pt**, minimum **12 pt**. Dynamic Type supports xS→xxxL plus **AX1–AX3**, and the **default size category depends on case size**: Small = 38mm, Large = 40/41/42mm, xLarge = 44/45/49mm. Leadings use half-points. Support **at least 140%** text enlargement. → [typography.md](../foundations/typography.md)

**Color:** always dark; **no Dark Mode setting**. Use background color to **communicate**, not decorate — Activity's infographic views use backgrounds matching each ring's color. **Avoid full-screen background color on long-lived screens** like workouts or audio players. → [color-and-materials.md](../foundations/color-and-materials.md)

**Materials:** keep the default material backgrounds on modal sheets — full-screen modals are common on watchOS, and the material contrast is what orients people. → [color-and-materials.md](../foundations/color-and-materials.md)

**Feedback:** **avoid indeterminate progress indicators.** An animated indicator implies people should keep watching the screen, which is the wrong model for a watch — instead, **promise a notification** when the process completes. A loading indicator still beats a blank screen for one-to-two-second waits. → [patterns/feedback-and-loading.md](../patterns/feedback-and-loading.md)

**Haptics** have a named vocabulary with specific meanings — Notification, Up, Down, Success, Failure, Retry, Start, Stop, Click. Use them per meaning; overusing Click diminishes it, and overlapping clicks confuse. → [patterns/media-and-haptics.md](../patterns/media-and-haptics.md)

**Media encoding:** audio **64 kbps HE-AAC**; video **H.264 High Profile, 160 kbps at up to 30 fps**, 208×260 px full-screen portrait or 320×180 px 16:9. **Keep clips under 30 seconds** — long clips cost disk space and require holding the wrist up, which tires people. Don't scale clips. Poster images should represent the content and **not look like a system control**.

**Images:** autoscaling PDFs cover all sizes — design for 40mm/42mm at @2x and WatchKit scales: 38mm 90%, 40mm 100%, 41mm 106%, 42mm 100%, 44mm 110%, 45mm 119%, 49mm 119%.

**Motion:** all layout- and appearance-based animations include **built-in easing you can't disable or customize** — don't design timing that assumes a linear curve. → [motion.md](../foundations/motion.md)

**Action sheets:** at most **four buttons including Cancel**, with Cancel in the **upper-left**. → [components/presentation.md](../components/presentation.md)

**Sheets:** only when the modal task needs a custom title or custom content presentation; otherwise use an alert or action sheet. Brief and occasional, never for navigation. If you change the dismiss label, prefer an SF Symbol — a label that looks like a page or app title leaves people unable to find the way out.

## Always On

The display stays on when the wrist drops, which changes both privacy and motion requirements.

**Hide sensitive information.** Redact anything people wouldn't want casual observers to see — bank balances, health data — including personal information that might appear in a notification.

**Keep other personal information glanceable where it makes sense** — pace and heart rate during a workout are appreciated. People who want nothing visible can turn Always On off.

**Keep important content legible and dim nonessential content.** Increase dimming on secondary text, images, and color fills. A to-do app might remove row backgrounds and dim item details to highlight titles. Consider removing rich images and large color areas in favor of dimmed colors.

**Maintain a consistent layout.** Avoid distracting changes when Always On begins or ends. **Transition an interactive component to an unavailable appearance rather than removing it.** Make infrequent, subtle updates — a sports app might pause play-by-play and update only the score.

**Transition motion gracefully to rest; never stop it instantly.** Finishing the current motion smoothly communicates the transition instead of looking like a failure.

## Complications and watch faces

Complications matter more than the app for many users, so treat them as a primary surface.

**Identify essential, dynamic content viewable at a glance.** People value **current information** over a launch shortcut. A static complication with no meaningful data won't keep a prominent spot.

**Support all complication families where possible** so you appear on more watch faces. Where you can't show useful information for a family, provide an image representing your app so people can still launch it.

**Consider multiple complications per family**, each **deep-linking to a different, most-relevant area**. A triathlon app could offer three circular complications — swim, bike, run — plus a **shareable watch face** preconfigured with all three and its custom images and colors, so people need no setup.

**Keep privacy in mind** — the Always-On display makes watch face information visible to others.

**Choose update times carefully.** You supply a timeline of entries with display times, and both the number of daily timeline updates and the number of stored entries are **limited**. Different data needs different timing: a meeting app shows information an hour before; a weather app shows forecast information at the time those conditions occur.

**Make images work in tinted mode.** The system applies a solid color to text, gauges, and images and **desaturates full-color images unless you provide tinted versions**. Many people **prefer** tinted mode, so this isn't an edge case.

**Use line widths of 2 pt or greater** — thinner lines are hard to see at a glance, especially in motion.

**Provide static placeholder images** for every complication family. The system uses them before your app can generate localized placeholders, and in the complication selection carousel. Note that **placeholder sizes may differ from actual image sizes** for the same complication.

## Workouts

**Show the data people care about** during an active session — elapsed or remaining time, calories, distance — plus relevant controls like lap or interval markers. watchOS keeps displaying your app between wrist raises during a session.

**Don't distract from the workout.** People don't need your workout list or other app areas mid-session.

**Use a distinct visual appearance for an active workout** so people recognize it at a glance — real-time updating metrics plus a unique layout.

**Make workout controls easy to find and tap**, with clear feedback when a session starts and stops.

**Explain unavailable sensor data.** Water can prevent heart-rate measurement, but you can still record distance and calories. For *Swimming* or *Other* workout types, explain it in language similar to the system Workout app.

**Provide an end-of-session summary** confirming completion and showing recorded information. Consider including Activity rings so people can check progress — following the Activity ring rules in [components/status.md](../components/status.md).

**Discard extremely brief sessions** — if one ends seconds after starting, discard automatically or ask whether to record it.

**Make text legible in motion** — large font sizes, high-contrast colors, most important information easiest to read.

## The watchOS rules that matter most

1. **Complications and notifications are often the primary surface** — design them first, not last.
2. **The Digital Crown drives vertical navigation**, always with a touch equivalent and always with visual feedback.
3. **Everything is glanceable** — single screens, minimal hierarchy, under a minute.
4. **Edge-to-edge content**, minimal padding, ≤ 3 glyph or ≤ 2 text buttons per row.
5. **Always On changes the rules**: redact sensitive data, dim secondary content, keep layout stable, no abrupt motion stops.
6. **No indeterminate progress indicators** — promise a notification instead.
7. **Always dark**, with background color used to communicate, not decorate, and not full-screen on long-lived views.
8. **Tinted mode is a first-class appearance** for complications, not a fallback.

## Key metrics

| Metric | Value |
|---|---|
| Default control size | 44 × 44 pt |
| Minimum control size | 28 × 28 pt |
| Default body text | 16 pt |
| Minimum text size | 12 pt |
| Text enlargement support | ≥ 140% |
| System font | SF Compact (SF Compact Rounded in complications) |
| Default size category | Small = 38mm, Large = 40/41/42mm, xLarge = 44/45/49mm |
| Controls per row | ≤ 3 glyph buttons, ≤ 2 text buttons |
| Action sheet buttons | ≤ 4 including Cancel (Cancel upper-left) |
| Complication line width | ≥ 2 pt |
| Video clip length | ≤ 30 s |
| Audio encoding | 64 kbps HE-AAC |
| Video encoding | H.264 High Profile, 160 kbps, ≤ 30 fps |
| Full-screen video | 208 × 260 px portrait |
| 16:9 video | 320 × 180 px |
| Scale factor | @2x |
| Dark Mode | N/A — always dark |
| Dynamic Type | xS–xxxL + AX1–AX3 |

Screen sizes in pixels: 38mm 272×340 · 40mm 324×394 · 41mm 352×430 · 42mm 374×446 · 44mm 368×448 · 45mm 396×484 · 46mm 416×496 · 49mm (Ultra 1–2) 410×502 · 49mm (Ultra 3) 422×514.

## Review checklist

For the watch app (Swift):

- [ ] Complications designed as a primary surface, showing dynamic current data — not just a launcher.
- [ ] All complication families supported, or an app-representing image supplied where data doesn't fit.
- [ ] Each complication deep-links to a distinct, relevant area.
- [ ] Tinted-mode versions of full-color complication images provided.
- [ ] Static placeholder images provided per family, at the correct placeholder sizes.
- [ ] Complication line widths ≥ 2 pt.
- [ ] Timeline update times chosen deliberately within the daily update and entry limits.
- [ ] Digital Crown drives vertical navigation, with touch equivalents and visible feedback.
- [ ] Update rate matches Crown turn speed; haptic detents suit the content (linear for uneven row heights).
- [ ] Content extends edge to edge with minimal internal padding.
- [ ] ≤ 3 glyph buttons or ≤ 2 text buttons per row; text buttons full-width where sensible.
- [ ] Corner buttons in a toolbar.
- [ ] Pages limited to one screen height where possible; variable-height pages placed last.
- [ ] SF Compact used; text ≥ 12 pt; layout survives 140% enlargement and AX3.
- [ ] Background color communicates something; no full-screen color on workouts or audio players.
- [ ] Default material backgrounds retained on modal sheets.
- [ ] No indeterminate progress indicators; completion notification promised instead.
- [ ] Haptics used per their documented meanings; Click not overused.
- [ ] Video clips ≤ 30 s at the recommended encodings, unscaled, with content-representative poster images.
- [ ] Animation timing doesn't assume a linear curve.
- [ ] Action sheets ≤ 4 buttons with Cancel upper-left; sheets used only for custom titles/content.
- [ ] Always On: sensitive data redacted, secondary content dimmed, layout stable, components made unavailable rather than removed, motion transitioned to rest gracefully.
- [ ] Workouts show relevant metrics with a distinct active appearance and easy controls.
- [ ] Unavailable sensor data explained; brief sessions discarded or confirmed; end-of-session summary provided.
- [ ] Activity rings used only per their documented rules.

For the RN iOS app that ships alongside it:

- [ ] Notification titles short enough for the watch; content free of sensitive information.
- [ ] Notification actions useful without opening the app.
- [ ] Watch target scaffolded as a Swift target with App Group / `WCSession` data sharing, and preserved across `expo prebuild`.
- [ ] Any HealthKit or Activity data surfaced in the RN app follows the Activity ring restrictions.
