# Privacy & Permissions

Source: HIG › Foundations › [Privacy](https://developer.apple.com/design/human-interface-guidelines/privacy)

Read this before requesting any permission, storing credentials, or building sign-in. Several of these rules are App Store review criteria, not just style guidance.

## Contents

- [Rules that matter most](#rules-that-matter-most)
- [What requires permission](#what-requires-permission)
- [When to ask](#when-to-ask)
- [Purpose strings](#purpose-strings)
- [Pre-alert screens](#pre-alert-screens)
- [The location button](#the-location-button)
- [Protecting data](#protecting-data)
- [Platform differences](#platform-differences)
- [React Native mapping](#react-native-mapping)
- [Review checklist](#review-checklist)

## Rules that matter most

**Request only the data you actually need, as specifically as possible.** Asking for more than a feature requires — or asking before someone has shown interest in that feature — costs trust immediately and permanently.

**Be transparent about collection and use.** People withhold data they don't understand the use of. Respect system privacy features like Hide My Email and Mail Privacy Protection rather than working around them.

**Process on device where you can.** The Neural Engine and Core ML models let you avoid round trips that are both slow and a liability.

**Adopt system-defined protections** rather than rolling your own — CloudKit, for example, provides encryption and key management for strings, numbers, and dates.

## What requires permission

- **Personal data** — location, health, financial, contact, and other personally identifying information
- **User-generated content** — email, messages, calendar, contacts, gameplay information, Apple Music activity, HomeKit data, audio/video/photo content
- **Protected resources** — Bluetooth peripherals, home automation, Wi-Fi connections, local networks
- **Device capabilities** — camera, microphone
- **visionOS ARKit data in a Full Space** — hand tracking, plane estimation, image anchoring, world tracking
- **The advertising identifier** (app tracking)

## When to ask

**Ask at the moment the feature needs it**, not before. A permission prompt that arrives with no visible cause reads as suspicious regardless of your intentions.

**Don't request at launch unless the app genuinely can't function without it.** Launch-time requests are tolerable only when the reason is self-evident — a navigation app needing location, or a visionOS game that bounces objects off the player's real walls needing surroundings data. Anything else at launch reads as a data grab.

## Purpose strings

The system alert shows your copy between your app name and the allow/deny buttons. Requirements:

- A **brief, complete sentence**
- Straightforward, specific, easy to understand
- **Sentence case**
- **Active voice**
- **Ends with a period**

The purpose string is the only place you get to make your case, and it's shown at the exact moment of decision. "Access to your photos" is not a reason. "Lets you attach photos to a repair request." is.

## Pre-alert screens

You may show a custom screen before the system alert, but it's tightly constrained — and violations here get apps **rejected by App Store review**, because the whole category exists to prevent manipulation.

**Include exactly one button, and make clear that it opens the system alert.** Title it **"Continue"** or **"Next"** — never **"Allow"**. A custom button that looks and reads like the alert's Allow button trains people into tapping through without meaning to, which is the manipulation being prohibited.

**Don't include any other action.** No close, no cancel, no way to leave without seeing the system alert. A second button diverts people from making the actual choice.

**Never mislead or confuse.** Apple is explicit that messaging designed to exploit people's habit of dismissing alerts quickly will be rejected.

## The location button

A lightweight alternative to the full permission flow for one-off location sharing — attaching location to a post, finding a store, identifying a plant or building.

Behavior worth knowing: **with no authorization status, tapping it behaves like choosing *Allow Once***. If someone previously chose *While Using the App*, tapping it doesn't change the status. So it's a good fit when your analytics show people frequently granting *Allow Once* — it gives them the benefit without a repeated alert.

You can customize it: pick the system title that fits ("Current Location", "Share My Current Location"), choose the filled or outlined glyph, set background and title/glyph colors, adjust corner radius.

**But if the system detects consistent problems with your customization, it stops granting location access on tap.** The button will still fire your action, and people will lose trust when it silently doesn't work. Keep customization within legibility — this is a case where "harmonize with my UI" has a hard limit enforced by the OS.

## Protecting data

**Don't rely on passwords alone.** Prefer **passkeys**. If you keep passwords, add two-factor authentication. For apps people stay logged into, gate re-entry with **Face ID / Optic ID / Touch ID**.

**Store sensitive information in the keychain.** Never in plain-text files — file permissions are not a substitute for encryption.

**Don't invent custom authentication schemes.** Use passkeys, Sign in with Apple, or Password AutoFill.

## Platform differences

### macOS

- **Sign with a valid Developer ID** if distributing outside the App Store.
- **Sandbox the app.** Required for the Mac App Store, and it limits blast radius from malware.
- **Don't assume who is signed in.** Fast user switching means multiple people can be active on one system simultaneously — so per-user state must be keyed to the actual user, not to the machine.

### visionOS

ARKit data in a Full Space (hand tracking, plane estimation, image anchoring, world tracking) is permission-gated. This is data about someone's *home*, so the "ask at the moment of need with a specific reason" rule matters more here than anywhere else.

## React Native mapping

### Purpose strings live in native config

RN cannot set these at runtime — they're `Info.plist` keys, and a missing one crashes the app on first request rather than showing a prompt.

```js
// Expo — app.json / app.config.js
{
  "expo": {
    "ios": {
      "infoPlist": {
        "NSCameraUsageDescription": "Lets you take a photo of the item you're reporting.",
        "NSPhotoLibraryUsageDescription": "Lets you attach existing photos to a repair request.",
        "NSLocationWhenInUseUsageDescription": "Shows repair shops near you.",
        "NSMicrophoneUsageDescription": "Lets you record a voice note describing the problem.",
        "NSFaceIDUsageDescription": "Unlocks the app without typing your password."
      }
    }
  }
}
```

Bare RN: the same keys in `ios/<App>/Info.plist`. Note each string follows the HIG format — sentence case, active voice, terminal period, and it names the *benefit*, not the data.

Android needs the parallel declarations in `AndroidManifest.xml`, but Android has no purpose-string equivalent, so the explanatory burden shifts to your own pre-permission UI.

### Request at point of need

```jsx
// Wrong: permission on mount.
useEffect(() => { requestCameraPermission(); }, []);

// Right: permission when the user reaches for the feature.
async function onAttachPhoto() {
  const { status } = await ImagePicker.requestCameraPermissionsAsync();
  if (status !== 'granted') {
    // Denied — degrade, don't dead-end. Offer the library, or manual entry.
    return showPhotoLibraryFallback();
  }
  launchCamera();
}
```

Always design the denied path. A screen that only works with permission granted is a screen some people simply cannot use.

### Checking before asking

```js
import * as ImagePicker from 'expo-image-picker';

// getXPermissionsAsync() reads status without prompting — use it to decide
// whether to show your own explanation first.
const { status, canAskAgain } = await ImagePicker.getCameraPermissionsAsync();

if (status === 'denied' && !canAskAgain) {
  // iOS only prompts once. After that, the only path is Settings.
  Linking.openSettings();
}
```

`canAskAgain === false` is the state most RN apps handle badly: re-calling `request…Async()` resolves immediately as denied with no visible prompt, which looks like a broken button. Route to Settings instead, and say so.

### Pre-permission screens, done legally

```jsx
// Exactly one button, titled Continue, no escape hatch, no "Allow".
function CameraExplainer({ onContinue }) {
  return (
    <View style={s.sheet}>
      <Icon name="camera" size={48} />
      <Text style={textStyles.title2}>Photos help us diagnose faster</Text>
      <Text style={textStyles.body}>
        A picture of the damage lets a technician quote without a site visit.
      </Text>
      {/* "Continue" — not "Allow", and no Cancel/Close/Skip alongside it. */}
      <Pressable onPress={onContinue} accessibilityRole="button">
        <Text>Continue</Text>
      </Pressable>
    </View>
  );
}
```

If you want to give people a way out, put it *before* this screen — the decision to open the explainer at all. Once the explainer is up, one button.

### Credentials

```js
// Keychain-backed storage — never AsyncStorage for secrets.
// AsyncStorage is unencrypted plain text on disk; it is exactly what the
// "never store passwords in plain-text files" rule prohibits.
import * as SecureStore from 'expo-secure-store';
await SecureStore.setItemAsync('refresh_token', token, {
  keychainAccessible: SecureStore.WHEN_UNLOCKED_THIS_DEVICE_ONLY,
});

// Or react-native-keychain in bare RN.
import * as Keychain from 'react-native-keychain';
await Keychain.setGenericPassword(username, token, {
  accessible: Keychain.ACCESSIBLE.WHEN_UNLOCKED_THIS_DEVICE_ONLY,
  accessControl: Keychain.ACCESS_CONTROL.BIOMETRY_CURRENT_SET,   // Face ID / Touch ID gate
});
```

```js
// Biometric re-entry for a stay-logged-in app.
import * as LocalAuthentication from 'expo-local-authentication';

const hasHardware = await LocalAuthentication.hasHardwareAsync();
const enrolled = await LocalAuthentication.isEnrolledAsync();
if (hasHardware && enrolled) {
  const { success } = await LocalAuthentication.authenticateAsync({
    promptMessage: 'Unlock your account',
    fallbackLabel: 'Use passcode',   // don't disable the passcode fallback
  });
}
```

Prefer **Sign in with Apple** or passkeys (`react-native-passkey`, or `expo-apple-authentication` for Sign in with Apple) over a hand-rolled email/password flow — see [technologies/identity-and-commerce.md](../technologies/identity-and-commerce.md).

### App tracking transparency

```js
// Required before touching the IDFA. Ask at a moment where the value is legible,
// not at launch.
import { requestTrackingPermissionsAsync } from 'expo-tracking-transparency';
const { status } = await requestTrackingPermissionsAsync();
```

### On-device processing

Where a feature could run locally, prefer it — it removes the permission conversation entirely for some cases and shrinks your data-handling obligations. In RN that usually means Core ML / Vision through a native module or an Expo module (`expo-face-detector`-style APIs, `react-native-vision-camera` frame processors) rather than uploading frames to a server.

## Review checklist

- [ ] Every permission request maps to a feature the user just reached for.
- [ ] Nothing requested at launch unless the app cannot function without it.
- [ ] Only the minimum scope requested (e.g. `WhenInUse` not `Always` for location; limited photo access where sufficient).
- [ ] Purpose strings present for every declared permission — sentence case, active voice, terminal period, stating the benefit.
- [ ] Denied path designed for every permission; no screen dead-ends on refusal.
- [ ] `canAskAgain === false` routes to Settings with an explanation, not a silently failing button.
- [ ] Pre-permission screens have exactly one button, titled "Continue"/"Next", never "Allow", with no cancel or close alongside.
- [ ] No custom messaging that could mislead someone into granting access.
- [ ] Location button not customized past legibility; its behavior verified.
- [ ] Secrets in Keychain/SecureStore — never AsyncStorage, never plain files.
- [ ] Passkeys or Sign in with Apple preferred over custom password auth; 2FA if passwords remain.
- [ ] Biometric gate on stay-logged-in apps, with passcode fallback intact.
- [ ] Tracking permission requested only where the IDFA is actually used, and not at launch.
- [ ] On-device processing preferred where feasible.
- [ ] macOS: signed with Developer ID, sandboxed, no assumptions about the current user.
- [ ] visionOS: ARKit/world-sensing permission requested at point of need with a specific reason.
