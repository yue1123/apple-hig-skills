# Sign in with Apple, Apple Pay, In-App Purchase & Wallet

Source: HIG › Technologies — [Sign in with Apple](https://developer.apple.com/design/human-interface-guidelines/sign-in-with-apple), [Apple Pay](https://developer.apple.com/design/human-interface-guidelines/apple-pay), [In-app purchase](https://developer.apple.com/design/human-interface-guidelines/in-app-purchase), [Wallet](https://developer.apple.com/design/human-interface-guidelines/wallet), [Tap to Pay on iPhone](https://developer.apple.com/design/human-interface-guidelines/tap-to-pay-on-iphone), [Game Center](https://developer.apple.com/design/human-interface-guidelines/game-center), [ID Verifier](https://developer.apple.com/design/human-interface-guidelines/id-verifier)

Account management generally (deletion, deferring sign-in) is in [patterns/data-entry-search-settings.md](../patterns/data-entry-search-settings.md).

## Contents

- [Sign in with Apple](#sign-in-with-apple)
- [Apple Pay](#apple-pay)
- [In-app purchase](#in-app-purchase)
- [Wallet](#wallet)
- [Tap to Pay on iPhone](#tap-to-pay-on-iphone)
- [React Native mapping](#react-native-mapping)
- [Review checklist](#review-checklist)

## Sign in with Apple

**Ask people to sign in only in exchange for value**, and show a brief, approachable description of the benefits — personalization, additional features, data sync.

**Delay sign-in as long as possible.** A live-streaming app can let people explore content before signing in to stream.

**If you require an account, ask people to set it up *before* offering sign-in options.** Explain the reason for requiring an account first; after setup completes, let them choose a sign-in method.

**In a commerce app, wait until after a purchase to ask for account creation.** With guest checkout, offer account creation on the **order confirmation page**. And if Apple Pay already supplied their name and email, **don't ask for it again**.

**Consider account linking** so people get Sign in with Apple's convenience while keeping access to an existing account's data — offered before or after they sign in to the existing account.

**Welcome people immediately after Sign in with Apple completes.** Don't delay with requests for information that isn't required.

**Indicate the current sign-in method** — "Using Sign in with Apple" in settings or account screens.

**Never ask for a password.** Not needing one is the point.

**Never ask for a personal email address when someone supplied a private relay address.** Respect the choice. For customer service or retail flows needing email identification, work with the relay address.

**Clarify whether additional data is required or recommended.** Legally or contractually required items (terms agreement, country, birth date, real-identity information) must be clearly mandatory; optional items must be clearly optional with the benefit explained.

**Ask for optional data after people have engaged**, tied to a benefit — a phone number for text updates, social information for playing with friends. **Never block account access or features when people decline optional data.**

**Be transparent about what you collect.** Welcoming people by the name or email they shared establishes how you use it and — for a relay address — shows them where to find it later. **If you don't display data you asked for, people wonder why you asked.**

**Display the button prominently** — **no smaller than other sign-in buttons**, and **never requiring scrolling to see**.

**Adjust the corner radius to match your other buttons** — rounded by default, but square or capsule are available.

## Apple Pay

**Offer it on every device and browser that supports it**, and **don't present it where it isn't supported.**

**If credentials are available, Apple Pay must be the primary payment option** everywhere you use the availability APIs — that's a requirement, not a preference. **Don't put it in a separate step or flow.** Pre-select it when shown alongside other options, present it first, larger, or visually separated.

**Apple Pay buttons initiate payment or Apple Pay setup — nothing else.** If someone taps it without Apple Pay set up, they get the setup opportunity.

**A custom payment button must not display "Apple Pay" or the Apple Pay logo.** In that case, indicate acceptance separately with the **Apple Pay mark** graphic or a text reference on the same page.

**The Apple Pay mark communicates acceptance only.** It doesn't facilitate payment — **never use it as, or position it as, a button.**

**Don't hide or disable the Apple Pay button.** If it can't be used yet — no size or color selected — **let people tap it and then gracefully point out the problem.**

**Collect required options (size, color) before people reach the button.** When something's missing at checkout, highlight it, add warning text, and **automatically navigate to the problematic field.**

**Collect optional information before checkout, or after purchase.** Gift messages and delivery instructions **can't be entered on the payment sheet**.

**Support coupons and promotional codes directly on the payment sheet** rather than as a separate step — especially important for express checkout, where people bypass the standard flow.

**Provide a cohesive checkout experience.** Your branding throughout, and **avoid opening different pages or windows** — on the web especially, new windows read as a handoff to a different site.

**Accelerate single-item purchases** with Apple Pay buttons on product detail pages. Such a purchase covers **that item only**, excluding the cart — and if the item was in the cart, **remove it after purchase**.

**Accelerate multi-item purchases with express checkout** — the payment sheet immediately, buying the whole cart with one shipping method and destination.

**Tell search engines you accept Apple Pay** if your site uses semantic markup for product details.

## In-app purchase

**Let people experience the app before purchasing.** For auto-renewable subscriptions, consider limited free access.

**Design an integrated shopping experience** — present products and handle transactions in your app's style, so it never feels like a different app.

**Simple, succinct product names and descriptions** that don't truncate or wrap.

**Display the total billing price for every purchase**, of every type.

**Only display your store when people can make payments.** If they can't — parental restrictions — hide the store or explain why it's unavailable.

**Use the default confirmation sheet.** It prevents accidental purchases; **don't modify or replicate it.**

**Mention Family Sharing prominently** where people learn about your content — "Family" or "Shareable" in a subscription name, and a reference on your sign-up screen. **Customize in-app messaging for both purchasers and family members** — a family member seeing shared content for the first time might get "Your family subscription includes…".

### Refunds

**Provide help people can view before requesting a refund** — a custom purchase-help screen alongside the link to the system refund flow, covering missing purchases, FAQs, and ways to contact you.

**Title the refund action simply** — "Refund" or "Request a Refund". The system flow already makes clear the request goes to Apple.

**Help people find the problematic purchase** — product image, name, description, original purchase date.

**Consider alternative solutions** — immediate fulfillment or a conciliatory item if something wasn't received — while **making clear people can still request a refund.**

**Don't let help content become a barrier.** **No scrolling or extra screens** to reach the refund-request button.

## Wallet

**Offer to add new passes** when an action produces one — buying a ticket, joining a rewards program. For frequent predictable actions like flight check-in, add passes **in the background** after a one-time authorization (Wallet notifies the person each time). If people should review a pass first, show a custom view with an **Add to Apple Wallet** button.

**Help people add passes created outside your app** — from your website or another device — the next time they open the app. **If they decline, don't ask again.**

**Add related passes as a group** — all boarding passes for a multi-leg flight at once; a bundled set of event tickets on your website.

**Display an Add to Apple Wallet button** wherever pass information appears, so people who previously declined or removed a pass can change their mind.

**Let people jump to the pass in Wallet** — a link labelled something like "View in Wallet".

**Tell the system when passes expire.** Set the expiration date, relevant date, and voided properties correctly so Wallet can hide expired passes.

**Always get permission before deleting passes.** Offer an in-app setting for manual vs. automatic removal, and show an alert before deletion if needed.

**Help the system suggest passes.** Supplying when and where a pass is relevant lets the system surface it on the Lock Screen at the right moment — a gym card as someone enters the gym — and for some types (event tickets) start a Live Activity.

**Keep passes up to date** — a boarding pass reflecting delays and gate changes.

**Use change messages only for time-critical updates.** A gate change, yes. A customer service phone number change, no. **Never for marketing or other noncritical communication.**

**Design for all devices.** A pass on Apple Watch shows less information and fewer images. **Don't put essential information in elements that might be unavailable**, and **don't pad images** — watchOS crops white space from some.

**Keep the pass front uncluttered.** Essential information — event date, account balance — in the **header**, visible when the pass is collapsed. Quick-access information on the rest of the front. Everything else on the additional information sheet.

**Make the pass instantly identifiable** with brand colors and visual elements.

**Ensure sufficient contrast** between background and text, against both solid backgrounds and background images.

## Tap to Pay on iPhone

For apps where a **merchant** accepts contactless payments on their iPhone.

**Get terms and conditions accepted before the merchant starts a customer-facing flow** — in your in-app messaging or onboarding.

**Present terms only to an administrative user.** A non-administrator gets a message explaining admin access is required; for enterprise apps, an administrator can accept through a web interface or another app, possibly on a non-iPhone device.

**Provide a tutorial** covering supported payment types and how to accept each.

**Offer Tap to Pay as a checkout option whether or not it's enabled yet.** Tapping it presents terms if needed, then opens the Tap to Pay screen once configuration completes.

**Never make merchants wait.** Configuration runs at app start **and again on every transition to the foreground** — prepare it immediately in both cases. Keep the checkout option **selectable during configuration**, showing a progress indicator: indeterminate normally, **determinate when the API reports progress**.

**Make the button easy to find** — no scrolling. If Tap to Pay is your only payment-acceptance method, **open it automatically when checkout begins**.

**Make switching between Tap to Pay and hardware accessories easy** — set both up together, and let merchants choose during checkout without visiting settings.

**Button label: "Tap to Pay on iPhone", or "Tap to Pay" if space is constrained.** If it's your only method, your existing Charge or Checkout button can activate it. With icons, use `wave.3.right.circle` or `wave.3.right.circle.fill`. **Never include the Apple logo.** Match your app's button color and shape otherwise.

**Determine the final amount before opening the Tap to Pay screen.** Tipping and other total-affecting interactions come first, and the **final amount should appear on the Tap to Pay screen.**

**Show pre-payment options before the Tap to Pay screen** — payment type selection goes in your checkout screen after the button tap, before the system screen.

**Start processing as soon as possible** — the API can return a successful tap's result **before** the completion checkmark animation finishes.

## React Native mapping

### Sign in with Apple

```jsx
import * as AppleAuthentication from 'expo-apple-authentication';

const [available, setAvailable] = useState(false);
useEffect(() => { AppleAuthentication.isAvailableAsync().then(setAvailable); }, []);

{available && (
  <AppleAuthentication.AppleAuthenticationButton
    buttonType={AppleAuthentication.AppleAuthenticationButtonType.SIGN_IN}
    buttonStyle={AppleAuthentication.AppleAuthenticationButtonStyle.BLACK}
    // Match your other buttons — and keep it no smaller than they are.
    cornerRadius={radius.md}
    style={{ height: 48, width: '100%' }}
    onPress={signIn}
  />
)}
```

Placement matters as much as the button: render it **above the fold** and **at least as large** as your other sign-in options. A Sign in with Apple button below a fold of email/password fields violates the guidance.

```js
async function signIn() {
  const credential = await AppleAuthentication.signInAsync({
    requestedScopes: [
      AppleAuthentication.AppleAuthenticationScope.FULL_NAME,
      AppleAuthentication.AppleAuthenticationScope.EMAIL,
    ],
  });
  // Name and email arrive ONLY on first authorization — persist them now.
  // On later sign-ins they're null, which is the most common integration bug.
  await saveIdentity({ id: credential.user, name: credential.fullName, email: credential.email });
}
```

Handle the relay address correctly:

```jsx
// A @privaterelay.appleid.com address is a real, working address.
// Never prompt for a "real" one.
{email.endsWith('privaterelay.appleid.com') && (
  <Text style={s.detail}>We'll email you at your Apple private relay address.</Text>
)}
```

Show the current method, and revoke tokens on account deletion:

```jsx
<SettingsRow title="Sign-In Method" detail="Using Sign in with Apple" />
// On deletion, your backend calls Apple's token revocation endpoint —
// required, per patterns/data-entry-search-settings.md.
```

### Apple Pay

```jsx
import { PlatformPay, PlatformPayButton, usePlatformPay } from '@stripe/stripe-react-native';
// Alternatives: react-native-payments (PassKit directly), or your PSP's SDK.

const { isPlatformPaySupported, confirmPlatformPayPayment } = usePlatformPay();
const [applePay, setApplePay] = useState(false);
useEffect(() => { isPlatformPaySupported().then(setApplePay); }, []);

// If available, it must be the PRIMARY option — first, prominent, pre-selected.
{applePay ? (
  <>
    <PlatformPayButton
      type={PlatformPay.ButtonType.Buy}
      appearance={PlatformPay.ButtonStyle.Black}
      borderRadius={radius.md}
      style={{ height: 48, width: '100%' }}
      onPress={pay}
    />
    <Divider label="Or pay another way" />
    <OtherPaymentOptions />
  </>
) : (
  <OtherPaymentOptions />   // don't show Apple Pay at all when unsupported
)}
```

Don't disable the button for missing options — let it be tapped and then point at the problem:

```jsx
// Wrong: <PlatformPayButton disabled={!size} />
// Right:
function pay() {
  if (!size) {
    setError('Choose a size to continue.');
    sizeFieldRef.current?.focus();      // navigate to the problem field
    return;
  }
  confirmPlatformPayPayment(clientSecret, { applePay: { cartItems, merchantCountryCode, currencyCode } });
}
```

Collect optional data outside the sheet, since it can't be entered there:

```jsx
// Gift message / delivery instructions: before checkout, or after purchase.
<GiftMessageField />   // rendered on the cart screen, not the payment sheet
```

### In-app purchase

```jsx
import { requestPurchase, getSubscriptions, initConnection } from 'react-native-iap';
// or expo-in-app-purchases / RevenueCat.

// Hide the store when payments aren't possible.
const [canPay, setCanPay] = useState(false);
useEffect(() => { initConnection().then(() => setCanPay(true)).catch(() => setCanPay(false)); }, []);

{canPay ? <Store /> : <StoreUnavailableExplanation />}
```

```jsx
// Show the TOTAL billing price, localized — not a computed string.
<Text style={text.body}>{product.localizedPrice} / {product.subscriptionPeriodUnitIOS}</Text>
```

Never build your own confirmation sheet — `requestPurchase` presents the system one, and replicating it is prohibited.

Family Sharing and refunds:

```jsx
// Surface Family Sharing where people learn about the content.
<Text style={text.headline}>Family Plan</Text>
<Text style={text.footnote}>Shareable with up to five family members.</Text>

// Refund help: the system flow, reachable without scrolling.
<Pressable onPress={() => Linking.openURL('https://reportaproblem.apple.com')}>
  <Text>Request a Refund</Text>
</Pressable>
// Purchase list with images, names, and original dates so people can identify one.
```

Subscription management links to the system screen rather than a custom one:

```js
Linking.openURL('itms-apps://apps.apple.com/account/subscriptions');
```

### Wallet

```js
// react-native-passkit-wallet or a native module wrapping PKAddPassesViewController.
import PassKit from 'react-native-passkit-wallet';

const canAdd = await PassKit.canAddPasses();
// Add a group at once, not one at a time.
await PassKit.addPasses(passesBase64Array);
```

Pass design (`.pkpass` bundles) is server-side, not RN. The design rules above — header content, contrast, watch-safe fields, change messages only for time-critical updates — apply to that server-side template.

```jsx
// Link into Wallet where you show pass information.
<Pressable onPress={() => Linking.openURL('shoebox://')}><Text>View in Wallet</Text></Pressable>
```

### Tap to Pay

ProximityReader has no RN binding — merchant-facing Tap to Pay requires a native module, and your PSP may supply one. The configuration-on-every-foreground requirement means the native side must hook `applicationWillEnterForeground`, not just app launch.

### Game Center

```js
// react-native-game-center / expo-game-center wrappers exist but are thin.
// Authentication, leaderboards, and achievements are GameKit — native.
```

## Review checklist

- [ ] Sign in with Apple button no smaller than other sign-in buttons and visible without scrolling.
- [ ] Button corner radius matches the app's other buttons.
- [ ] Sign-in deferred until a value exchange; commerce apps ask after purchase.
- [ ] Name and email persisted on first authorization (they're null afterwards).
- [ ] Private relay addresses accepted; no request for a "real" email.
- [ ] No password requested when using Sign in with Apple.
- [ ] Current sign-in method shown in settings; tokens revoked on account deletion.
- [ ] Required vs. optional additional data clearly distinguished; declining optional data never blocks access.
- [ ] Data that was requested is actually used or displayed.
- [ ] Apple Pay hidden entirely when unsupported; primary and pre-selected when credentials exist.
- [ ] Apple Pay not isolated into a separate step or flow.
- [ ] Apple Pay buttons only initiate payment or setup; custom buttons carry no Apple Pay wordmark or logo.
- [ ] Apple Pay mark used only to indicate acceptance, never as a button.
- [ ] Apple Pay button never hidden or disabled; missing options surfaced after tap with focus moved to the field.
- [ ] Required product options collected before checkout; optional data collected before or after, not on the sheet.
- [ ] Coupon/promo entry available on the payment sheet.
- [ ] Checkout stays in-app with your branding; no new windows.
- [ ] Product-page Apple Pay purchases cover only that item, and remove it from the cart afterwards.
- [ ] IAP store hidden or explained when payments are unavailable.
- [ ] Total billing price displayed, localized, for every product.
- [ ] System purchase confirmation sheet used unmodified.
- [ ] Family Sharing surfaced where people learn about content, with messaging for both purchasers and members.
- [ ] Refund request reachable without scrolling, titled simply, with identifiable purchase history and optional alternatives.
- [ ] Subscription management links to the system screen.
- [ ] Passes offered on creation, added as groups, and re-offerable via an Add to Apple Wallet button.
- [ ] "View in Wallet" links provided wherever pass information appears.
- [ ] Pass expiration, relevant date, and voided properties set; permission obtained before deletion.
- [ ] Pass relevance information supplied so the system can surface it on the Lock Screen.
- [ ] Change messages only for time-critical updates, never marketing.
- [ ] Pass header carries the essential information; no essential data in watch-unavailable fields; images unpadded; text contrast verified over images.
- [ ] Tap to Pay: terms accepted before customer-facing flows and only by an administrator.
- [ ] Tap to Pay prepared at launch **and** on every foreground transition; option selectable during configuration with a progress indicator.
- [ ] Tap to Pay button labelled correctly, without the Apple logo, and reachable without scrolling.
- [ ] Final amount determined before the Tap to Pay screen opens.
