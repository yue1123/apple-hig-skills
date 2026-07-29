# Data Entry, Search, Settings & Accounts

Source: HIG › Patterns › [Entering data](https://developer.apple.com/design/human-interface-guidelines/entering-data), [Searching](https://developer.apple.com/design/human-interface-guidelines/searching), [Settings](https://developer.apple.com/design/human-interface-guidelines/settings), [Managing accounts](https://developer.apple.com/design/human-interface-guidelines/managing-accounts)

Read this when building forms, search, a settings screen, or sign-in/sign-up and account deletion.

## Contents

- [Entering data](#entering-data)
- [Search](#search)
- [Settings](#settings)
- [Accounts and sign-in](#accounts-and-sign-in)
- [Account deletion](#account-deletion)
- [Platform differences](#platform-differences)
- [React Native mapping](#react-native-mapping)
- [Review checklist](#review-checklist)

## Entering data

The two structural moves: **pre-gather as much as possible** so people supply less, and **support every available input method** so they can pick.

**Get it from the system rather than asking.** Anything available from settings, or via permission (location, calendar), shouldn't be a text field.

**Be explicit about what you need.** A placeholder showing the format (`username@company.com`) or an introductory label ("Email"). Prefill reasonable defaults — it removes a decision as well as typing.

**Use secure entry for sensitive data** — obscured characters. On tvOS, digit entry views can obscure numerals. On visionOS, system text fields show entered data to the wearer only, and secure fields **automatically blur when AirPlaying**.

**Never prepopulate a password field.** Always require entry, biometric, or keychain authentication.

**Offer choices instead of text entry where you can.** Picking from a list beats typing even with a keyboard right there — pickers, menus, selection components.

**Support drag-and-drop and paste.** Both ease entry and make the app feel part of the system.

**Validate dynamically, as people type.** Making someone fix mistakes after completing a long form is where forms get abandoned. Verify on entry and give feedback immediately. For numbers, use a formatter — it restricts input to numerals and can render as percentage, currency, or fixed decimals.

**Make required data unmistakable.** If there's a Next or Continue button after a field set, keep it unavailable until the required data is present. That's clearer than letting people press it and then explaining what's missing.

## Search

**Give search a primary position if it's important.** Notes puts a search field in the bottom toolbar alongside other key actions; Photos and Apple TV give search a dedicated tab.

**Aim for a single searchable location.** One clearly identified place to find anything. Apps with genuinely distinct sections may add local search too — iOS Music uses search as a filter on the current view.

**Display the current scope clearly** — descriptive placeholder text, scope bars, tokens, or a title. Mail always shows which mailbox is being searched. Ambiguous scope is why people conclude "search doesn't find my stuff".

**Provide suggestions.** Recent searches before typing; predictive suggestions during typing. Both mean less typing and faster results.

**Consider privacy before showing search history.** People may not want their history visible to whoever's nearby. If you show it, provide a way to clear it.

**Index content in Spotlight** with descriptive metadata so system-wide search finds it. Define metadata for custom file types via a Spotlight File Importer; implement a **Quick Look generator** so Spotlight and other apps can preview your documents. Prefer the system open/save views — they include search across the whole system.

## Settings

**Provide defaults that suit the largest number of people.** Auto-maximize performance for the current device rather than asking. Ideally people never open settings at all.

**Minimize the number of settings.** Too many makes the app feel unapproachable *and* makes any individual setting hard to find.

**Respect system-wide settings and never duplicate them.** Accessibility accommodations, scrolling behavior, authentication methods — people manage these in the Settings app and expect every app to obey. A custom in-app version implies the system setting might not apply to you, and that changing yours might affect other apps. Both implications are wrong and confusing.

**Don't use settings to collect setup information you could detect.** Detect the connected controller; detect Dark Mode.

**Make settings reachable the expected way** — ⌘, with a physical keyboard, Esc in a game.

### Where each kind of option belongs

- **Custom settings area** — general, **infrequently changed** options: window configuration, game-save behavior, keyboard mappings, account options. People must suspend their task to get here, so only put things here that don't need frequent change.
- **In the screen it affects** — **task-specific** options: showing/hiding parts of the current view, reordering items, filtering a list. Putting these in a settings area disconnects them from context, forces a task interruption, and usually hides the result until people come back.
- **System Settings app** — only the **most rarely changed** options. If you do add some, provide a button that opens it directly.

## Accounts and sign-in

**Explain the benefit of an account and how to sign up**, in the sign-in view itself, briefly and warmly.

**Delay sign-in as long as possible.** Forced sign-in before anything useful is a top cause of abandonment. Let people get a feel for the app first — a shopping app can allow unlimited browsing and require sign-in only at purchase.

**Prefer Sign in with Apple; otherwise prefer a passkey.** Passkeys remove password creation and entry entirely — people just supply a user name. If you keep passwords, add two-factor authentication.

**Name the authentication method in the button.** "Sign In with Face ID", not "Sign In".

**Only reference methods available on this device.** Don't say Face ID on a Touch ID device — check the biometry type.

**Don't offer an in-app toggle for biometric authentication.** People enable biometrics at the system level; an app-level switch is redundant and confusing.

**Never use the word *passcode*** for account authentication. A passcode unlocks the device or authenticates Apple services; using the term suggests you're asking people to reuse it in your app.

## Account deletion

**Provide a clear way to start account deletion inside the app.** If deletion can't happen in-app, provide a **direct link** to the page where it can — and make it easy to find. Burying it in the Privacy Policy or Terms is explicitly called out as not acceptable.

**Keep the in-app and web deletion flows consistent.** Don't make one longer or more complicated than the other.

**Consider scheduled deletion**, so people can use remaining services or wait out a renewal — but always also offer immediate deletion.

**Say when deletion will complete, and notify when it's done.** Deletion can take time; silence reads as failure.

**Explain billing and cancellation interaction with in-app purchases:**

- Auto-renewable subscription billing continues **through Apple until the subscription is cancelled**, regardless of account deletion.
- After deleting an account, people still need to cancel the subscription or request a refund.

You must support account deletion **even if the subscription wasn't purchased through your app**.

If people used Sign in with Apple, **revoke the associated tokens** on deletion.

If legal requirements force you to retain accounts or data (digital health records, say) or to follow a specific process, **describe the situation clearly** so people understand what's kept and why.

## Platform differences

### macOS

- **Settings item goes in the App menu.** Don't put a settings button in a window toolbar — it costs space needed for frequently used commands. Document-level options go in the **File menu**.
- **Dim the settings window's minimize and maximize buttons.** ⌘, reopens it instantly, so it doesn't need to live in the Dock, and it sizes to the current pane so expanding is pointless.
- **Use a non-customizable toolbar that stays visible and always shows the active button.** People rely on a stable settings layout to find things.
- **Update the window title to the visible pane.** Single-pane windows use *App Name* Settings.
- **Restore the last-viewed pane** — people usually adjust related settings more than once.
- **Expansion tooltips** can reveal the full value of clipped text in a small field (available to iOS/iPadOS apps running on Mac too).

### tvOS

- **Let people authenticate from another device.** With associated domains configured, Apple TV can suggest credentials from other devices, including Sign in with Apple.
- **Don't re-ask for profile selection on a shared account.** tvOS 16+ can share credentials across users while storing per-person profiles separately, so the current user's profile is used automatically.
- **Minimize data entry.** Beyond a small amount of information, send people to a website on another device. For an email address, show the email keyboard screen with its list of recent addresses.
- **TV provider accounts:** don't show a sign-out option when people are signed in at the system level — if you must, prompt them to go to Settings › TV Provider. **Never** tell people to sign out via Settings › Privacy; those controls manage which apps can access the account, they are not a sign-out mechanism.

## React Native mapping

### Field configuration is most of the work

The keyboard, autofill, and validation props are what make a form feel native, and they're the most commonly under-specified part of RN forms:

```jsx
<TextInput
  // Right keyboard, so people aren't hunting for @ or digits.
  keyboardType="email-address"
  // iOS autofill + password manager integration.
  textContentType="emailAddress"
  autoComplete="email"              // Android
  // Don't fight the content: emails aren't capitalized, names are.
  autoCapitalize="none"
  autoCorrect={false}
  // Format demonstration, per the HIG guidance.
  placeholder="username@company.com"
  // Return key advances the form rather than dismissing it.
  returnKeyType="next"
  onSubmitEditing={() => passwordRef.current?.focus()}
  // Content must scale — see typography.md.
  maxFontSizeMultiplier={2}
  // Labelled for VoiceOver even though the placeholder is visible.
  accessibilityLabel="Email address"
/>

<TextInput
  ref={passwordRef}
  secureTextEntry                        // obscured input
  textContentType="password"             // or "newPassword" on sign-up
  // Never set `value` from stored credentials — that's prepopulating a password field.
  autoComplete="current-password"
  passwordRules="minlength: 12; required: lower; required: upper; required: digit;"
  returnKeyType="go"
  onSubmitEditing={submit}
/>
```

`textContentType="newPassword"` plus `passwordRules` is what triggers iOS's Strong Password suggestion — worth having, since it makes the passkey-adjacent path (a managed strong password) the default rather than something people opt into.

One-time codes:

```jsx
<TextInput textContentType="oneTimeCode" autoComplete="sms-otp" keyboardType="number-pad" />
```

### Validate as you type, not on submit

```jsx
function useFieldValidation(value, validate) {
  const [touched, setTouched] = useState(false);
  const error = validate(value);
  // Validate live once touched — feedback while typing, not a wall of errors at the end.
  return { error: touched ? error : null, onBlur: () => setTouched(true), isValid: !error };
}

// Required data gates the action, per the HIG rule.
<Pressable disabled={!formIsValid} onPress={submit} accessibilityState={{ disabled: !formIsValid }}>
```

Show the error adjacent to its field, phrased as an instruction ("Use at least 12 characters"), never as a scold — see [typography.md](../foundations/typography.md) for the copy rules.

### Prefer pickers over free text

```jsx
// A date, a country, a category — these should never be TextInputs.
import DateTimePicker from '@react-native-community/datetimepicker';
import { Picker } from '@react-native-picker/picker';

// iOS 14+ inline/compact date picker, matching the platform component.
<DateTimePicker value={date} mode="date" display="compact" onChange={onChange} />
```

### Numeric formatting

```jsx
// Restrict and format, rather than validating free text afterwards.
function CurrencyInput({ value, onChange }) {
  return (
    <TextInput
      keyboardType="decimal-pad"
      value={formatCurrency(value)}      // Intl.NumberFormat — respects locale
      onChangeText={t => onChange(parseCurrency(t))}
    />
  );
}
```

Use `Intl.NumberFormat` / `Intl.DateTimeFormat` with the device locale rather than hand-rolled formatting — that's the locale-adaptivity requirement from [layout.md](../foundations/layout.md).

### Keyboard handling

```jsx
// Fields must not be covered by the keyboard.
<KeyboardAvoidingView behavior={Platform.OS === 'ios' ? 'padding' : 'height'} style={{ flex: 1 }}>
  <ScrollView
    keyboardShouldPersistTaps="handled"     // taps on buttons work with keyboard open
    keyboardDismissMode="interactive"       // iOS: drag the keyboard down
    contentInsetAdjustmentBehavior="automatic"
  >
    {fields}
  </ScrollView>
</KeyboardAvoidingView>
```

`react-native-keyboard-controller` handles this considerably better than `KeyboardAvoidingView` if the form is non-trivial — `KeyboardAvoidingView` is a frequent source of jumpy layouts on Android.

### Search

```jsx
// Native-stack exposes the real UISearchController — the search field then behaves
// like the system one (scroll-to-reveal, cancel button, scope bar).
<Stack.Screen
  name="Library"
  options={{
    headerSearchBarOptions: {
      placeholder: 'Songs, Albums, Artists',   // states the scope
      onChangeText: e => setQuery(e.nativeEvent.text),
      hideWhenScrolling: false,
      autoCapitalize: 'none',
    },
  }}
/>
```

A hand-built `<TextInput>` in a header is where search stops feeling native — no scroll-reveal, no system cancel behavior, wrong keyboard dismissal.

Recents and suggestions:

```jsx
// Before typing: recents (with a clear affordance, for privacy).
// While typing: debounced suggestions.
{query === '' ? (
  <RecentSearches items={recents} onClear={clearRecents} />
) : (
  <Suggestions items={suggestions} />
)}
```

Debounce suggestion requests (~250 ms) and cancel in-flight ones, or fast typists get results for a prefix they've already moved past.

Spotlight indexing needs a native module (`react-native-spotlight-search` or a custom Expo module) — there's no JS API. Worth it if your content is the kind people look for outside the app.

### Settings

```jsx
// Task-specific options live in the screen they affect...
<ListHeader>
  <SortMenu value={sort} onChange={setSort} />
  <FilterMenu value={filter} onChange={setFilter} />
</ListHeader>

// ...and only rarely changed ones go to Settings.
// Do NOT add a theme toggle here — follow the system appearance.
// Do NOT add a "large text" or "reduce motion" toggle — those are system settings.
<SettingsSection title="Account">
  <Row title="Manage Subscription" onPress={() => Linking.openURL('itms-apps://apps.apple.com/account/subscriptions')} />
  <Row title="Delete Account" destructive onPress={confirmDeletion} />
</SettingsSection>

// Deep link to system settings rather than duplicating them.
<Row title="Notification Settings" onPress={() => Linking.openSettings()} />
```

### Sign-in

```jsx
import * as AppleAuthentication from 'expo-apple-authentication';

// Sign in with Apple first on Apple platforms — and it's an App Store requirement
// if you offer other third-party sign-in options.
<AppleAuthentication.AppleAuthenticationButton
  buttonType={AppleAuthentication.AppleAuthenticationButtonType.SIGN_IN}
  buttonStyle={AppleAuthentication.AppleAuthenticationButtonStyle.BLACK}
  cornerRadius={8}
  onPress={signInWithApple}
/>
```

Name the biometric method correctly, and only when available:

```jsx
import * as LocalAuthentication from 'expo-local-authentication';

const [label, setLabel] = useState('Sign In');
useEffect(() => {
  LocalAuthentication.supportedAuthenticationTypesAsync().then(types => {
    if (types.includes(LocalAuthentication.AuthenticationType.FACIAL_RECOGNITION)) setLabel('Sign In with Face ID');
    else if (types.includes(LocalAuthentication.AuthenticationType.FINGERPRINT)) setLabel('Sign In with Touch ID');
  });
}, []);
```

Don't add a "Use Face ID" switch in your settings — the system setting governs.

Defer the sign-in wall:

```jsx
// Browse freely; require auth only at the committing action.
function onCheckout() {
  if (!session) return navigation.navigate('SignIn', { returnTo: 'Checkout', reason: 'to place your order' });
  proceed();
}
```

Passing `reason` lets the sign-in screen state the benefit in context, which is the "explain why an account is needed" rule.

### Account deletion

```jsx
// Discoverable — a top-level Settings row, not buried in a policy screen.
async function confirmDeletion() {
  Alert.alert(
    'Delete Account?',
    'Your data will be removed within 30 days. Any active subscription must be cancelled separately in the App Store — deleting your account does not stop billing.',
    [
      { text: 'Cancel', style: 'cancel' },
      { text: 'Delete Account', style: 'destructive', onPress: startDeletion },
    ],
  );
}

// If people signed in with Apple, revoke the token server-side on deletion.
// expo-apple-authentication exposes the credential; revocation is a REST call
// your backend makes to Apple.
```

Tell people the completion timeline in the confirmation, and notify when it finishes — silence during a multi-day deletion is what generates support tickets.

## Review checklist

- [ ] No field asks for data obtainable from the system or by permission.
- [ ] Every `TextInput` sets `keyboardType`, `textContentType`/`autoComplete`, `autoCapitalize`, `autoCorrect`.
- [ ] Placeholders demonstrate the expected format; every field has a visible label and an `accessibilityLabel`.
- [ ] `returnKeyType` advances through the form; last field submits.
- [ ] Password fields use `secureTextEntry` and are never prepopulated; sign-up uses `newPassword` + `passwordRules`.
- [ ] OTP fields use `oneTimeCode` / `sms-otp`.
- [ ] Validation runs as people type; errors sit next to their field, phrased as instructions.
- [ ] Submit action disabled (with `accessibilityState`) until required data is present.
- [ ] Pickers/menus used instead of text entry for constrained values; dates use the native picker.
- [ ] Numbers and dates formatted via `Intl` with the device locale.
- [ ] Keyboard never covers the focused field; taps work with the keyboard open.
- [ ] Paste and drag-and-drop supported where sensible.
- [ ] Search uses the native search bar; scope is stated in placeholder or title.
- [ ] Recents shown before typing with a clear-history affordance; suggestions debounced and cancellable.
- [ ] Task-specific options live in the screen they affect, not in Settings.
- [ ] No in-app duplicates of system settings (theme, text size, reduce motion, biometrics).
- [ ] Settings deep-link to the system Settings app where relevant.
- [ ] Settings count kept small; defaults good enough to skip settings entirely.
- [ ] Sign-in deferred until a committing action; benefit explained in context.
- [ ] Sign in with Apple offered on Apple platforms; passkeys preferred over passwords otherwise.
- [ ] Auth buttons name the actual available method (checked at runtime).
- [ ] The word "passcode" not used for account auth.
- [ ] Account deletion reachable from a top-level settings row or a direct link — not buried in policy text.
- [ ] Deletion flow explains subscription billing continuation and cancellation separately.
- [ ] Deletion timeline stated and completion notified; Sign in with Apple tokens revoked.
- [ ] macOS: settings in the App menu with ⌘,; stable non-customizable toolbar; last pane restored.
- [ ] tvOS: minimal data entry, cross-device authentication, no sign-out shown for system-level TV provider accounts.
