# tvOS (Apple TV)

Source: HIG › Getting started › [Designing for tvOS](https://developer.apple.com/design/human-interface-guidelines/designing-for-tvos), [Focus and selection](https://developer.apple.com/design/human-interface-guidelines/focus-and-selection), [Remotes](https://developer.apple.com/design/human-interface-guidelines/remotes), [Top Shelf](https://developer.apple.com/design/human-interface-guidelines/top-shelf), [Live-viewing apps](https://developer.apple.com/design/human-interface-guidelines/live-viewing-apps), plus the tvOS sections of every other page.

React Native support: **[react-native-tvos](https://github.com/react-native-tvos/react-native-tvos)**, a fork of RN with focus-engine integration. Real but narrower ecosystem than iOS.

## Contents

- [Device characteristics](#device-characteristics)
- [Best practices](#best-practices)
- [The focus system](#the-focus-system)
- [The remote](#the-remote)
- [Layout for 10-foot viewing](#layout-for-10-foot-viewing)
- [Top Shelf](#top-shelf)
- [Live-viewing apps](#live-viewing-apps)
- [Key metrics](#key-metrics)
- [React Native landing notes](#react-native-landing-notes)
- [Review checklist](#review-checklist)

## Device characteristics

**Display** — very large, high-resolution.

**Ergonomics** — people are **8 feet or more** away, stationary, but sometimes continuing to interact while moving around the room.

**Inputs** — a remote, a game controller, voice, and apps on their other devices.

**App interactions** — deep immersion in a single experience lasting hours, but also picture-in-picture to follow a second app or video.

## Best practices

- **Support the fluid, familiar Siri Remote gestures.**
- **Embrace the focus system**, letting it highlight and expand items as people move among them so they always know what to do and where they are.
- **Deliver edge-to-edge artwork, subtle fluid animation, and engaging audio** — cinematic, and **clear and legible from across the room**.
- **Make sign-in easy and infrequent**, handle shared sign-in, and switch profiles automatically when the viewer changes.

## The focus system

Focus is the whole interaction model on tvOS, and it drives layout, asset sizing, and spacing decisions that don't exist on other platforms.

**Rely on system-provided focus effects.** They're precisely tuned and give consistency and predictability. **Create custom focus effects only if absolutely necessary.**

**Every onscreen element must be focusable.** Unlike iPadOS/macOS, where full keyboard access reaches controls and you only handle focus for content elements, tvOS users reach *everything* by directional gestures — so buttons, sliders, and toggles all need focus support.

**Never indicate focus with color alone.** Subtle **scaling and responsive animation** are the primary signals. → [color-and-materials.md](../foundations/color-and-materials.md)

**Don't change focus without interaction.** People rely on focus to know where they are; moving it costs them time finding it again. The one exception: when someone is moving focus with a discrete directional input (remote, keyboard, game controller) and the focused item **disappears**, move focus to a nearby item — there are only a few within one step, so the indicator stays findable. When they're *not* using such an input, you can't predict their next target, so **hide the focus indicator** instead.

**Focus rings for text and search fields; highlights for lists and collections.** A whole-row highlight is easier to track than a ring. A ring can work for an item that fills a cell, like a photo.

### The five focus states

Each is visually distinct, and each needs design attention:

| State | Meaning |
|---|---|
| **Unfocused** | Less prominent than focused items |
| **Focused** | Stands out via elevation to the foreground, illumination, and animation |
| **Highlighted** | Instant visual feedback on being chosen — e.g. briefly inverting colors and animating |
| **Selected** | Chosen or activated — a heart button filled vs. empty |
| **Unavailable** | Can't be focused or chosen; appears inactive |

**Because focusing an item usually increases its scale, supply assets at the larger focused size** so they stay sharp, and **make sure the enlarged item doesn't crowd its neighbors.** This is the single most common tvOS layout bug.

**In a full-screen experience, let gestures affect the content, not focus.** A full-screen item shows no focus, so people naturally expect gestures to manipulate the object itself.

**Avoid displaying a pointer.** People expect to navigate a fixed number of items by changing focus, not by dragging a tiny pointer across a huge screen. Free-form movement can make sense during gameplay; menus and interface elements use the focus model. If you truly need a pointer, make it **highly visible** and integrated.

## The remote

**Prefer standard gestures for standard actions.** Outside active gameplay, people expect the remote to behave identically in every app. Redefining standard behaviors causes confusion.

**Move focus in the same direction as the gesture.**

**Provide clear feedback** for gestures — lightly resting a thumb shows people where to swipe to reveal an info area.

**Differentiate press from tap.** **Pressing is intentional** — use it for choosing a button, confirming, initiating gameplay actions. **Tapping is easily inadvertent** — people tap by resting a thumb, picking the remote up, moving it, or handing it over. Taps suit navigation or showing additional information, and it's often best to **ignore taps during live video playback**.

**Consider positional taps** — the remote distinguishes up, down, left, and right taps on the touch surface — but only where it's intuitive and discoverable.

### Button behavior

| Control | In an app | In a game |
|---|---|---|
| Touch surface (swipe) | Navigates; changes focus | Directional pad |
| Touch surface (press) | Activates a control or item; navigates deeper | Primary button |
| **Back** | Returns to previous screen; exits to the Apple TV Home Screen | Pauses/resumes; returns to previous screen or main menu |
| **Play/Pause** | Activates media playback; pauses/resumes | Secondary button; skips intro video |

**Back opens the *parent* of the current screen — not necessarily the previous screen.** At an app's top level the parent is the Apple TV Home Screen; within the app it's defined by your hierarchy. **The exception is active gameplay**, where repeated accidental Back presses are easy — respond by opening an in-game **pause menu**, and have Back close the menu and resume. Press-and-hold always goes Home.

**Respond correctly to Play/Pause during media playback** — play, pause, or resume.

**EPG buttons:** a "guide" or "browse" button opens your EPG; while browsing, "page up"/"page down" navigate the EPG — don't repurpose them there. While content plays, **page up/down change the channel**. If you have no EPG, the system routes those presses to the viewer's default guide app.

## Layout for 10-foot viewing

**Layouts do not adapt to screen size** — every TV gets the same interface. Design one layout that survives a wide size range. → [layout.md](../foundations/layout.md)

**Safe area: inset primary content 60 pt top and bottom, 80 pt left and right.** Content nearer the edge is hard to see and may be cropped by overscan on older TVs. Only deliberately-offscreen or partially-visible content belongs outside.

**Pad between focusable elements** to leave room for focus growth.

**Grids:** consistent spacing is what makes a grid readable. Titled rows need extra vertical space between the previous row and the title's center, and between the title and the row's items. Keep partially visible offscreen content **symmetric** on both sides.

**Type:** body is **29 pt**, minimum **23 pt**, and the default weight is **Medium** — thin text doesn't survive TV upscaling and distance. → [typography.md](../foundations/typography.md)

**Controls: 66 × 66 pt** default, **56 × 56 pt** minimum. → [accessibility.md](../foundations/accessibility.md)

**Color:** a limited palette coordinated with your logo, letting content dominate.

**Materials:** Liquid Glass appears through navigation and system surfaces; **image views and buttons adopt glass on focus**. Standard materials map to thickness: `ultraThin` for full-screen light-scheme views, `thin` for light-scheme overlays, `regular` for overlays, `thick` for dark-scheme overlays.

**Layered images** get parallax automatically with standard views and system focus APIs. **The background layer must be opaque.** Keep depth subtle, leave a safe zone around foreground layers (focus scaling can crop them), and preview in Xcode / Parallax Previewer and then on a real TV. → [icons-and-images.md](../foundations/icons-and-images.md)

**Video overlays:** small and unobtrusive; some displays suffer **image retention**, so keep overlays short and prefer translucent SDR over bright opaque. Interactive overlays need a **≥ 0.5 s delay** before pausing media, plus a clear dismiss path. → [patterns/media-and-haptics.md](../patterns/media-and-haptics.md)

**Tab bar** sits at the **top of the screen** — not the bottom. A bottom tab bar ported from iOS is wrong on tvOS, and it's one of the most visible signs an app wasn't designed for the platform. It **scrolls offscreen** when the tab holds a single main view, and stays **pinned** when the screen has a split view. Menu always returns focus to it. → [navigation-and-search.md](../components/navigation-and-search.md)

**Split views** rather than segmented controls for content filtering, with a **single title above** the whole split view.

**Lockups:** consistent sizes within a row, spacing that accounts for focus growth, images preferred over initials. → [layout-and-organization.md](../components/layout-and-organization.md)

**Text entry:** minimize it. Beyond a small amount, send people to a website on another device. Use **cross-device authentication** and shared sign-in with per-user profiles; for an email address, show the email keyboard screen with its recent-address list. → [patterns/data-entry-search-settings.md](../patterns/data-entry-search-settings.md)

**Search:** suggestions are essential — popular, context-specific, and recent searches — because nobody wants to type on a TV.

## Top Shelf

The banner shown when your app is in the top row of the Home Screen.

**Help people jump right into content.** The Carousel actions and Carousel details templates each include a primary button (begin playback) and a More Info button (open your app to details).

**Feature new content** — new releases and episodes, upcoming titles. **Avoid promoting content people already purchased, rented, or watched.**

**Personalize.** People put their most-used apps in Top Shelf, so show targeted recommendations, resume points, and ways to jump back into gameplay.

**Avoid advertisements and prices.** People already chose your app, so ads aren't welcome. Purchasable content is fine, but focus on what's new and exciting and **show prices only when people show interest**.

**Prefer dynamic, layered content** over static images.

**Supply at least one static fallback image.** The system shows it when your app is in the Dock and focused but full-screen content is unavailable. **tvOS flips and blurs it** to fit 1920 px wide at 16:9. **Avoid implying interactivity** in a static image — it isn't focusable.

**Carousel actions:** succinct title (show, movie, or album name), optional brief subtitle (a date range, an episode's show name).

**Carousel details:** a title identifying the currently playing content near the top, optionally with a short phrase or attribution above it ("Featured on *My App*").

**Sectioned content row:** enough images to span the full screen width, plus **at least one label** for consistency and context. Mixing image sizes causes **automatic scale-up to the tallest image** (a 16:9 image scales to 500 px high alongside a poster or square). **Three to eight images** — fewer than three doesn't feel like a scrolling banner, more than eight makes navigation hard. **This layout shows no labels under content**, so any text must be part of the image — put it on a dedicated layer in a layered image, and **add it to the image's accessibility label** so VoiceOver reads it.

## Live-viewing apps

**Feature live content prominently in the first tab** so it's one tap away, ideally with a Watch Now button over featured or recent content that disappears as playback begins full-screen.

**Make live content look live** — playing it is best, but also badge, symbol, or sash it, and group other channels in a "Live" row.

**Indicate progress of in-progress live content** so people know where they'll land.

**Provide additional actions in a consistent order** — Watch, Start Over, Record, Favorite — with playback always primary. Show other airtimes so people can schedule.

**Content footer for browsing during playback:** give it a subtle darkening for legibility, badge or tint the currently playing thumbnail, match its categories to your EPG, and make invoke/dismiss predictable (swipe up to invoke → swipe down to dismiss).

**Give instant visual feedback on channel change** — it confirms arrival and covers stream loading time.

**Match audio to context.** Audio continues while browsing with content playing behind, but **stops when people leave the live tab** — they've left the live-viewing context.

**EPG:** show current program, channel, and time prominently so people can return to the current channel instantly. Make paging, scrolling, and jumping easy; offer My Channels or Favorites. Group into familiar categories (Movies, TV Shows, Kids, Sports, Popular) matching the content footer. **Let people browse the EPG without leaving their content** — PiP or background playback.

## Key metrics

| Metric | Value |
|---|---|
| Safe area inset | **60 pt top/bottom, 80 pt left/right** |
| Default control size | 66 × 66 pt |
| Minimum control size | 56 × 56 pt |
| Default body text | 29 pt, **Medium** weight |
| Minimum text size | 23 pt |
| Viewing distance | 8+ feet |
| Scale factors | @1x and @2x |
| Top Shelf static image | fits 1920 px wide at 16:9 (flipped and blurred by the system) |
| Top Shelf sectioned row | 3–8 images |
| Interactive overlay delay | ≥ 0.5 s before pausing media |
| Focus indication | Scale + animation — **never color alone** |

## React Native landing notes

### Setup

```bash
# react-native-tvos is a fork — install it in place of react-native.
npx @react-native-community/cli init MyTVApp --template react-native-tvos@latest
```

Expo does not support tvOS. Many community packages lack tvOS support; check for `Platform.isTV` handling before adopting one.

### Focus is the central concern

```jsx
import { TVFocusGuideView, useTVEventHandler, Pressable } from 'react-native';

// Every interactive element must be focusable and must show focus by scale.
function Card({ item, onSelect }) {
  const [focused, setFocused] = useState(false);
  return (
    <Pressable
      onFocus={() => setFocused(true)}
      onBlur={() => setFocused(false)}
      onPress={onSelect}
      // Scale + elevation, not color — the tvOS focus convention.
      style={[
        s.card,
        focused && { transform: [{ scale: 1.1 }], shadowOpacity: 0.5, shadowRadius: 20 },
      ]}
    >
      {/* Asset supplied at the focused (1.1×) size so it stays sharp. */}
      <Image source={item.art} style={s.art} />
    </Pressable>
  );
}
```

Space for focus growth explicitly, since RN won't do it for you:

```js
const CARD_W = 300, FOCUS_SCALE = 1.1;
// Half the growth on each side, plus the visual gap you actually want.
const GAP = Math.ceil((CARD_W * (FOCUS_SCALE - 1)) / 2) + 24;
```

Without this, a focused card overlaps its neighbor's title — the most common tvOS-in-RN bug.

### Guiding focus

```jsx
// TVFocusGuideView redirects focus into a region that would otherwise be
// unreachable by directional navigation (e.g. a control off the main grid axis).
<TVFocusGuideView destinations={[playButtonRef.current]} style={{ height: 1 }} />

// Preferred initial focus when a screen appears.
<Pressable hasTVPreferredFocus={isInitialItem} />
```

Don't move focus programmatically outside the "focused item disappeared" case — it violates the guidance and disorients people.

### Safe area

```js
// RN does not apply the tvOS overscan insets. Hard-code them.
export const TV_SAFE = { top: 60, bottom: 60, left: 80, right: 80 };

<View style={{ paddingTop: TV_SAFE.top, paddingHorizontal: TV_SAFE.left, flex: 1 }} />
```

Backgrounds and artwork still go edge to edge — only primary content is inset.

### Remote events

```jsx
useTVEventHandler(evt => {
  switch (evt.eventType) {
    case 'up': case 'down': case 'left': case 'right':
      break;                            // let the focus engine handle navigation
    case 'select': onSelect(); break;
    case 'playPause': togglePlayback(); break;   // must work during media playback
    case 'menu':
      // Back = parent screen, not previous screen. In gameplay, open a pause menu.
      isPlaying ? openPauseMenu() : goToParent();
      break;
    case 'swipeUp': revealInfo(); break;
    // Ignore taps ('focus' events with no press) during live playback —
    // resting a thumb on the remote generates them.
  }
});
```

Note the `menu` handling: mapping Back to `navigation.goBack()` is wrong when the previous screen isn't the hierarchical parent.

### Navigation

The tab bar has to move to the top. `@react-navigation/bottom-tabs` puts it at the bottom, so shipping the iOS navigator untouched leaves the bar in the wrong place — the single most visible structural giveaway on tvOS.

```jsx
<Tab.Navigator
  screenOptions={{
    tabBarPosition: 'top',                       // React Navigation 7; confirm on your version
    tabBarStyle: { paddingTop: TV_SAFE.top, paddingHorizontal: TV_SAFE.left },
  }}
>
```

If your version doesn't support `tabBarPosition`, render your own focusable tab row above the content rather than restyling the bottom bar in place. Either way, `menu` must return focus to that row (see the remote handler above), and the row participates in the focus engine like anything else — five focus states, scale-based focus indication, 66 pt targets.

### Type and controls

```js
const tvText = {
  title1: { fontSize: 76, lineHeight: 96, fontWeight: '500' },
  title2: { fontSize: 57, lineHeight: 66, fontWeight: '500' },
  title3: { fontSize: 48, lineHeight: 56, fontWeight: '500' },
  headline:{ fontSize: 38, lineHeight: 46, fontWeight: '500' },
  body:   { fontSize: 29, lineHeight: 36, fontWeight: '500' },   // Medium, not Regular
  caption1:{ fontSize: 25, lineHeight: 32, fontWeight: '500' },
};

const TV_MIN_TARGET = 66;
```

Reusing the iOS type scale on tvOS produces text nobody can read from a sofa — this is the most visible porting failure.

### Video

```jsx
// react-native-video works on tvOS; use the native controls so remote
// gestures and scrubbing behave correctly.
<Video source={{ uri }} controls resizeMode="contain" />
```

### Top Shelf and layered images

Top Shelf requires a **TVTopShelf extension** in Swift — no RN path. Layered images (`.lcr`) are asset-catalog artifacts built with Parallax Exporter, also outside RN, though you can ship them as assets the native side references.

### What needs native work

- Top Shelf extension, layered/parallax images
- TV provider account integration
- SharePlay, TV app integration (`TVTopShelfItem`, `MPNowPlayingInfoCenter`)
- Custom focus effects beyond scale/opacity

## Review checklist

- [ ] Every interactive element is focusable and reachable by directional navigation.
- [ ] Focus indicated by scale and animation — never by color alone.
- [ ] Assets supplied at the focused (enlarged) size; no blurring on focus.
- [ ] Spacing accounts for focus growth; focused items never overlap neighbors' content.
- [ ] Focus not moved programmatically except when the focused item disappears.
- [ ] Focus indicator hidden when the focused object disappears under non-directional input.
- [ ] Focus rings on text/search fields, highlights on list and collection rows.
- [ ] All five focus states designed: unfocused, focused, highlighted, selected, unavailable.
- [ ] Full-screen experiences route gestures to content, not focus.
- [ ] No pointer in menus or interface navigation.
- [ ] Safe area 60 pt top/bottom, 80 pt left/right for primary content; backgrounds still edge to edge.
- [ ] tvOS type scale used (29 pt body, Medium weight, 23 pt minimum) — not the iOS scale.
- [ ] Control targets ≥ 56 × 56 pt, defaulting to 66 × 66 pt.
- [ ] Tab bar at the top of the screen — an iOS bottom tab bar was not carried over as-is.
- [ ] Back opens the hierarchical parent, not the previous screen; gameplay Back opens a pause menu.
- [ ] Play/Pause works during media playback.
- [ ] Taps ignored during live video playback; press and tap distinguished.
- [ ] EPG buttons behave per context (open guide, page through guide, change channel during playback).
- [ ] Grid spacing consistent; titled rows have extra vertical space; offscreen content symmetric.
- [ ] Video overlays small, short, translucent SDR; interactive overlays delay ≥ 0.5 s.
- [ ] Layered images have opaque backgrounds, subtle depth, and a foreground safe zone; previewed on a real TV.
- [ ] Text entry minimized; cross-device sign-in and shared accounts with per-user profiles supported.
- [ ] Search offers popular, contextual, and recent suggestions.
- [ ] Top Shelf features new content, no ads or prominent prices, dynamic where possible, with a static fallback that doesn't imply interactivity.
- [ ] Sectioned Top Shelf rows have 3–8 images, at least one label, and text baked into images also present in accessibility labels.
- [ ] Live content is visibly live, one tap from launch, with consistent action ordering and progress indication.
- [ ] Live audio stops when people leave the live tab.
