# Apple HIG for React Native

**English** · [简体中文](README.zh-CN.md)

An agent skill that gives Claude Apple's Human Interface Guidelines — all 172 pages of them — distilled into 33 reference files, each pairing Apple's rules with the React Native code that implements them.

Models are good at React Native and vague about Apple. Ask for a settings screen and you get something that works, renders, and quietly reads as Android: a fixed row height, a hard-coded `#F2F2F7`, a hand-rolled header, a Material chevron. Every one of those is a real number the HIG specifies and the model approximated.

This skill removes the approximating. **Never guess at a value** — hit targets, type scales, safe area insets, contrast ratios, spring behaviour. They're all in here, with the RN mapping next to them and a review checklist at the end of every file.

## Install

Clone or download this repo, then:

```bash
cp -r apple-hig ~/.claude/skills/
```

For a single project instead of your whole machine, copy it to `.claude/skills/` inside the project.

On Claude.ai or Claude Desktop, upload [`apple-hig.skill`](apple-hig.skill) under Settings → Capabilities → Skills.

Claude loads it on its own once it's installed — the description covers screens, navigation, sheets, typography, colour, dark mode, Liquid Glass, haptics, safe areas, Dynamic Type, VoiceOver, widgets, and App Store requirements, so you don't have to invoke it by name. Asking whether something "feels native" is enough.

## Why use it?

### The problem is precision, not taste

A model that hasn't read the HIG doesn't produce ugly UI — it produces UI that's off by a few points in fifteen places. iOS body text is 17 pt, not 16. Rows need `minHeight`, not `height`, or they truncate at large Dynamic Type sizes. tvOS body text is 29 pt **Medium** with a 23 pt floor, so an iOS type scale reused on Apple TV is unreadable from a couch. Liquid Glass can't be faked with `rgba()` because it adapts to luminosity and paints specular highlights, and a `backgroundColor` on a native tab bar overrides the material entirely.

None of that is a judgement call. It's all documented, and it's all in here.

### It measurably changes the answer

Five tasks from [`evals.json`](apple-hig/evals/evals.json), each answered twice by identical agents — one with the skill, one with no access to it and no web search. Same model, same prompt, same length budget. Scored against each eval's `expected_output`, decomposed into checkable points:

| Task | With skill | Baseline |
|---|---|---|
| Port an Android-designed settings screen to iOS | **9.0** / 9 | 5.0 / 9 |
| Ship an iOS media app on Apple TV | **9.0** / 9 | 5.0 / 9 |
| Present an edit-profile sheet with unsaved changes | **6.5** / 7 | 4.0 / 7 |
| Get the iOS 26 glass material on a tab bar | **6.0** / 8 | 5.0 / 8 |
| Review a component for App Store & a11y flags | **9.0** / 9 | 8.5 / 9 |
| **Total** | **39.5 / 42 · 94%** | 27.5 / 42 · 65% |

The gap is not uniform, and that's the useful part. It's widest on platform-specific knowledge that's thin in training data, and nearly closed on generic accessibility review — contrast ratios and missing labels are already well known. What the baseline got wrong were specifics: `height: 44` instead of `minHeight`, the system palette hard-coded as literals, an alert where an action sheet belongs, Expo claimed to support tvOS, no Reduce Transparency fallback.

One run each, scored non-blind — treat the ordering as the finding, not the exact number.

## Reference

Start at [`SKILL.md`](apple-hig/SKILL.md) — it holds the routing table, the critical metrics, and a universal review checklist. The files below are what it routes to.

**Foundations** — the cross-cutting rules

- [color-and-materials.md](apple-hig/references/foundations/color-and-materials.md) — semantic colours, dark mode, blur, Liquid Glass
- [typography.md](apple-hig/references/foundations/typography.md) — type scales, Dynamic Type, UI copy, empty states
- [layout.md](apple-hig/references/foundations/layout.md) — screen structure, safe areas, rotation, breakpoints, RTL
- [accessibility.md](apple-hig/references/foundations/accessibility.md) — VoiceOver, hit targets, contrast, Reduce Motion
- [motion.md](apple-hig/references/foundations/motion.md) — animation, transitions, gesture-driven interaction
- [icons-and-images.md](apple-hig/references/foundations/icons-and-images.md) — SF Symbols, asset export, app icons
- [privacy.md](apple-hig/references/foundations/privacy.md) — permissions, credentials, sign-in

**Patterns** — how flows should behave

