# visionOS (Apple Vision Pro)

Source: HIG › Getting started › [Designing for visionOS](https://developer.apple.com/design/human-interface-guidelines/designing-for-visionos), [Spatial layout](https://developer.apple.com/design/human-interface-guidelines/spatial-layout), [Eyes](https://developer.apple.com/design/human-interface-guidelines/eyes), [Immersive experiences](https://developer.apple.com/design/human-interface-guidelines/immersive-experiences), [Ornaments](https://developer.apple.com/design/human-interface-guidelines/ornaments), [Windows](https://developer.apple.com/design/human-interface-guidelines/windows), plus the visionOS sections of every other page.

React Native support: **[react-native-visionos](https://github.com/callstack/react-native-visionos)** (Callstack). Runs RN apps as visionOS windows in the Shared Space. Volumes, immersive spaces, RealityKit, and ARKit require native SwiftUI/RealityKit work.

**Safety note from Apple:** Apple Vision Pro shouldn't be used while operating a vehicle or heavy machinery, or while moving around unsafe environments (balconies, streets, stairs). It's designed to be fit and used only by people **13 years of age or older**. Design accordingly — don't build experiences that encourage movement through unsafe space.

## Contents

- [Device characteristics](#device-characteristics)
- [Best practices](#best-practices)
- [Comfort is the primary constraint](#comfort-is-the-primary-constraint)
- [Eyes and hands](#eyes-and-hands)
- [Spatial layout](#spatial-layout)
- [Windows and volumes](#windows-and-volumes)
- [Immersion](#immersion)
- [The visionOS rules that matter most](#the-visionos-rules-that-matter-most)
- [Key metrics](#key-metrics)
- [React Native landing notes](#react-native-landing-notes)
- [Review checklist](#review-checklist)

## Device characteristics

**Space** — a limitless canvas for windows, volumes, and 3D objects, plus deeply immersive experiences.

**Immersion** — apps launch in the **Shared Space** where multiple apps run side by side and people open, close, and relocate windows. People can transition an app to a **Full Space** where it's the only app running, blending 3D content with their surroundings, opening a portal, or entering a different world.

**Passthrough** — live video from the external cameras, letting people interact with virtual content while seeing their actual surroundings. People control the amount with the **Digital Crown**.

**Spatial Audio** — the device models the sonic characteristics of the person's surroundings so audio sounds natural. With permission to access surroundings information, you can fine-tune it further.

**Eyes and hands** — most actions are performed by **looking** at an object and making an **indirect** gesture like a tap. **Direct** gestures (touching with a finger) are also available.

**Ergonomics** — people rely **entirely on the device's cameras for everything they see**, real and virtual, so **visual comfort is paramount**. The system places content relative to the wearer's head regardless of height or posture, and **brings content to people rather than making them move to it**.

**Accessibility** — VoiceOver, Switch Control, Dwell Control, Guided Access, Head Pointer, and more.

## Best practices

- **Embrace space, Spatial Audio, and immersion**, integrating passthrough and spatial input from eyes and hands.
- **Find the *minimum* level of immersion that suits each moment.** Don't assume everything needs to be fully immersive.
- **Use windows for contained, UI-centric experiences** with familiar controls.
- **Prioritize comfort** — see below.
- **Support shared activities** via SharePlay so people see each other's spatial Personas.

## Comfort is the primary constraint

Every layout and motion decision on visionOS is a comfort decision. The specific requirements:

- **Display content within the field of view, positioned relative to the head.** Never require people to turn their head or change position to interact.
- **Avoid overwhelming, jarring, or too-fast motion, and always provide a stationary frame of reference.** → [motion.md](../foundations/motion.md)
- **Support gestures people can make with hands resting in their lap or at their sides.**
- **If you support direct gestures, keep the content nearby and the interaction brief.**
- **Don't encourage movement in a fully immersive experience.**

**Don't anchor content to the wearer's head.** It makes people feel stuck and confined, obscures passthrough, and reduces the apparent stability of their surroundings — *and* it blocks assistive technologies like Pointer Control. Anchor content in their **space** instead, so they can look around naturally.

**Place content at least one meter away** for anything people read or engage with over time. Don't place content very close unless the interaction is brief.

**Avoid requiring specific body movements or positions.** Not everyone can, due to disability, spatial constraints, or environment. Offer alternative inputs.

**Let people use your app with minimal or no physical movement** unless movement is essential.

## Eyes and hands

### Eye-driven targeting changes layout rules

**Provide generous space around interactive components.** Eyes make small quick adjustments even while fixating, so crowded elements are hard to target individually. The concrete numbers:

- **Button centers at least 60 pt apart**
- **At least 16 pt of margin** around each item's bounds
- **Buttons 60 pt or larger need 4 pt of padding** so the hover effect doesn't overlap
- **Never let controls overlap other interactive elements or views**

**Prefer rounded shapes.** Eyes are drawn toward corners, making it hard to keep looking at a shape's center. The more rounded, the easier to target. **Prefer circular or capsule buttons**, and a capsule when a button stands alone. For stacks and rows: **rounded rectangle in a vertical stack, capsule in a horizontal row.** Avoid small or mini buttons in a stack or row.

**Give a multi-element interactive component one containing shape** that visionOS can highlight. An image with a label below acting as one control needs a custom region encompassing both, or only one element highlights.

**Minimize visual distraction, especially movement.** People automatically look toward motion — revealing content near a button someone is looking at makes them involuntarily look away from the button.

**Avoid repeating patterns or textures filling the field of view.** Eyes can lock onto different elements of a pattern, making them appear at different depths. Use the pattern in a smaller area.

**Avoid requiring multiple quick eye adjustments** over a large area or through multiple depth levels.

### Hover effects

**Prefer standard UI components** — they respond consistently to being looked at. Custom components with different visual feedback are hard to learn and remember.

**Use custom hover effects to emphasize a special moment, not everywhere.** Too many dilute the design, distract, and can cause visual discomfort. Note that **visionOS buttons don't support custom hover effects.**

**Keep one or more primary views unchanged across both hover states** — that visual stability lets people follow the transition. If everything moves, it disorients.

**Choose the right delay** — instant, short, or slightly longer, depending on expected interaction.

**Test hover effects while wearing the device.** It's the only way to know whether they feel alive rather than distracting.

### Gestures

**Indirect** gestures: look to target, manipulate from a distance. Comfortable at any distance, minimal movement, quick focus changes. **Prioritize these** for UI and common components.

**Direct** gestures: physically touching the object. Best for nearby objects inviting close inspection, used briefly — keeping arms raised is tiring.

Standard direct gestures: touch (select), touch and hold (context menu), touch and drag (move), double touch (preview / select a word), swipe (reveal actions, dismiss, scroll), two-handed pinch and drag apart/together (zoom), two-handed pinch and drag in a circle (rotate).

**Support standard gestures everywhere.** Tap is the first thing people try after looking at something.

**Custom gestures require a Full Space and permission to access hand information.** For them: **prioritize comfort** (test ergonomics continually — raised arms tire quickly, repeated similar movements stress joints), **be careful with multi-finger or two-handed gestures** (people may not have both hands free — offer a lower-movement alternative), and **never require a specific hand** (it increases cognitive load and excludes people with strong hand dominance or limb differences).

**Reserve the area around a person's hand for system overlays.** In visionOS 2+, looking at your palm opens Home and Control Center overlays — systemwide and reserved. **Don't anchor content to hands or wrists**; if a game must, place it **outside the immediate hand area** to avoid colliding with the Home indicator. In a Full Space you can **defer** the overlay so it requires a tap instead, for games with virtual hands.

**When VoiceOver is on, apps with custom gestures don't receive hand input by default**, so people can explore by voice. People can opt into Direct Gesture mode. An app that depends on custom hand gestures is therefore inert under default VoiceOver — which is why non-gesture alternatives are mandatory. → [accessibility.md](../foundations/accessibility.md)

## Spatial layout

**Center important content in the field of view.** visionOS launches an app directly in front of people. In an immersive experience, keep important content centered and avoid distracting motion or bright high-contrast objects in the periphery.

**Provide accurate depth cues.** Missing or conflicting cues cause visual discomfort.

**Use depth to communicate hierarchy** — an object standing out from surrounding content is more noticeable, and people notice depth *changes* (a sheet appearing makes the window recede along z).

**Avoid adding depth to text.** Hovering text is hard to read, slows people down, and can cause discomfort. → [typography.md](../foundations/typography.md)

**Make depth add value.** Depth suits large important elements — a tab bar or toolbar standing out from a window. It works poorly on small objects: depth on a button's symbol makes the button *less* legible. And **review how often you change depth** — refocusing for each depth difference is tiring when it happens often or quickly.

**Scale:** the system scales windows dynamically as they move along z, keeping content legible near or far. **Fixed scale** suits a virtual object that must look exactly like a physical one (a life-size product) — use it **sparingly and only for non-interactive objects**, because interactive content needs to scale to stay usable.

**Avoid displaying too many windows.** They obscure surroundings, make people feel overwhelmed and constricted, and make relocating the app cumbersome.

**The Digital Crown recenters windows** — your app needs to do nothing to support it.

**Use the floor to place a large immersive experience.** Content extending up from the floor should be placed on a flat horizontal plane aligned with the floor, so it blends with the surroundings.

## Windows and volumes

**Prefer a window for familiar interfaces and tasks**, reserving immersion for meaningful content. For bounded 3D content like a game board, use a **volume**.

**Retain the glass background.** It adapts to lighting and uses specular reflections and shadows to communicate the window's scale and position. Removing it makes UI and text less legible and less related to each other; an opaque background obscures surroundings and feels constricting and heavy.

**Default window size is 1280 × 720 pt**, placed about **two meters** in front of the wearer, giving an apparent width of about **three meters**. **Choose an initial size that minimizes empty area** — too much empty space looks unnecessarily large and obscures other content.

**Match the initial shape to the content** — Keynote opens wide because slides are wide; Safari opens tall because webpages are.

**Set a minimum and maximum size** for every window. Without them, people can make a window so small that elements overlap or so large it's unusable.

**Minimize the depth of 3D content in a window.** The system clips content extending too far from the surface; for greater depth, use a volume.

**Keep content within window bounds.** System controls sit just outside — Share above, resize/move/close below. Content encroaching there makes them hard to use.

**Use an ornament** for controls that belong with the window but not inside it. Toolbars and tab bars **already appear as ornaments** — don't recreate them. Keep an ornament's width ≤ the window's, prefer borderless buttons on its glass background, and keep the number of ornaments low. → [menus-and-actions.md](../components/menus-and-actions.md)

**Volumes:** for rich 3D content. Place 2D content so it reads from multiple angles (use an **attachment** to pin 2D content to specific parts of 3D content). **Use dynamic scaling in general**; fixed scaling (the default) for real-world-object representation. The **baseplate glow** helps people find the volume's edges and its resize control — you may not want it if your content is full-bleed or you supply a custom baseplate. A volume can carry **one additional ornament** beyond a toolbar and tab bar, anchored (`topBack`, `bottomFront`) so it stays put relative to the viewer as they move around — avoid putting it on the same edge as a toolbar or tab bar. **Choose an alignment** matching interaction: parallel to the floor for low-interaction content, tilting to match the person's gaze for content that must stay usable while reclining.

## Immersion

**Prefer launching in the Shared Space or the `mixed` immersion style.** It lets people reference your app alongside others and switch seamlessly. Even for a fully immersive app, launching into a Shared Space window gives context while loading and gives you somewhere to put the control that enters immersion.

**Reserve immersion for meaningful moments.** Photos browses albums in a Shared Space window and transitions to a Full Space only to examine a single photo.

**Draw attention with subtle cues first** — dimming, tinting, motion, scale, Spatial Audio — strengthening them only with good reason.

**Prefer subtle passthrough tints.** Bright or dramatic tints distract and diminish immersion.

**Choose the immersion style that supports the movements people will make.** Minor movement is fine — shifting weight, turning, sitting to standing — but excessive movement can cause the system to interrupt. **Avoid `progressive` or `full`, or transition back to `mixed`, if people might move beyond the 1.5-meter boundary.**

**Don't encourage movement in progressive or fully immersive experiences.** Let people bring an object closer rather than expecting them to move to it.

**With `mixed`, don't obscure passthrough too much.** People use it to understand and navigate their surroundings. If your virtual objects would substantially block their view, use `full` or `progressive` instead.

**Design smooth, predictable transitions** between immersion levels — gentle enough to visually track, never sudden or jarring.

**Let people choose when to enter or exit immersion.** Provide a clear action for both; Keynote's fully immersive Rehearsal has a prominent Exit button. **Don't make people use system controls to reduce immersion.**

**Make an exit control's purpose clear** — returning to a less immersive context vs. quitting entirely. If exiting quits, offer a way to pause or save first.

**Virtual hands:** match the viewer's positions and gestures and familiar characteristics. **Be careful making them larger than human hands** — they block content, feel clumsy, and appear out of proportion (too close to the face). **On a hand-tracking interruption, fade the virtual hands out and reveal the real ones**, then fade back in when data returns. Never let them freeze.

**Environments:** minimize distracting content — avoid lots of movement or high-contrast detail when the primary task is something like watching video. To draw attention to an area, use the highest-quality textures and shapes there and lower-quality assets plus dimming elsewhere.

**Adopt ARKit** for blending custom content with surroundings or using hand positions — and request permission, since that's sensitive data. → [privacy.md](../foundations/privacy.md)

## The visionOS rules that matter most

**Color and materials:** no Dark Mode (system colors use dark values); windows use the non-customizable **glass** material; prefer translucency over opacity; use color **sparingly** and prefer it in **bold text and large areas**; keep brightness balanced in full immersion. Three vibrancy levels: `label`, `secondaryLabel`, `tertiaryLabel` (inactive only). → [color-and-materials.md](../foundations/color-and-materials.md)

**Typography:** SF Pro; bolder Body and Title styles than iOS, plus **Extra Large Title 1/2**; **prefer 2D text**; default to **white**; **bold backgroundless text** rather than adding shadows (there may be no surface to cast onto and you can't predict the environment); **billboard** spatially anchored text so it always faces the wearer. → [typography.md](../foundations/typography.md)

**Controls:** **60 × 60 pt** default, 28 × 28 pt minimum. Prefer a discernible background shape and fill — `thin` material on glass, glass material when floating in space. **Never use white background with black text/icons** — the system reserves that for the toggled state. **Use standard controls for their audible feedback**, because **visionOS plays no haptics**. → [selection-and-input.md](../components/selection-and-input.md)

**Audio:** **prefer playing sound** — a silent app feels lifeless or broken. Design custom sounds for custom elements. Use ambient audio and audio sources; prefer **tracked** over **fixed** sound; vary repetitive sounds. → [patterns/media-and-haptics.md](../patterns/media-and-haptics.md)

**Video:** small resizable window, surroundings visible, **never auto-start fully immersive video**, don't occlude the ornament controls, keep inline players 2D and windowed, 160 px scrub thumbnails, RealityKit player for splash/transition video.

**Scroll views:** thicker indicator than iOS (widen tight margins); **Look to Scroll** is off by default and must be added per scroll view — for reading and browsing views only, consistently, with custom scroll effects removed. → [presentation.md](../components/presentation.md)

**Menus and sheets:** menus near the content they control with the **subtle** breakthrough effect; sheets **centered in the field of view**, not emerging from the window's bottom edge, sized to preserve context. Consider a context menu instead of a panel or inspector window to reduce clutter.

**Multitasking:** don't change window edge appearance (it would break the feathered mask the system applies when people look away); **don't pause video when people look away**; expect audio to duck when you're not the Now Playing app.

**Images:** prefer vector; rasterized images above @2x cost performance (apply high-quality filtering above @6x); spatial photos need stereo HEIC with spatial metadata, **standalone views**, and a **feathered glass background** behind overlaid text; spatial scenes take seconds to generate, so gate them behind an explicit action. → [icons-and-images.md](../foundations/icons-and-images.md)

## Key metrics

| Metric | Value |
|---|---|
| Default control size | **60 × 60 pt** |
| Minimum control size | 28 × 28 pt |
| Button center spacing | **≥ 60 pt** |
| Margin around interactive items | **≥ 16 pt** |
| Padding for buttons ≥ 60 pt | 4 pt (hover effect clearance) |
| Default window size | 1280 × 720 pt |
| Initial window distance | ~2 m (≈3 m apparent width) |
| Comfortable reading distance | ≥ 1 m |
| Immersion movement boundary | 1.5 m |
| Default body text | 17 pt (bolder than iOS) |
| Minimum text size | 12 pt |
| Dark Mode | **Not supported** |
| Haptics | **None** |
| Scrub thumbnail width | 160 px |
| Scale factors | @2x or higher (avoid > @6x) |
| Button shapes | Mini 28 / Small 32 / Regular 44 / Large 52 / XL 64 pt |

## React Native landing notes

### Setup

```bash
npx @callstack/react-native-visionos init MyApp     # or add the visionos target
```

`react-native-visionos` renders your RN app in a visionOS **window** in the Shared Space. That's the right default per the guidance — but it also means **volumes, immersive spaces, RealityKit content, and ARKit are outside RN**.

### What you get, and the layout consequences

Your RN views render on the glass window material, so most of the work is respacing for eye targeting:

```js
// visionOS spacing is substantially looser than iOS. Don't reuse iOS values.
export const vision = {
  minTarget: 60,          // vs 44 on iOS
  itemMargin: 16,         // minimum around each interactive item
  centerSpacing: 60,      // minimum between button centers
  largeButtonPadding: 4,  // for buttons ≥60pt, keeps hover effects clear
};
```

```jsx
// A row of buttons: gap derived from center spacing, not chosen by eye.
const BUTTON_W = 44;
const GAP = Math.max(vision.centerSpacing - BUTTON_W, vision.itemMargin);

<View style={{ flexDirection: 'row', gap: GAP }}>
  {/* Capsule shapes in a horizontal row, per the shape guidance. */}
  <Pressable style={{ height: 44, paddingHorizontal: 20, borderRadius: 22 }} />
</View>
```

An iOS layout dropped onto visionOS unchanged is almost always too tight to target reliably — this is the single biggest porting issue.

### Shapes and materials

```jsx
// Rounded rectangle in a vertical stack; capsule in a horizontal row.
const stacked = { borderRadius: 16 };
const inRow   = { borderRadius: 22 };   // capsule at height 44

// Buttons need a discernible background on glass — thin material.
// White background + black text is reserved for the toggled state: don't use it.
<Pressable style={[inRow, { backgroundColor: 'rgba(255,255,255,0.12)' }]} />
```

Don't remove or paint over the window's glass background — RN can't replace it correctly, and an opaque full-bleed background violates the translucency guidance.

### Text

```jsx
// 2D, white by default, bolder than iOS. No text shadows.
<Text style={{ fontSize: 17, fontWeight: '600', color: '#fff' }}>{label}</Text>

// Backgroundless text gets bolded rather than shadowed.
<Text style={{ fontWeight: '700' }}>{floatingLabel}</Text>
```

### No haptics, so sound carries feedback

```js
// expo-haptics does nothing on visionOS. Feedback must come from
// standard controls' audible feedback, or your own sounds.
if (Platform.OS !== 'visionos') Haptics.impactAsync(...);
```

Using standard `Pressable`-based controls gets you the system's audible feedback; a fully custom control is silent unless you add sound.

### Hover

```jsx
// The system hover effect applies automatically to standard interactive views.
// visionOS buttons don't support custom hover effects — don't try to build one.
// Ensure a multi-element control is ONE view so the whole region highlights:
<Pressable>{/* one Pressable wrapping image + label, not two siblings */}
  <Image source={art} />
  <Text>{title}</Text>
</Pressable>
```

### Ornaments, volumes, immersive spaces

```
// All native. react-native-visionos exposes toolbars/tab bars as ornaments
// automatically; anything beyond that needs a SwiftUI scene:
//   WindowGroup      → what RN renders into
//   Volume           → native only
//   ImmersiveSpace   → native only
```

If your app's value is genuinely spatial, RN is the wrong tool for that part — build the immersive scene in SwiftUI/RealityKit and use RN for the windowed UI, or go native.

### Accessibility

```jsx
// Custom gestures don't receive hand input under default VoiceOver, so a
// non-gesture path is mandatory, not optional.
<Pressable accessibilityRole="button" accessibilityLabel="Rotate model" onPress={rotate} />
```

Also avoid head-anchored content and large repetitive gestures, and prefer horizontal over vertical layouts to avoid neck strain. → [accessibility.md](../foundations/accessibility.md)

## Review checklist

- [ ] Button centers ≥ 60 pt apart; ≥ 16 pt margin around each interactive item; no overlapping controls.
- [ ] Control targets ≥ 28 pt, defaulting to 60 pt; buttons ≥ 60 pt have 4 pt padding.
- [ ] iOS spacing values not reused unchanged.
- [ ] Shapes rounded — capsules in rows, rounded rectangles in stacks; no small/mini buttons stacked.
- [ ] Multi-element controls are a single view so the whole region highlights.
- [ ] No white-background/black-text custom buttons (reserved for toggled state).
- [ ] Window glass background retained; no opaque full-bleed backgrounds.
- [ ] Window has a minimum and maximum size; initial size and shape suit the content with little empty area.
- [ ] Content stays within window bounds, clear of system controls above and below.
- [ ] Ornaments no wider than the window, few in number, with borderless buttons; toolbars/tab bars not recreated as ornaments.
- [ ] No content anchored to the head, hands, or wrists; content anchored in space.
- [ ] Readable content placed ≥ 1 m away.
- [ ] Text is 2D, white by default, bolded when backgroundless, never shadowed, billboarded when spatially anchored.
- [ ] Depth used for large elements only, not on text or small objects; depth changes infrequent.
- [ ] No repeating full-field patterns or textures.
- [ ] No peripheral motion; large moving objects made translucent or low-contrast; no world rotation; stationary frame of reference provided.
- [ ] Standard gestures supported everywhere; indirect gestures prioritized; direct gestures reserved for nearby, brief interactions.
- [ ] No custom gesture requires a specific hand, both hands, or sustained raised arms; every one has a non-gesture alternative.
- [ ] Hand-adjacent area left free for system overlays; overlay deferral used only where justified.
- [ ] Sound present and spatial; standard controls used so audible feedback exists; no reliance on haptics.
- [ ] App launches in the Shared Space or `mixed`, with an explicit control to increase immersion and a clear labelled exit.
- [ ] `mixed` doesn't substantially obscure passthrough; `progressive`/`full` avoided if people may move beyond 1.5 m.
- [ ] Immersion transitions gentle and trackable; no auto-entry into immersion.
- [ ] Virtual hands match real hand size and gestures, and fade out on tracking loss.
- [ ] Video: small resizable window, surroundings visible, never auto-immersive, ornament controls unoccluded, inline players 2D.
- [ ] Look to Scroll added to reading/browsing views only, consistently, with custom scroll effects removed.
- [ ] Sheets centered in the field of view, sized to preserve context.
- [ ] Menus near the content they control, using the subtle breakthrough effect.
- [ ] Window edge appearance untouched; video keeps playing when looked away from.
- [ ] Spatial photos in standalone views with feathered glass behind overlaid text.
- [ ] ARKit/hand-tracking permission requested at point of need with a specific reason.
