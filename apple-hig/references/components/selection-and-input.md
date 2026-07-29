# Text Fields, Toggles, Pickers, Sliders & Keyboards

Source: HIG › Components › Selection and input — [Text fields](https://developer.apple.com/design/human-interface-guidelines/text-fields), [Toggles](https://developer.apple.com/design/human-interface-guidelines/toggles), [Pickers](https://developer.apple.com/design/human-interface-guidelines/pickers), [Sliders](https://developer.apple.com/design/human-interface-guidelines/sliders), [Steppers](https://developer.apple.com/design/human-interface-guidelines/steppers), [Segmented controls](https://developer.apple.com/design/human-interface-guidelines/segmented-controls), [Color wells](https://developer.apple.com/design/human-interface-guidelines/color-wells), [Combo boxes](https://developer.apple.com/design/human-interface-guidelines/combo-boxes), [Digit entry views](https://developer.apple.com/design/human-interface-guidelines/digit-entry-views), [Image wells](https://developer.apple.com/design/human-interface-guidelines/image-wells), [Virtual keyboards](https://developer.apple.com/design/human-interface-guidelines/virtual-keyboards)

Form-level guidance (validation timing, keyboard avoidance, autofill) is in [patterns/data-entry-search-settings.md](../patterns/data-entry-search-settings.md).

## Contents

- [Picking the right control](#picking-the-right-control)
- [Text fields](#text-fields)
- [Toggles, checkboxes, radio buttons](#toggles-checkboxes-radio-buttons)
- [Pickers](#pickers)
- [Sliders and steppers](#sliders-and-steppers)
- [Segmented controls](#segmented-controls)
- [Color wells, combo boxes, digit entry, image wells](#color-wells-combo-boxes-digit-entry-image-wells)
- [Virtual keyboards](#virtual-keyboards)
- [React Native mapping](#react-native-mapping)
- [Review checklist](#review-checklist)

## Picking the right control

| Control | For |
|---|---|
| **Text field** | A small amount of text — a name, an email address |
| **Text view** | Larger amounts of text |
| **Toggle / switch** | Two **opposing** values affecting state |
| **Pop-up button** | Choosing one from a flat list of mutually exclusive options |
| **Pull-down button** | A short list of **actions** |
| **Picker** | Medium-to-long lists of items |
| **List or table** | Very large sets (adjustable height, and tables can have an index) |
| **Radio buttons** (macOS) | More than two mutually exclusive options needing individual labels |
| **Segmented control** | Closely related choices affecting an object, state, or view |
| **Slider** | A continuous value across a range |
| **Stepper** | Small incremental changes |
| **Combo box** (macOS) | A list of likely choices **plus** a custom value |

The picker boundary is worth remembering: too short and a picker adds visual weight where a pull-down button would do; too long and a list or table is faster because it can be indexed.

## Text fields

**Show a hint via placeholder text** — "Email", "Password". Because the placeholder disappears on typing, **also include a separate label** to keep the purpose visible.

**Use secure text fields for sensitive data.**

**Match field size to expected input length.** Size is how people gauge how much to type.

**Space multiple fields evenly**, with enough room to see which label belongs to which field. **Stack vertically** where possible, and use **consistent widths** — first/last name at one width, address/city at another.

**Make tab order flow logically.** The system usually gets this right; verify it.

**Validate at the right moment**, which differs by field: an **email address** validates when people move to another field; a **user name or password** validates **before** they leave the field.

**Use a number formatter for numeric data** — it restricts input to numerals and formats as decimals, percentage, or currency. **Don't assume the presentation**, since formatting varies significantly by locale.

**Adjust line breaks to the field's needs.** The default clips overflowing text; you can wrap at character or word level, or truncate at the beginning, middle, or end.

**Consider an expansion tooltip** (macOS) to show the full value of clipped text on hover.

**Show the right keyboard type** on iOS, iPadOS, tvOS, visionOS.

**Minimize text entry on tvOS and watchOS** — gather information with buttons instead where you can.

### iOS/iPadOS

**Display a Clear button at the trailing end** so people don't hold down Delete.

**Use the ends purposefully:** the **leading** end indicates the field's purpose, the **trailing** end offers additional features like a Bookmarks button.

## Toggles, checkboxes, radio buttons

**A toggle is for two opposing values that affect state.** If you need other kinds of action — choosing from a list — use a different control.

**Identify what the toggle affects.** Usually the surrounding context is enough; on macOS you may add a label describing the state it controls. A button behaving like a toggle should use an icon that communicates its purpose plus a changing appearance (typically the background) for state.

**Make the state difference obvious and not color-only.** Add or remove a color fill, show or hide a background shape, change inner details like a checkmark or dot. Not everyone perceives color differences.

### iOS/iPadOS

**Use the switch style only in a list row** — the row content provides the context, so no label is needed.

**Change the default green only if necessary.** Your accent color is fine, but it must have enough contrast against the off state to be perceptible.

**Outside a list, use a toggle-behaving button, not a switch.** Phone's recent-calls filter button adds a blue highlight when active and removes it when inactive. **Don't label it** — the icon plus the alternate background communicates the purpose.

### macOS

**Put switches, checkboxes, and radio buttons in the window body, not the frame** — never in a toolbar or status bar.

**Switch vs. checkbox:** a switch has more visual weight, so use it for settings you want to emphasize, or for one controlling a group of settings. **Don't replace an existing checkbox with a switch.**

**Use a checkbox for a hierarchy of settings.** Checkboxes align and indent well, so a parent checkbox governing subordinate ones reads clearly. A switch can't express that.

**In a grouped form, a mini switch** matches the height of buttons and other controls, keeping rows consistent. For a hierarchy in a grouped form: regular switch for the primary setting, mini switches for subordinates.

**Use radio buttons for more than two mutually exclusive options** needing unique labels.

**Introduce a group of checkboxes with a label** if their relationship isn't clear, aligning the label's baseline with the first checkbox.

## Pickers

**Use predictable, logically ordered values.** Most values are hidden before interaction, so people need to predict what's there — an alphabetized country list lets them move quickly.

**Don't switch views to show a picker.** It belongs in context, below or near the field being edited — typically at the bottom of a window or in a popover.

**Reduce minute granularity in date pickers** where appropriate. The default is 0–59; use any interval that divides evenly into 60 (quarter hours: 0, 15, 30, 45).

### Platform notes

- **iOS/iPadOS** — the **compact** date picker shows a button with the current value in your accent color; tapping opens a modal with the familiar calendar and time editor, where people can make multiple edits before tapping outside to confirm.
- **macOS** — **textual** style for limited space and specific selections; **graphical** style for browsing days, selecting ranges, or when a clock face suits the app.

## Sliders and steppers

**Customize a slider's appearance when it adds value** — track color, thumb image and tint, and leading/trailing icons. A size slider might show a small image icon leading and a large one trailing.

**Use familiar directions.** Minimum leading, maximum trailing (horizontal); minimum bottom, maximum top (vertical). Reversing this is disorienting regardless of how the data is modeled.

**Consider pairing a slider with a text field and stepper** for wide ranges — the field shows and accepts an exact value, the stepper increments whole values.

**Make the value a stepper affects obvious** — a stepper displays nothing itself.

**Pair a stepper with a text field when large changes are likely.** Steppers alone suit small changes of a few taps.

### Platform notes

- **iOS/iPadOS** — **never use a slider for audio volume.** Use a volume view, which includes the level slider *and* the output device control.
- **macOS** — **give live feedback** as the value changes (Dock icons scale in real time with the Size slider). **Horizontal** for a fixed start and end (opacity 0–100%); **circular** for repeating or indefinite values (rotation 0–360°, or spin counts where four rotations = 1440°). Introduce with a label using **sentence-style capitalization ending in a colon**. **Tick marks** clarify scale and help locate values; **labels on tick marks** add more — usually only minimum and maximum are needed, with periodic labels for nonlinear scales. Provide a **tooltip showing the thumb's value** on hover. For large ranges, support **Shift-click** on a stepper to change by a larger increment (10× the default).
- **visionOS** — **prefer horizontal sliders**; side-to-side gestures are easier than up-and-down.
- **watchOS** — the system shows plus and minus signs by default; create custom glyphs if they communicate the purpose better.

## Segmented controls

**For closely related choices affecting an object, state, or view.**

**Good when grouping matters or selection state must be visible.** Unlike other button styles, segmented controls **preserve their grouping** regardless of view size or placement.

**Keep control types consistent within one control.** Don't mix actions into a control that otherwise represents selection state, or show selection state in one that performs actions. This is the most common segmented control error and it makes the control's meaning unreadable.

**Limit segments:** about **five to seven** in a wide interface, no more than **five on iPhone**.

**Keep segment size consistent**, and keep icon and title widths similar.

**Use either text or images, not both** in one control — mixing produces a disconnected, confusing result.

**Use similar-sized content per segment**, since equal widths make uneven fill look wrong.

**Label segments with nouns or noun phrases, title-style capitalization.** A text-labeled segmented control needs no introductory text.

### Platform notes

- **iOS/iPadOS** — good for switching between closely related **subviews** (Calendar's New Event sheet switches between event and reminder). For **completely separate sections**, use a tab bar.
- **macOS** — introductory text can clarify purpose; with symbols, consider a label below each segment, and provide a **tooltip per segment**. **Use a tab view in the main window area** for view switching, not a segmented control — reserve segmented controls for a toolbar or inspector pane. Consider **spring loading**.
- **tvOS** — for content filtering, **prefer a split view**; people navigate between content and filter options more easily, and a segmented control may be awkward to reach depending on placement.

## Color wells, combo boxes, digit entry, image wells

**Color wells:** prefer the **system color picker** — consistent experience, and it lets people save a color set accessible from any app, including across iOS, iPadOS, and macOS.

**Combo boxes (macOS):** populate with a **meaningful default value from the list** (not necessarily the first item) that hints at the hidden choices. Use an **introductory label** with title-style capitalization ending in a colon. Offer **relevant choices** alongside the ability to type a custom value. **Keep list items no wider than the text field**, or they'll truncate.

**Digit entry views (tvOS):** use **secure** digit fields (asterisks) for sensitive data. **State the purpose** with a title and prompt explaining why digits are needed.

**Image wells (macOS):** **revert to the default image** if the well requires one and people clear it. If you support copy and paste, **make the standard Edit menu items and keyboard shortcuts available** — people expect them.

## Virtual keyboards

**Match the keyboard to the content.** Specifying a semantic meaning for the input area lets the system pick the right keyboard *and* refine its corrections.

**Customize the Return key type** when it clarifies the experience — a search Return key for a field that initiates search.

**A custom input view must make obvious sense**, or people will wonder why they can't get the system keyboard back.

**Play the standard keyboard sound** in a custom input view — people expect it, and they can disable it globally in Settings › Sounds.

**Provide an obvious way to switch keyboards** — people know the Globe key does this and expect an equivalent.

**Don't duplicate system keyboard features.** The Emoji/Globe and Dictation keys appear beneath the keyboard on some devices even for custom keyboards; you can't affect them, and repeating them confuses.

**Put keyboard tutorials in your app, not in the keyboard.**

### iOS/iPadOS

**Use the keyboard layout guide** so the keyboard feels integrated and important interface parts stay visible.

**Place custom controls above the keyboard thoughtfully.** An input accessory view should hold controls **relevant to the current task**. If other views use Liquid Glass, or the view looks out of place above the keyboard, **apply Liquid Glass** to the container — a standard toolbar adopts it automatically. Use the keyboard layout guide and standard padding so positioning matches expectations.

## React Native mapping

### Text fields

See [patterns/data-entry-search-settings.md](../patterns/data-entry-search-settings.md) for keyboard type, autofill, and validation props. Component-level specifics:

```jsx
<View>
  {/* Persistent label — the placeholder disappears on typing. */}
  <Text style={textStyles.footnote} nativeID="emailLabel">Email</Text>
  <TextInput
    accessibilityLabelledBy="emailLabel"
    placeholder="name@example.com"
    // iOS Clear button.
    clearButtonMode="while-editing"
    // Size hints at expected length.
    style={[s.field, { width: 240 }]}
    // Leading affordance indicates purpose; trailing offers extra features.
  />
</View>
```

Validation timing differs per field type, matching the HIG rule:

```js
// Email → validate on blur.
<TextInput onBlur={() => validateEmail(value)} />
// Password/username → validate as they type, before they can leave.
<TextInput onChangeText={t => { setValue(t); setError(validatePassword(t)); }} />
```

### Toggles

```jsx
import { Switch } from 'react-native';

// Switch only inside a list row; the row label is the context.
<ListRow title="Wi-Fi Assist">
  <Switch
    value={enabled}
    onValueChange={setEnabled}
    // Default green usually correct; only override with adequate contrast.
    trackColor={{ true: PlatformColor('systemGreen'), false: PlatformColor('systemGray5') }}
    accessibilityRole="switch"
    accessibilityState={{ checked: enabled }}
  />
</ListRow>
```

Outside a list, use a toggling button — not a switch:

```jsx
// Phone's filter button pattern: icon + background change, no label.
<Pressable
  onPress={() => setFiltered(f => !f)}
  accessibilityRole="button"
  accessibilityState={{ selected: filtered }}
  accessibilityLabel="Filter to missed calls"
  style={[s.iconButton, filtered && { backgroundColor: PlatformColor('systemBlue') }]}
>
  <Icon name="line.3.horizontal.decrease" color={filtered ? '#fff' : PlatformColor('label')} />
</Pressable>
```

Note `accessibilityRole="switch"` vs `"button"` with `selected` — VoiceOver announces these differently, and the distinction should match which control you chose.

State must not be color-only:

```jsx
// Add a shape or glyph change alongside the color.
{filtered && <Icon name="checkmark" size={10} />}
```

### Pickers

```jsx
import DateTimePicker from '@react-native-community/datetimepicker';

// Compact style = the iOS accent-colored button opening a modal editor.
<DateTimePicker
  value={date}
  mode="datetime"
  display="compact"
  minuteInterval={15}       // divides evenly into 60, per the guidance
  onChange={(_, d) => d && setDate(d)}
/>
```

Keep the picker in context rather than pushing a screen for it:

```jsx
// Wrong: navigation.navigate('DatePickerScreen')
// Right: inline below the field, or in a popover/bottom sheet anchored to it.
```

For long lists, prefer a searchable list over a wheel picker:

```jsx
// A country picker is a list, not a <Picker> — indexable and searchable.
<SectionList sections={countriesByLetter} ListHeaderComponent={<SearchField />} />
```

### Sliders

```jsx
import Slider from '@react-native-community/slider';

<Slider
  minimumValue={0}
  maximumValue={100}
  value={opacity}
  onValueChange={setOpacity}
  // Minimum leading, maximum trailing — never inverted.
  minimumTrackTintColor={PlatformColor('systemBlue')}
  // Leading/trailing icons communicating the dimension.
  // Accessibility: this is the difference between usable and not.
  accessibilityRole="adjustable"
  accessibilityLabel="Opacity"
  accessibilityValue={{ min: 0, max: 100, now: Math.round(opacity), text: `${Math.round(opacity)} percent` }}
  accessibilityIncrements={['1 percent', '5 percent']}
/>
```

Pair with a field and stepper for wide ranges:

```jsx
<View style={{ flexDirection: 'row', alignItems: 'center', gap: 12 }}>
  <Slider style={{ flex: 1 }} … />
  <TextInput value={String(value)} keyboardType="number-pad" style={{ width: 56 }} />
  <Stepper value={value} onChange={setValue} />
</View>
```

Under RTL, mirror the slider (it represents progress) — see [layout.md](../foundations/layout.md).

Never use a slider for volume; use the system volume view (`react-native-volume-manager` or an `MPVolumeView` wrapper) so the output picker comes with it.

### Segmented controls

```jsx
import SegmentedControl from '@react-native-segmented-control/segmented-control';

// ≤5 segments on iPhone. Text OR icons, never both. Nouns, title case.
<SegmentedControl
  values={['Day', 'Week', 'Month']}
  selectedIndex={index}
  onChange={e => setIndex(e.nativeEvent.selectedSegmentIndex)}
/>
```

This wraps `UISegmentedControl`, so it gets the correct Liquid Glass appearance and selection haptic. A hand-rolled row of `Pressable`s does not, and it's visually obvious.

Don't mix selection and action segments:

```jsx
// Wrong: ['Day', 'Week', 'Export']  — Export is an action among selections.
// Right: keep Export in the toolbar.
```

### Keyboards

```jsx
// Return key type clarifies what submitting does.
<TextInput returnKeyType="search" onSubmitEditing={runSearch} />

// Input accessory view — relevant to the current task only.
<InputAccessoryView nativeID="formatBar">
  {/* A standard toolbar adopts Liquid Glass; a custom view needs it applied. */}
  <BlurView intensity={60} style={s.accessoryBar}>
    <Icon name="bold" /><Icon name="italic" /><Icon name="underline" />
  </BlurView>
</InputAccessoryView>

<TextInput inputAccessoryViewID="formatBar" />
```

`InputAccessoryView` is iOS-only; on Android position the bar with `react-native-keyboard-controller`'s keyboard height so it tracks the keyboard rather than jumping.

Avoid custom keyboards in RN entirely unless the app is a keyboard — a `KeyboardAvoidingView` plus the right `keyboardType` covers essentially every case, and a custom input view that people can't escape is a real usability failure.

### Color wells

```js
// No RN binding for UIColorPickerViewController; react-native-color-picker
// and similar are JS implementations. If color choice is central, wrap the
// system picker natively so people get their saved colors.
```

## Review checklist

- [ ] Control choice matches the data: field vs. text view, toggle vs. pop-up button, picker vs. list.
- [ ] Text fields have a persistent label in addition to a placeholder.
- [ ] Field widths hint at expected input length; sibling fields share consistent widths and spacing.
- [ ] Tab/next order is logical; last field submits.
- [ ] Validation timing matches field type (email on blur, credentials before leaving).
- [ ] Numeric fields use a locale-aware formatter.
- [ ] iOS text fields offer a Clear button; leading/trailing affordances used purposefully.
- [ ] Switches appear only in list rows; toggling buttons used elsewhere, unlabelled with icon + background state.
- [ ] Toggle state distinguishable without color; `accessibilityRole` matches the control type.
- [ ] Pickers appear in context, never on a pushed screen; values predictably ordered.
- [ ] Date pickers use a sensible `minuteInterval` dividing into 60.
- [ ] Sliders run minimum→maximum leading→trailing (bottom→top vertical) and mirror under RTL.
- [ ] Sliders have `accessibilityRole="adjustable"` with min/max/now values.
- [ ] Wide-range sliders paired with a text field and stepper.
- [ ] No slider used for audio volume.
- [ ] Steppers make the affected value obvious.
- [ ] Segmented controls hold ≤ 5 segments on iPhone (≤ 7 wide), consistent widths, text **or** icons, noun labels.
- [ ] No action segments mixed into a selection segmented control; native implementation used.
- [ ] Segmented controls used for subviews; tab bars for separate sections.
- [ ] Keyboard type and Return key type set per field.
- [ ] Input accessory views hold task-relevant controls and adopt Liquid Glass.
- [ ] No custom input view unless the app is a keyboard; standard keyboard sound played if there is one.
- [ ] Color selection uses the system picker where color is central.
- [ ] macOS: switches/checkboxes/radios in the window body; checkboxes for setting hierarchies; sliders labelled, tick-marked, and live-updating.
- [ ] tvOS/watchOS: text entry minimized; digit entry secured where sensitive.
