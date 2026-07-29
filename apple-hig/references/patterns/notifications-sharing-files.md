# Notifications, Sharing, Drag & Drop, Files & Printing

Source: HIG › Patterns › [Managing notifications](https://developer.apple.com/design/human-interface-guidelines/managing-notifications), [Collaboration and sharing](https://developer.apple.com/design/human-interface-guidelines/collaboration-and-sharing), [Drag and drop](https://developer.apple.com/design/human-interface-guidelines/drag-and-drop), [File management](https://developer.apple.com/design/human-interface-guidelines/file-management), [Printing](https://developer.apple.com/design/human-interface-guidelines/printing)

Read this when sending push notifications, adding a share affordance, implementing drag and drop, handling documents, or offering print.

## Contents

- [Notifications](#notifications)
- [Sharing and collaboration](#sharing-and-collaboration)
- [Drag and drop](#drag-and-drop)
- [File management](#file-management)
- [Printing](#printing)
- [React Native mapping](#react-native-mapping)
- [Review checklist](#review-checklist)

## Notifications

### Focus and delivery scheduling

Two user-facing mechanisms you must design around:

- **Focus** filters notifications during a reserved activity — sleeping, working, reading, driving.
- **Delivery scheduling** lets people receive alerts immediately or batched into a summary at chosen times.

People choose which contacts and apps break through a Focus, and may choose to let all **Time Sensitive** notifications through.

**Important:** even when Focus delays the *alert*, the notification itself is available as soon as it arrives. You're influencing when someone is interrupted, not whether they get the data.

### Communication vs. noncommunication

- **Communication** notifications — direct communications like calls and messages. Adopt SiriKit intents, which also lets people customize behavior through Siri. Delivery timing is determined by **the sender**.
- **Noncommunication** notifications — everything else. You must specify an **interruption level**, which determines delivery timing.

### Interruption levels

| Level | Meaning | Overrides scheduled delivery | Breaks through Focus | Overrides Ring/Silent |
|---|---|---|---|---|
| **Passive** | Viewable at leisure — a restaurant recommendation | No | No | No |
| **Active** (default) | Worth knowing on arrival — a score update | No | No | No |
| **Time Sensitive** | Directly impacts the person, needs immediate attention — account security issue, package delivery | **Yes** | **Yes** | No |
| **Critical** | Urgent health and safety — extremely rare, typically governmental/public agencies or health/home apps | **Yes** | **Yes** | **Yes** |

**Critical requires an entitlement**, precisely because it overrides the Ring/Silent switch.

### Rules

**Represent urgency accurately.** People can adjust or disable your notifications entirely. A high urgency level attached to low-priority information is how you lose the permission.

**Use Time Sensitive only for things relevant *in the moment*** — an event happening now or within the hour. The system tells people how Time Sensitive works the first time you use it and offers to turn it off, then **periodically re-offers** that choice. So misusing the level doesn't just annoy people; the OS actively surfaces the opt-out.

**Never use Time Sensitive for marketing.** Even with consent to marketing notifications, they must never break through Focus or scheduled delivery.

**Get explicit permission for promotional/marketing notifications** — a dedicated alert, modal, or interface describing what you'd send, with a clear opt-in and opt-out.

**Provide an in-app settings screen** where people can change those choices. This is required, not optional.

### watchOS

iPhone notification settings apply to the same app on Apple Watch by default. People manage them in the Watch app on iPhone, or swipe left on an arriving notification for per-notification options like **Mute 1 Hour** or **Turn off Time Sensitive**.

## Sharing and collaboration

**Put the Share button somewhere convenient — a toolbar.** The system share sheet (iOS 16+) and sharing popover (iPadOS 16 / macOS 13+) include file-sharing method selection and permission setup for a new collaboration.

**Customize the sheet for the sharing types you support.** With CloudKit, pass both the file and your collaboration object and the sheet automatically offers "send copy". With iCloud Drive it's supported by default. For custom collaboration, include a file — or a plain-text representation — in the collaboration object.

**Summarize permissions in a short phrase** — "Only invited people can edit", "Everyone can make changes". The system uses it as the label of the button that reveals sharing options.

**Keep sharing options minimal and grouped** — who can access, read vs. edit, whether collaborators can invite others.

**Show the Collaboration button prominently once collaboration starts**, next to the Share button. It reminds people the content is shared and identifies who's sharing.

**Add custom actions to the collaboration popover only if needed.** The popover has three sections: collaborators + Messages/FaceTime buttons (top), your custom items (middle), manage-shared-file button (bottom). Only the most essential items belong in the middle — Notes puts recent-update summaries and buttons for more detail.

**Customize the management button title** if "Manage Shared File" doesn't fit. CloudKit sharing provides the management view; otherwise you build it.

**Consider posting collaboration events to Messages** — content changes, membership changes, participant mentions — with a universal link back into the relevant view.

## Drag and drop

**Support it broadly.** People try drag and drop everywhere. System components like text fields and text views support it already.

**Always provide alternatives.** Dragging is inconvenient or impossible for some people. Provide menu commands to copy and move. On iOS/iPadOS, declare accessibility drag sources and drop points so assistive technology users can perform the same operation.

**Move vs. copy:** move when source and destination containers are the **same**; copy when they're **different**. Before deviating, consider what people expect and choose whichever is less likely to cause frustration or data loss.

**Support multi-item drag** where sensible. iOS/iPadOS/macOS/visionOS allow selecting multiple items and dragging as a group; macOS allows gathering from several apps; iPadOS lets people **add items mid-drag**.

**Prefer undoable drops.** People misdrop. Where undo isn't possible, ask for confirmation — the Finder confirms dragging into a write-only folder, because the file couldn't be retrieved afterward. Or provide a reversal: Photos lets people cancel sharing after dropping into a shared stream.

**Offer multiple fidelities, highest first.** A line drawing might offer PDF vector, then lossless PNG with transparency, then lossy JPEG. A chart might offer the native chart object, then an image for destinations that can't take it.

**Consider spring loading** — activating a control by hovering dragged content over it. Calendar lets people drag an event over day/week/month/year segments to move it. Force-click on a Magic Trackpad; hover on iPad.

### Feedback while dragging

**Show the drag image after about three points of movement.** A **translucent** representation of the content — translucency distinguishes it from the original and lets people see destinations underneath. Keep it until the drop.

**Modify the drag image when it adds clarity** — expanding to show the photo's default size in the target document. Use **flocking** to visually group multiple items (so people can confirm nothing was missed) and ungroup on drop. But don't let the drag image change constantly or radically.

**Show whether a destination accepts the content.** Insertion point or highlight when it can; nothing, or an explicit `circle.slash`, when it can't. Show the cue **only while content is over the destination**, and highlight one destination at a time.

**Give feedback on invalid or failed drops** — the item returns to its source if visible, or scales up and fades out to read as evaporating rather than landing.

### Accepting drops

**Auto-scroll the destination** while dragging within a scrolling container — and stop once the drag leaves the container.

**Take the richest version you can handle.** Extract the native chart object if you support charts; fall back to the image if not.

**Extract only the relevant portion.** Dragging a contact into an email recipient field yields name and email, not the postal address.

**Check the Option key at drop time.** Holding Option forces a same-container drag to copy. Releasing Option before dropping makes it a move — so read the state at drop, not at drag start.

**Show progress for slow transfers** — a progress indicator, and a placeholder at the drop location in lists and collections so people know where the content will land.

**Show that a drop initiated a task** — dropping on a print control should visibly begin printing and report progress.

**Style dropped text appropriately** — preserve font, typeface, size, and attributes when both sides support the same styles; otherwise apply the destination's style.

**Manage selection after the drop.** Dropped content stays selected so people can act on it. Same-container move: content disappears from the original location. Same-container copy: **deselect the original**. Cross-container: deselect in the source.

### Platform specifics

- **iOS/iPadOS** — support adding items to an in-progress drag session (with flocking feedback) and accepting multiple simultaneous drops.
- **macOS** — let people drag content **into the Finder** in a format your app can reopen (Calendar exports `.ics`); text becomes a *clipping*, a temporary container unrelated to the Clipboard. Allow dragging from an **inactive window** without activating it (a *background selection*, which looks different from an active selection), and dragging an individual unselected item from an inactive window without disturbing its existing selection. Consider a **numeric badge** for multi-item drags, updating it if the destination accepts only a subset. Change the **pointer** to indicate the outcome — copy, drag link, disappearing item, operation not allowed. Let people select and drag in a single motion.
- **visionOS** — handle content dropped into **empty space** by launching a window or scene for it. Dropping a URL launches Safari; dropping Quick Look–supported content launches Quick Look.

## File management

**Provide New and Open via menus and keyboard shortcuts.** iPadOS surfaces them in the Command-key shortcut overlay; macOS in the File menu. Include an **Add (+) button** regardless of keyboard availability.

**A custom file browser must respect the platform's file system.** Open at the most relevant location — Documents, iCloud, or the last-used folder — but let people navigate the rest of the file system.

**Save automatically.** People should be confident work is preserved unless they cancel or delete. Save periodically while editing, on close, and on app switch. Avoid requiring an explicit save action.

**Hide file extensions by default**, let people show them, and reflect that choice in every save and open interface.

**Use a Quick Look viewer** so people can preview files your app can't open. **Implement a Quick Look generator** for your custom file types so Finder, Files, and Spotlight can preview them.

### iOS / iPadOS document launch experience

- **Title card buttons map to your most important functions** — primary usually creates a new document; secondary offers options (Numbers: "Start Writing" / "Choose a Template").
- **Background distinct from accessories and title card.** Solid color, gradient, or pattern; avoid complex imagery that competes with foreground elements.
- **Place accessories carefully** — in front of and behind the title card for depth, but the app name and both buttons must stay clearly visible. Don't clutter, and test across screen sizes and orientations.
- **Animate sparingly** — gentle, repeating motion (breathing, swaying), nothing disorienting.

### File provider extensions

- **Show only contextually appropriate documents** — a PDF editor loading your extension should see only PDFs. Consider showing modification dates, sizes, and local vs. remote status.
- **Let people choose a destination** when exporting or moving, unless you store everything in one directory. Consider allowing new subdirectories.
- **No custom top toolbar** — the extension loads in a modal view that already has one.

### macOS

- **Make file opening convenient** — "open recent" alongside "open", filter criteria, multi-select. You can retitle the Open button to match the task ("Insert").
- **Provide a save interface** for name, format, and location. New documents default to "Untitled". Offer format choice if you support multiple.
- **Consider a custom accessory view in the Save dialog** — Mail's includes an option to include attachments.
- **Handle autosave being off.** People can disable it via "Ask to keep changes when closing documents". Then you must show unsaved state and present a save dialog on close, quit, logout, or restart.
- **Indicate unsaved changes only when autosave is off** — a dot on the window's close button and beside the name in the Window menu. With autosave **on**, showing that dot is confusing because it implies action is needed. "Edited" in the title bar is fine either way, but remove it as soon as a save occurs.

## Printing

**Make printing discoverable in standard locations** — a Print item in the macOS File menu; a toolbar button opening an action sheet on iOS/iPadOS. A macOS toolbar Print button is fine as an *optional* customizable button.

**Only present printing when it's possible.** Nothing to print or no printers available → dim the macOS File menu item, remove the action from the iOS action sheet, dim or hide a custom button.

**Use the system view for relevant options** — page range, copies, duplex — where the printer supports them.

### macOS

- **Add a custom print panel category** for app-specific options, named uniquely (e.g. your app name). Keynote offers presenter notes, slide backgrounds, skipped slides.
- **Consider a page setup dialog** for document-specific page size, orientation, and scaling — but don't reimplement what the system already provides (orientation, reverse order).
- **Make interdependencies clear** — double-sided available means transparencies unavailable.
- **Separate advanced from frequent options** behind a disclosure control labeled *Advanced Options*.
- **Consider previewing a setting's effect** — updating a thumbnail as a tone control changes.
- **Store modified settings with the document**, at minimum until it closes.

## React Native mapping

### Notification interruption levels

The level is a payload field, so it's set server-side — but the design decision is yours:

```json
{
  "aps": {
    "alert": { "title": "Package delivered", "body": "Left at the front door." },
    "interruption-level": "time-sensitive",
    "relevance-score": 0.9
  }
}
```

Valid values: `passive`, `active` (default), `time-sensitive`, `critical`. Marketing content must use `passive` or `active` — never `time-sensitive`.

```js
// Request only what you'll use. Note provisional authorization: notifications
// arrive quietly in Notification Center without a permission prompt, which is a
// good fit for the "represent urgency accurately" principle — you earn the alert.
import * as Notifications from 'expo-notifications';

await Notifications.requestPermissionsAsync({
  ios: {
    allowAlert: true,
    allowBadge: true,
    allowSound: true,
    allowProvisional: true,          // quiet delivery, no prompt
    // allowCriticalAlerts requires an Apple entitlement — don't request casually.
  },
});
```

Provide the required in-app settings screen, and separate informational from marketing consent:

```jsx
<SettingsSection title="Notifications">
  <Switch label="Order updates" value={prefs.transactional} onValueChange={…} />
  <Switch label="Offers and promotions" value={prefs.marketing} onValueChange={…} />
  <Row title="System Notification Settings" onPress={() => Linking.openSettings()} />
</SettingsSection>
```

### Share sheet

```js
import { Share } from 'react-native';

// The system share sheet — don't build a custom sharing UI.
await Share.share({
  message: recipe.title,
  url: recipe.webUrl,       // iOS: url gives rich previews and more destinations
  title: recipe.title,      // Android
});
```

For files and richer activity items, `expo-sharing` (`shareAsync`) handles a local file URI, which gives the sheet a proper preview:

```js
import * as Sharing from 'expo-sharing';
if (await Sharing.isAvailableAsync()) await Sharing.shareAsync(fileUri, { UTI: 'com.adobe.pdf' });
```

Put the trigger in the toolbar with the standard symbol:

```jsx
<Pressable onPress={onShare} accessibilityRole="button" accessibilityLabel="Share">
  <Icon name="square.and.arrow.up" />{/* the Apple share glyph — see icons-and-images.md */}
</Pressable>
```

CloudKit collaboration and the Collaboration button have no RN bindings — a genuinely collaborative app needs native work here.

### Drag and drop

RN's core has no drag-and-drop between apps. Options:

```jsx
// Within the app: react-native-gesture-handler + Reanimated.
const drag = Gesture.Pan()
  .activateAfterLongPress(200)
  .onStart(() => {
    opacity.value = 0.7;         // translucent drag image, per the guidance
    scale.value = withSpring(1.03);
  })
  .onUpdate(e => { x.value = e.translationX; y.value = e.translationY; })
  .onEnd(() => {
    const target = hitTestDropZones(x.value, y.value);
    if (!target) {
      // Invalid drop: return to source, per "give feedback on failed drops".
      x.value = withSpring(0); y.value = withSpring(0);
    } else {
      commitDrop(target);
    }
    opacity.value = 1;
  });
```

Note the ~3 pt activation threshold from the HIG maps onto `activeOffsetX/Y` rather than firing on touch-down, so a tap doesn't start a drag.

Highlight only the hovered zone:

```jsx
<View style={[s.zone, isHovered && (canAccept ? s.zoneValid : s.zoneInvalid)]}>
  {isHovered && !canAccept && <Icon name="circle.slash" />}
</View>
```

Cross-app drag and drop (iPadOS) needs native `UIDragInteraction` — `react-native-drax` and `react-native-drag-drop` cover in-app cases only. For accepting drops from other apps, `expo-document-picker` plus a declared file-type association covers the import path without a full drag implementation.

Always provide the alternative:

```jsx
// Menu equivalents for every drag operation — plus accessibilityActions.
<ContextMenu actions={[{ title: 'Move to…' }, { title: 'Duplicate' }]} />
```

For reordering lists, `DraggableFlatList` gives the drag path; pair it with accessibility actions ("Move up"/"Move down") as covered in [accessibility.md](../foundations/accessibility.md).

### Files and autosave

```jsx
// Autosave: no explicit save action, saved on edit (debounced), on blur, and on background.
const save = useDebouncedCallback(doc => persist(doc), 800);
useEffect(() => { save(doc); }, [doc]);

useEffect(() => {
  const sub = AppState.addEventListener('change', s => {
    if (s !== 'active') save.flush();   // flush before suspension
  });
  return () => sub.remove();
}, []);
```

Never show an unsaved-changes dot while autosaving — the HIG note about it implying required action applies equally in RN.

```js
// Document picking and export.
import * as DocumentPicker from 'expo-document-picker';
const res = await DocumentPicker.getDocumentAsync({ type: 'application/pdf' });  // contextual types only

// Let people choose the destination on export.
import * as Sharing from 'expo-sharing';
await Sharing.shareAsync(exportedUri);   // routes through Files, iCloud, etc.
```

Quick Look previews come from `expo-file-system` + `Sharing`, or a native module wrapping `QLPreviewController` for in-app preview of unsupported types.

### Printing

```jsx
import * as Print from 'expo-print';

// Hide the affordance when printing isn't possible.
const [canPrint, setCanPrint] = useState(false);
useEffect(() => { Print.selectPrinterAsync && setCanPrint(true); }, []);

{canPrint && <Pressable onPress={() => Print.printAsync({ html })}><Icon name="printer" /></Pressable>}
```

`printAsync` presents the system print panel, which supplies page range, copies, and duplex — don't build your own options UI.

## Review checklist

- [ ] Interruption level chosen honestly; Time Sensitive only for within-the-hour relevance.
- [ ] No marketing notification uses Time Sensitive; Critical only with an entitlement.
- [ ] Explicit opt-in obtained for promotional notifications, with an in-app settings screen to change it.
- [ ] Provisional authorization considered instead of an upfront prompt.
- [ ] Notification permission request asks only for the capabilities used.
- [ ] Share uses the system share sheet with the standard `square.and.arrow.up` symbol in a toolbar.
- [ ] Share payload includes a URL/file so the sheet can show a rich preview.
- [ ] Every drag-and-drop operation has a menu or accessibility-action alternative.
- [ ] Drag activates after a small threshold, not on touch-down; drag image is translucent.
- [ ] Drop zones highlight only while hovered, one at a time; invalid drops show `circle.slash` or no cue.
- [ ] Failed drops animate back to source or fade out.
- [ ] Same-container drags move; cross-container drags copy (unless deliberately reasoned otherwise).
- [ ] Drops are undoable, or confirmed when irreversible.
- [ ] Slow transfers show progress and a placeholder at the drop location.
- [ ] Selection state handled after drop (original deselected on same-container copy; source deselected cross-container).
- [ ] Autosave on edit, blur, and backgrounding; no explicit save required.
- [ ] No unsaved-changes indicator while autosave is on.
- [ ] File extensions hidden by default, consistently across open and save UI.
- [ ] File pickers restricted to contextually relevant types; export lets people choose a destination.
- [ ] Quick Look preview offered for file types the app can't open.
- [ ] Print action in a standard location and hidden/dimmed when impossible; system print panel used for options.
- [ ] macOS: drag to Finder in a reopenable format; background selections draggable without activating the window; pointer reflects the drop outcome.
- [ ] visionOS: drops into empty space handled by launching an appropriate window or scene.
