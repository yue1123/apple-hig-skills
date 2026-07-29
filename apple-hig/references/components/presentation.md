# Sheets, Alerts, Action Sheets, Popovers, Scroll Views & Windows

Source: HIG › Components › Presentation — [Sheets](https://developer.apple.com/design/human-interface-guidelines/sheets), [Alerts](https://developer.apple.com/design/human-interface-guidelines/alerts), [Action sheets](https://developer.apple.com/design/human-interface-guidelines/action-sheets), [Popovers](https://developer.apple.com/design/human-interface-guidelines/popovers), [Scroll views](https://developer.apple.com/design/human-interface-guidelines/scroll-views), [Page controls](https://developer.apple.com/design/human-interface-guidelines/page-controls), [Panels](https://developer.apple.com/design/human-interface-guidelines/panels), [Windows](https://developer.apple.com/design/human-interface-guidelines/windows)

Windows are covered per platform in [platforms/macos.md](../platforms/macos.md), [platforms/ipados.md](../platforms/ipados.md), and [platforms/visionos.md](../platforms/visionos.md).

## Contents

- [Choosing the right presentation](#choosing-the-right-presentation)
- [Sheets](#sheets)
- [Alerts](#alerts)
- [Action sheets](#action-sheets)
- [Popovers](#popovers)
- [Scroll views](#scroll-views)
- [Page controls](#page-controls)
- [Panels (macOS)](#panels-macos)
- [React Native mapping](#react-native-mapping)
- [Review checklist](#review-checklist)

## Choosing the right presentation

This decision is where most presentation mistakes originate, and Apple draws the lines precisely:

| Use | When |
|---|---|
| **Alert** | Something **unexpected** happened that needs attention, and ideally action. Interrupts. |
| **Action sheet** | Offering **choices related to an action the person intentionally took** |
| **Sheet** | A distinct, narrowly scoped modal task |
| **Full-screen modal** | In-depth content or a complex multistep task |
| **Popover** | A small amount of information or functionality, temporarily, in a wide view |
| **Panel** (macOS) | Repeated input + observing results, or inspector functionality |
| **Menu** | Choices people **chose to reveal** |

The alert/action sheet distinction: an **alert is unexpected** — it tells people about a problem or a change. An **action sheet is expected** — it appeared because they just did something. Cancelling a Mail draft produces an action sheet (delete draft / save draft / keep editing), not an alert, because the person initiated it and there are multiple valid choices.

The action sheet/menu distinction: an action sheet appears *because of an action*; a menu appears *because someone opened it*.

## Sheets

**Use alternatives for complex or prolonged flows.** iOS/iPadOS full-screen modal for videos, photos, camera, or multistep editing. On macOS, a **new window** for a self-contained task like document editing, or full-screen mode for media. On visionOS, a transition to a **Full Space**.

**One sheet at a time from the main interface.** Closing a sheet should return to the parent. If a sheet action needs another sheet, **close the first, then present the second** — and re-present the first afterward if needed. Sheet-behind-sheet is how people lose their place.

**Use a nonmodal view for supplementary items that affect the parent's main task** — a split view on visionOS, a panel on macOS, a nonmodal sheet on iOS/iPadOS.

**Always pair Done with Cancel** (or Back). Done alone implies completing the task is the only way out, which is restrictive and misleading.

### Platform notes

- **iOS/iPadOS** — support the **medium detent** for progressive disclosure where it helps (the share sheet shows the most relevant items at medium height). Skip it where full height is genuinely more useful (Messages and Mail compose sheets need the room). **Include a grabber** on a resizable sheet: it signals resizability, cycles detents on tap, and — importantly — **works with VoiceOver** so people can resize without seeing the screen. **Support swipe to dismiss**, and if there are unsaved changes, present an **action sheet** to confirm. On iPad prefer the **page** or **form** sheet styles, which use consistent default sizes centered over a dimmed background.
- **macOS** — use a **reasonable default size**; people don't expect to resize sheets, though supporting resizing is appreciated when content needs expanding. **Let people bring other app windows forward without dismissing the sheet.** Use a **panel** instead when people need to repeatedly provide input and observe results (find and replace, where each replacement is checked individually).
- **visionOS** — **don't emerge a sheet from the window's bottom edge**; center it in the field of view. Keep the default size small enough to preserve context — avoid covering most of the window — but allow resizing.
- **watchOS** — use a sheet **only** when the modal task needs a custom title or custom content presentation; otherwise use an alert or action sheet. Keep interactions **brief and occasional**, and never use a sheet for navigation. If you change the default dismiss label, prefer an SF Symbol — a label that looks like a page or app title leaves people unable to work out how to dismiss it.

## Alerts

**Use them sparingly**, and make every one carry essential information and useful actions.

**Don't use an alert merely to inform.** An informative but non-actionable interruption is resented. Communicate in context instead — Mail shows an indicator when a server connection is unavailable and lets people choose to learn more.

**Don't alert for common, undoable actions, even destructive ones.** Deleting an email or file is the intended outcome and is undoable. **Do** alert for uncommon destructive actions that **can't be undone**, in case they were initiated accidentally.

**Never show an alert at launch.** If there's important information, make it discoverable in the interface. For a startup problem like no network, show cached or placeholder data with a nonintrusive label describing the issue.

### Copy

**Be direct, neutral, approachable.** Alerts usually describe problems, so don't be oblique, accusatory, or minimizing.

**Title:** clearly and succinctly describes the situation — ideally what happened, in what context, and why. Never "Error" or "Error 329347 occurred". Never so long it wraps past two lines. A complete sentence gets **sentence-style capitalization and ending punctuation**; a fragment gets **title-style capitalization and no punctuation**.

**Informative text only if it adds value** — short, complete sentences, sentence case, proper punctuation.

**Don't explain the buttons.** If the text and titles are clear, explanation is unnecessary. In the rare case you must guide, use "choose" (device- and input-agnostic) and refer to the button by its exact title without quotes.

**Include a text field only if input is needed to resolve the situation** — a password, for instance.

### Buttons

**One or two words describing the result** — verbs and verb phrases tied to the alert text: "View All", "Reply", "Ignore". Sentence-style capitalization, no ending punctuation. Always title a cancelling button **"Cancel"**.

**Avoid "OK" except in purely informational alerts.** "OK" is ambiguous in a confirmation — does it mean "OK, do it" or "OK, I understand what would have happened"? Use "Erase", "Convert", "Clear", "Delete". Avoid "Yes" and "No" entirely.

**Placement:** the most likely choice goes on the **trailing** side of a row or the **top** of a stack; the default button always goes there. Cancel goes **leading** in a row or **bottom** in a stack.

**Use the destructive style for a destructive action people *didn't* deliberately choose.** The nuance matters: Empty Trash's confirmation does **not** style Empty Trash as destructive, because the button performs the person's original intent and the convenience of pressing Return outweighs re-warning them. Destructive styling is for drawing attention to consequences they didn't intend.

**Include Cancel whenever there's a destructive action** — and **never make Cancel the default**. If you want people to actually read an alert rather than reflexively pressing Return, **make no button the default**. For a single-button alert that is also the default, title it **Done**, not Cancel.

**Offer alternative ways to cancel** — keyboard shortcuts and other quick paths.

### Platform notes

- **iOS/iPadOS** — use an **action sheet, not an alert**, for choices related to an intentional action. **Avoid alerts that scroll**; keep titles short and messages brief, since large text sizes can force scrolling.
- **macOS** — use a caution symbol like `exclamationmark.triangle` **sparingly**: only when extra attention is genuinely needed, such as confirming possible *unexpected* data loss. **Not** for tasks whose entire purpose is overwriting or removing data (save, empty trash).

## Action sheets

**Use sparingly** — they interrupt too.

**Titles short enough for one line.** Long titles are slow to read and may truncate or force scrolling.

**A message only if necessary** — usually the title plus the context of the action is enough.

**Provide Cancel where data could be destroyed**, at the **bottom** of the sheet (**upper-left** on watchOS).

**Make destructive choices prominent** — destructive style, placed at the **top** where they're most noticeable.

Note the asymmetry with alerts: in an action sheet, destructive goes **top** and Cancel goes **bottom**; in an alert, the default goes **trailing/top** and Cancel goes **leading/bottom**.

### Platform notes

- **iOS/iPadOS** — **avoid letting an action sheet scroll.** More buttons means more time to choose, and scrolling one is hard to do without accidentally tapping a button.
- **watchOS** — **at most four buttons including Cancel**, so aim for three choices.

## Popovers

**Expose a small amount of information or functionality** — a few related tasks. A popover disappears after interaction, so a calendar event popover lets people change date, time, or calendar and then get back to reviewing events.

**Consider a popover when you need room for content temporarily.** Sidebars and panels consume permanent space.

**Point the arrow as directly as possible at the element that revealed it**, and ideally cover neither that element nor essential content people need while using it.

**Use a Close button only for confirmation and guidance** — where it clarifies exiting with or without saving. Otherwise a popover closes on outside click/tap or item selection. **If multiple selections are possible, keep it open** until explicit dismissal or an outside tap.

**Always save work when automatically closing a nonmodal popover.** People dismiss nonmodal popovers accidentally by tapping outside. **Discard only on an explicit Cancel.**

**One popover at a time.** Never cascade or nest them; close the open one first.

**Nothing displays over a popover** except an alert.

**Let people close one and open another in a single click or tap** — especially when several bar buttons each open one.

**Keep popovers only as big as their contents require.**

**Animate size changes** between condensed and expanded views, so it doesn't read as a different popover replacing the old one.

**Don't use a popover for a warning** — people miss them or close them accidentally. Use an alert.

**Don't say "popover" in help documentation.** "Select the Show button", not "Select the Show button at the bottom of the popover".

### Platform notes

- **iOS/iPadOS** — **avoid popovers in compact views.** Adjust layout by size class: popovers for wide views, a full-screen modal or sheet for compact ones.
- **macOS** — consider letting people **detach** a popover into a panel so it stays visible alongside other information. Keep the detached panel's appearance nearly identical so context is preserved.

## Scroll views

**Support default scrolling gestures and keyboard shortcuts.** If you build custom scrolling, keep the **elastic** indicator behavior people expect.

**Make scrollability apparent.** Scroll indicators aren't always visible, so show **partial content at the view's edge** to signal more exists. Most people try scrolling anyway, but signposting it is considerate.

**Never nest scroll views of the same orientation** — the result is unpredictable and hard to control. Horizontal inside vertical (or vice versa) is fine.

**Consider page-by-page scrolling** where it fits. Define the page as the view's current height or width, and consider subtracting a **unit of overlap** — a line of text, a row of glyphs, part of a picture — so people keep their context across pages.

**Scroll automatically only to help people find their place** when relevant content has left the view.

**Set sensible zoom minimums and maximums.** Zooming until one character fills the screen rarely makes sense.

### Scroll edge effects

**Prefer the automatic style.** It gives more opaque separation for top toolbars with many controls, text outside Liquid Glass controls, and pinned table headers. If you choose the soft style, test thoroughly for control legibility.

**Only use a scroll edge effect where a scroll view sits behind floating interface elements.** They aren't decorative — they don't block or darken like an overlay; they exist so controls stay visually distinct.

**One scroll edge effect per view.** In iPad and Mac split views each pane can have its own — keep their **heights consistent** so they align.

### Platform notes

- **iOS/iPadOS** — show a **page control** with page-by-page scrolling, and **hide the scroll indicator on the same axis** so you don't present two redundant position indicators.
- **macOS** — small or mini scroll bars are acceptable in space-tight panels; use one size for all controls in that panel.
- **visionOS** — the scroll indicator is **thicker** than iOS's, so increase tight margins to prevent overlap. **Support Look to Scroll** in reading and browsing views (it's **off by default** and must be added per scroll view). **Don't** use it for views with UI controls or dense information needing precise scrolling — Notes supports it in the note body but not in the notes list. **Be consistent**: if one collection view supports it, all similar ones should. Prefer making Look-to-Scroll views **full width or full height** for generous space and clear edges; if inset, provide clear boundaries. **Remove custom scroll-driven effects** — parallax and scroll-position animations make Look to Scroll behave unexpectedly.
- **watchOS** — prefer **vertical** scrolling, driven by the Digital Crown. Use **tab views for page-by-page** content: stacked vertically, the Crown moves between full-screen pages and the system shows a page indicator that **expands into a scroll indicator** when a page is taller than the screen. Prefer limiting each page to **one screen height** for glanceability; use variable-height pages sparingly and place them **after** fixed-height ones.

## Page controls

**For movement between an ordered list of pages only** — not hierarchical or non-sequential relationships. Use a sidebar or split view for more complex navigation.

**Center it horizontally near the bottom** of the view.

**Don't exceed about 10 dots.** More than that is impossible to count at a glance; consider a grid or another arrangement that allows arbitrary-order navigation.

**Custom indicator images must be simple** — no complex shapes, negative space, text, or inner lines, all of which turn muddy at indicator size. SF Symbols work well.

**Customize the default indicator only when it enhances meaning** — `bookmark.fill` if every page holds bookmarks.

**At most two different indicator images.** One special page (Weather's current-location page) is findable; several unique images force people to memorize a legend and look haphazard.

**Don't color indicators.** Custom colors reduce the contrast that distinguishes the current page and keeps the control visible. Let the system color them.

### Platform notes

- **iOS/iPadOS** — **don't animate page transitions during scrubbing.** Scrubbing is fast, and animating every transition causes lag and visual flashing; animate only for taps. **Don't support scrubbing with the minimal background style**, which gives no scrubbing feedback — use automatic or prominent.
- **tvOS** — for collections of **full-screen** peer pages. Additional controls make focus hard to maintain while moving between pages.
- **watchOS** — use **vertical** pagination with the Digital Crown; it's more effective than horizontal pagination or deep hierarchical navigation. Give each page a clear purpose and prefer one screen height.

## Panels (macOS)

**Quick access to important controls or information related to the current content** — controls or settings affecting the selected item.

**Good for inspector functionality** — an inspector shows details of the current selection and updates as the selection changes. An **Info** window, which keeps the same contents regardless of selection, should be a **regular window**, not a panel. A split view pane is another option for an inspector.

**Prefer simple adjustment controls** — sliders and steppers, which give direct control. Avoid controls requiring typing or multi-step selection.

**Write a brief noun or noun-phrase title** with title-style capitalization — "Fonts", "Colors", "Inspector". A panel floats above other windows and needs a title bar so people can position it.

**Bring all open panels forward when your app activates**, regardless of which window was active. **Hide all panels when your app is inactive.**

**Don't list panels in the Window menu's documents list** — show/hide commands in the Window menu are fine, but panels aren't documents or standard windows.

**Generally disable the minimize button** — a panel appears when needed and disappears when the app is inactive, so minimizing is pointless.

**Refer to panels by title, without the word "panel"** — "Show Fonts", "Show Colors", "Show Inspector". In help, use the title alone, or append "window" where it clarifies ("Fonts window").

**Prefer standard panels over HUDs.** A HUD with no logical reason confuses, and may not match the current appearance setting. If you use one: **keep the style consistent** across mode changes (a HUD in full screen stays a HUD out of it), **use color sparingly** (too much color in a dark HUD distracts — small amounts of high-contrast color for important information), and **keep it small** so it neither obscures the content it adjusts nor competes for attention.

## React Native mapping

### Sheets with detents

```jsx
// Native-stack formSheet gives real UISheetPresentationController behavior.
<Stack.Screen
  name="Filters"
  component={Filters}
  options={{
    presentation: 'formSheet',
    sheetAllowedDetents: [0.5, 1.0],   // medium + large
    sheetGrabberVisible: true,          // signals resizability AND works with VoiceOver
    sheetCornerRadius: 20,
    sheetExpandsWhenScrolledToEdge: true,
  }}
/>
```

The grabber is not decoration — omitting it removes the VoiceOver resize affordance entirely.

Guard swipe-dismiss when there are unsaved changes, and confirm with an **action sheet** (not an alert), per the sheet guidance — see [patterns/modality-and-multitasking.md](../patterns/modality-and-multitasking.md) for the `beforeRemove` implementation.

Always pair Done with Cancel:

```jsx
// Wrong: Done only.
<Header right={<Button title="Done" />} />

// Right.
<Header left={<Button title="Cancel" role="cancel" />} right={<Button title="Done" role="primary" />} />
```

### Alerts

```js
import { Alert } from 'react-native';

// Title states what happened and why; buttons name their results.
Alert.alert(
  'Couldn\'t Save Your Note',                       // fragment → title case, no period
  'The note is stored on iCloud, which isn\'t reachable right now.',   // sentence, punctuated
  [
    { text: 'Cancel', style: 'cancel' },            // leading on iOS
    { text: 'Retry', onPress: retry },              // trailing = most likely choice
  ],
);
```

`Alert.alert` maps the button order correctly on iOS — `cancel` goes leading, the last non-cancel button goes trailing. On Android the order differs; don't hand-position them.

Things not to do:

```js
// Wrong: informational-only interruption.
Alert.alert('Sync Complete');                       // put it in the UI instead

// Wrong: ambiguous OK on a confirmation.
Alert.alert('Delete all photos?', '', [{ text: 'Cancel' }, { text: 'OK' }]);
// Right: name the action.
Alert.alert('Delete All Photos?', 'This can\'t be undone.',
  [{ text: 'Cancel', style: 'cancel' }, { text: 'Delete All', style: 'destructive', onPress: del }]);

// Wrong: alert on launch.
useEffect(() => { if (!online) Alert.alert('No Internet'); }, []);
// Right: cached data plus an inline banner.
```

`Alert` has no text-input variant on Android; `Alert.prompt` is iOS-only. Use it only when input is genuinely needed to resolve the situation.

### Action sheets

```js
import { ActionSheetIOS } from 'react-native';   // or @expo/react-native-action-sheet cross-platform

// Destructive at top, Cancel at bottom — the action-sheet ordering, not the alert ordering.
ActionSheetIOS.showActionSheetWithOptions(
  {
    title: 'Discard this draft?',            // one line
    options: ['Delete Draft', 'Save Draft', 'Cancel'],
    destructiveButtonIndex: 0,
    cancelButtonIndex: 2,
  },
  i => { if (i === 0) discard(); if (i === 1) save(); },
);
```

Use this — not an alert — whenever the person initiated the action and there are multiple valid responses.

### Popovers

```jsx
// iPad/macOS wide views only. In compact views, present a sheet instead.
const { width } = useWindowDimensions();
const usePopover = width >= 768;

usePopover
  ? <Popover anchor={buttonRef} onRequestClose={savingClose}>{content}</Popover>
  : <BottomSheet>{content}</BottomSheet>;
```

`react-native-popover-view` covers the arrow and positioning. The critical behavior to implement correctly:

```jsx
// Outside tap = dismiss AND save (nonmodal popovers get dismissed accidentally).
onRequestClose={() => { commitChanges(); close(); }}
// Only an explicit Cancel discards.
onCancel={() => { discardChanges(); close(); }}
```

Getting this backwards — discarding on outside tap — is the most damaging popover bug, because the dismissal is often unintentional.

### Scroll views

```jsx
<ScrollView
  // Never nest same-axis scroll views. Horizontal inside vertical is fine:
  //   <ScrollView>{…}<ScrollView horizontal />{…}</ScrollView>
  contentInsetAdjustmentBehavior="automatic"
  // Elastic behavior is the default on iOS — don't disable bounces to "fix" it.
  // Page-by-page:
  pagingEnabled={isPaged}
  showsHorizontalScrollIndicator={!isPaged}   // no indicator when a page control is shown
  decelerationRate="fast"
/>
```

For paged content with overlap:

```jsx
// snapToOffsets with an overlap unit, per the HIG page-overlap guidance.
const OVERLAP = 24;
const offsets = pages.map((_, i) => i * (pageWidth - OVERLAP));
<ScrollView horizontal snapToOffsets={offsets} decelerationRate="fast" />
```

Signal more content at the edge rather than relying on the indicator:

```jsx
// Let the next item peek — the "partial content at the edge" cue.
<FlatList horizontal contentContainerStyle={{ paddingRight: 40 }} snapToInterval={CARD_W + GAP} />
```

Scroll edge effects come from the native header/tab bar (`headerTransparent` + `headerBlurEffect`); there's no JS API to add one to an arbitrary view, and hand-rolling a gradient overlay is not equivalent — it darkens content rather than preserving control legibility.

### Page controls

```jsx
// ≤10 dots, centered near the bottom, system-colored.
function PageControl({ count, index }) {
  if (count > 10) return null;   // use a different arrangement instead
  return (
    <View style={s.dots} accessibilityRole="tablist" accessibilityValue={{ text: `Page ${index + 1} of ${count}` }}>
      {Array.from({ length: count }, (_, i) => (
        <View key={i} style={[s.dot, i === index && s.dotActive]} />
      ))}
    </View>
  );
}
const s = StyleSheet.create({
  dots: { position: 'absolute', bottom: 16, alignSelf: 'center', flexDirection: 'row', gap: 8 },
  dot: { width: 7, height: 7, borderRadius: 3.5, backgroundColor: PlatformColor('tertiaryLabel') },
  dotActive: { backgroundColor: PlatformColor('label') },
});
```

Note the semantic colors rather than a brand tint — that's the "don't color indicators" rule, and it's what keeps the current page distinguishable in both appearances.

### Panels

macOS panels need `NSPanel`, which `react-native-macos` doesn't expose from JS. An inspector is better modeled as a split-view pane in RN — which the HIG explicitly allows.

## Review checklist

- [ ] Presentation type matches the situation: alert for unexpected, action sheet for intentional actions, sheet for scoped tasks, popover for small temporary content.
- [ ] No alert used purely to inform, or shown at launch.
- [ ] No confirmation on common undoable destructive actions; confirmation present for uncommon irreversible ones.
- [ ] Alert titles describe what happened without error codes, ≤ 2 lines, correct capitalization for sentence vs. fragment.
- [ ] Alert buttons are 1–2 word verb phrases naming the result; no "OK" outside informational alerts; no "Yes"/"No".
- [ ] Cancel titled exactly "Cancel", never the default; single-button default alerts use "Done".
- [ ] Destructive style used only for consequences the person didn't intend.
- [ ] Alerts don't scroll; no button explanations.
- [ ] Action sheets put destructive at top, Cancel at bottom, and don't scroll (≤ 4 buttons on watchOS).
- [ ] Sheets: one at a time, Done always paired with Cancel/Back, grabber present when resizable.
- [ ] Swipe-dismiss confirmed via action sheet when unsaved changes exist.
- [ ] iPad sheets use page/form styles; visionOS sheets centered and context-preserving.
- [ ] Popovers only in wide views; sheets substituted in compact views.
- [ ] Popover outside-tap saves; only explicit Cancel discards.
- [ ] One popover at a time; nothing but an alert over a popover; popover sized to content with animated size changes.
- [ ] No popover used as a warning; the word "popover" absent from help text.
- [ ] No same-axis nested scroll views; elastic behavior preserved.
- [ ] Scrollability signalled by peeking content, not just the indicator.
- [ ] Scroll edge effect only behind floating controls, one per view, consistent heights across split panes.
- [ ] Paged scrolling hides the same-axis indicator and shows a page control.
- [ ] Page controls ≤ 10 dots, centered near the bottom, system-colored, ≤ 2 indicator image types.
- [ ] Page transitions not animated during scrubbing.
- [ ] Zoom has sensible min/max.
- [ ] visionOS: Look to Scroll added to reading/browsing views only, consistently, with custom scroll effects removed and margins widened for the thicker indicator.
- [ ] watchOS: vertical paging via the Digital Crown, pages limited to one screen height where possible.