- [modality-and-multitasking.md](apple-hig/references/patterns/modality-and-multitasking.md) — sheets, modals, backgrounding, audio interruption
- [feedback-and-loading.md](apple-hig/references/patterns/feedback-and-loading.md) — async state, errors, undo, destructive actions
- [launching-and-onboarding.md](apple-hig/references/patterns/launching-and-onboarding.md) — launch screens, first run, tips, state restoration
- [data-entry-search-settings.md](apple-hig/references/patterns/data-entry-search-settings.md) — forms, search, settings, account deletion
- [media-and-haptics.md](apple-hig/references/patterns/media-and-haptics.md) — audio, video, haptic feedback
- [notifications-sharing-files.md](apple-hig/references/patterns/notifications-sharing-files.md) — push, sharing, drag and drop, documents

**Components** — specific controls

- [menus-and-actions.md](apple-hig/references/components/menus-and-actions.md) — buttons, toolbars, menus, share sheets
- [navigation-and-search.md](apple-hig/references/components/navigation-and-search.md) — tab bars, sidebars, search fields
- [presentation.md](apple-hig/references/components/presentation.md) — sheets, alerts, action sheets, popovers
- [selection-and-input.md](apple-hig/references/components/selection-and-input.md) — text fields, toggles, pickers, sliders, keyboards
- [layout-and-organization.md](apple-hig/references/components/layout-and-organization.md) — lists, tables, grids, split views, disclosure
- [content-and-charts.md](apple-hig/references/components/content-and-charts.md) — charts, image views, text views, web views
- [status.md](apple-hig/references/components/status.md) — progress, gauges, activity rings, ratings
- [system-experiences.md](apple-hig/references/components/system-experiences.md) — widgets, Live Activities, Controls, App Shortcuts

**Platforms** — read the ones you ship to

- [ios.md](apple-hig/references/platforms/ios.md) — first-class RN support
- [ipados.md](apple-hig/references/platforms/ipados.md) — same target; layout, input, resizing
- [macos.md](apple-hig/references/platforms/macos.md) — `react-native-macos` or Mac Catalyst
- [tvos.md](apple-hig/references/platforms/tvos.md) — `react-native-tvos`
- [visionos.md](apple-hig/references/platforms/visionos.md) — `react-native-visionos`, windowed UI only
- [watchos.md](apple-hig/references/platforms/watchos.md) — no RN path; read for the rules that reach your iOS app

**React Native implementation**

- [design-tokens.md](apple-hig/references/rn/design-tokens.md) — copy-pasteable token files
- [liquid-glass.md](apple-hig/references/rn/liquid-glass.md) — glass surfaces, scroll edge effects
- [platform-strategy.md](apple-hig/references/rn/platform-strategy.md) — project structure, libraries, which platforms to target

**Inputs and technologies**

- [inputs.md](apple-hig/references/inputs.md) — gestures, keyboard shortcuts, hover, focus, Pencil, sensors
- [identity-and-commerce.md](apple-hig/references/technologies/identity-and-commerce.md) — Sign in with Apple, Apple Pay, IAP, Wallet
- [system-integration.md](apple-hig/references/technologies/system-integration.md) — Siri and App Intents, Maps, SharePlay, iCloud, App Clips

## The numbers you'll look up most

| Platform | Hit target | Body text | Minimum text | Dynamic Type |
|---|---|---|---|---|
| iOS, iPadOS | **44 × 44 pt** | 17 pt | 11 pt | to ~3.1× |
| macOS | 28 × 28 pt | 13 pt | 10 pt | none |
| tvOS | 66 × 66 pt | 29 pt **Medium** | 23 pt | yes |
| visionOS | **60 × 60 pt** | 17 pt | 12 pt | yes |
| watchOS | 44 × 44 pt | 16 pt | 12 pt | yes, + AX1–AX3 |

Contrast: **4.5:1** for text up to 17 pt, **3:1** at 18 pt+ or bold, **7:1** target for custom small text — verified in both appearances.

tvOS safe area: **60 pt** top and bottom, **80 pt** left and right. React Native does not apply these for you.

## Source and currency

Distilled from all 172 pages of the [Human Interface Guidelines](https://developer.apple.com/design/human-interface-guidelines), current as of the site's December 2025 revisions — which covers Liquid Glass and the iOS 26-era guidance.

Two caveats the skill states about its own content: specific colour values and spring constants drift, so hex values are design-time reference and should be resolved at runtime through `PlatformColor`; and RN library recommendations age faster than the HIG does, though the principle of preferring packages that wrap real UIKit components over JS reimplementations will outlast any package name.

## Contributing

The [evals](apple-hig/evals/evals.json) are the fastest way to check whether a change helps. Add a task with an `expected_output`, run it with and without the skill, and see whether the reference file you edited actually gets read and applied — the A/B above surfaced two content gaps that way, including a tvOS tab bar detail the references never stated.
