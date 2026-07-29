# Liquid Glass in React Native

Design guidance: [foundations/color-and-materials.md](../foundations/color-and-materials.md). This page is about implementation.

## Contents

- [What you can and can't get](#what-you-can-and-cant-get)
- [Get it for free: use native components](#get-it-for-free-use-native-components)
- [Approximating it on custom views](#approximating-it-on-custom-views)
- [Scroll edge effects](#scroll-edge-effects)
- [Accessibility settings you must honor](#accessibility-settings-you-must-honor)
- [Android and web](#android-and-web)
- [Performance](#performance)
- [Review checklist](#review-checklist)

## What you can and can't get

Liquid Glass is not a blur. It's blur **plus** luminosity adaptation, specular edge highlighting, content-driven light/dark flipping, and interaction-responsive deformation. RN can reach some of that and not the rest:

| Property | Reachable from RN? |
|---|---|
| Background blur | Yes — `expo-blur` / `@react-native-community/blur` wrap `UIVisualEffectView` |
| Adapts to light/dark appearance | Yes — via blur `tint` and `useColorScheme` |
| **Luminosity adaptation to underlying content** | **No** — needs the real material |
| **Specular highlights on edges** | **No** |
| **Light/dark flip driven by scrolled content** | **No** |
| **Deformation in response to touch** | **No** |
| Correct behavior under Reduce Transparency / Increase Contrast | Only if you implement it |

The practical conclusion: **use native components wherever chrome appears**, and treat hand-built glass as a last resort for genuinely custom surfaces.

## Get it for free: use native components

This is the whole strategy. Every native-backed surface adopts the real material automatically:

```jsx
// Header / toolbar — real UINavigationBar material.
<Stack.Screen
  options={{
    headerTransparent: true,             // content scrolls underneath
    headerBlurEffect: 'systemMaterial',   // maps to the system material
    headerShadowVisible: false,
    headerLargeTitle: true,
  }}
/>
```

```jsx
// Tab bar — react-native-bottom-tabs wraps UITabBarController, so it gets
// the real material, the scroll-under behavior, and the selection haptic.
// The JS tab bar cannot.
import { createNativeBottomTabNavigator } from 'react-native-bottom-tabs/react-navigation';
```

```jsx
// Sheets — native-stack presentation uses UISheetPresentationController.
<Stack.Screen options={{ presentation: 'formSheet', sheetAllowedDetents: [0.5, 1] }} />
```

```jsx
// Menus and context menus — @react-native-menu/menu gives real UIMenu.
// Segmented controls — @react-native-segmented-control gives real UISegmentedControl.
```

**Do not set a `backgroundColor` on any of these.** A background color overrides the material and is the single most common way apps end up looking pre-iOS-26. If you need the bar tinted, let the content layer inform it instead — that's the HIG guidance and it's also what the material already does.

## Approximating it on custom views

Only for custom surfaces the system doesn't provide. Keep it to your app's most important functional elements, per the "use Liquid Glass sparingly" rule.

```jsx
import { BlurView } from 'expo-blur';
import { Platform, StyleSheet, View, useColorScheme } from 'react-native';
import { colors, radius } from '@/tokens';

/**
 * A glass-ish surface for the chrome layer only. Never use inside content —
 * per the HIG, Liquid Glass in the content layer produces a confused hierarchy.
 */
export function GlassSurface({ children, variant = 'regular', style }) {
  const scheme = useColorScheme();
  const { reduceTransparency, increaseContrast } = useA11ySettings();

  // Reduce Transparency / Increase Contrast → opaque, not "less blurry".
  // A fainter blur still fails the need the setting expresses.
  if (reduceTransparency || increaseContrast || Platform.OS === 'android') {
    return (
      <View style={[styles.base, { backgroundColor: colors.surface }, style]}>
        {children}
      </View>
    );
  }

  return (
    <BlurView
      // ultraThin ≈20, thin ≈40, regular ≈60, thick ≈85.
      intensity={variant === 'clear' ? 25 : 60}
      tint={scheme === 'dark' ? 'systemMaterialDark' : 'systemMaterialLight'}
      style={[styles.base, style]}
    >
      {/* A hairline top edge is the cheapest approximation of the specular
          edge — it reads as a distinct layer rather than a blurred region. */}
      <View style={styles.edge} pointerEvents="none" />
      {children}
    </BlurView>
  );
}

const styles = StyleSheet.create({
  base: { borderRadius: radius.xl, overflow: 'hidden' },
  edge: {
    ...StyleSheet.absoluteFillObject,
    borderTopWidth: StyleSheet.hairlineWidth,
    borderColor: 'rgba(255,255,255,0.25)',
    borderRadius: radius.xl,
  },
});
```

### The `clear` variant needs a dimming layer

Per the HIG, `clear` glass over **bright** media requires a **35% dark dimming layer**; over already-dark content, or with standard AVKit controls (which bring their own), it doesn't.

```jsx
function ClearGlassOverMedia({ children, contentIsBright }) {
  return (
    <BlurView intensity={25} tint="systemUltraThinMaterialDark" style={styles.base}>
      {contentIsBright && (
        <View style={[StyleSheet.absoluteFillObject, { backgroundColor: 'rgba(0,0,0,0.35)' }]}
              pointerEvents="none" />
      )}
      {children}
    </BlurView>
  );
}
```

Deciding `contentIsBright` reliably is the hard part — you can't sample the underlying content. Options: compute average luminance from the media's own thumbnail, or simply always dim, which is safe and matches what most media players do.

### Color on glass

```jsx
// Colour the BACKGROUND for a primary action, not the label or symbol —
// and at most one or two per view.
<GlassSurface>
  <Pressable style={{ backgroundColor: colors.accent, borderRadius: radius.capsule }}>
    <Text style={{ color: '#fff' }}>Done</Text>
  </Pressable>
  {/* Everything else stays monochrome. */}
  <Pressable><Icon name="ellipsis" color={colors.textPrimary} /></Pressable>
</GlassSurface>
```

If your content layer is colorful, keep the whole surface monochrome — colorful labels over colorful content is unreadable regardless of the material.

### Sliders and toggles

The one legitimate use of a glass appearance inside content is a **transient interactive element**: a slider thumb or toggle knob adopting glass *while being manipulated*.

```jsx
const active = useSharedValue(false);
const thumbStyle = useAnimatedStyle(() => ({
  // Glass appearance only during interaction, per the HIG exception.
  backgroundColor: active.value ? 'rgba(255,255,255,0.45)' : '#FFFFFF',
}));
```

## Scroll edge effects

The scroll edge effect — blurring and fading content as it approaches floating chrome — is what makes a transparent toolbar legible without an opaque background. **It comes from the native header/tab bar**; there is no JS API to add one to an arbitrary view.

```jsx
// Correct: native header supplies the effect.
<Stack.Screen options={{ headerTransparent: true, headerBlurEffect: 'systemMaterial' }} />
```

```jsx
// A gradient overlay is NOT equivalent — it darkens content rather than
// blurring it, so it dims what people are reading instead of protecting
// the controls. Use it only where no native bar exists, and keep it subtle.
<LinearGradient
  colors={[colors.surface, 'transparent']}
  style={{ position: 'absolute', top: 0, left: 0, right: 0, height: 24 }}
  pointerEvents="none"
/>
```

One effect per view. In a split layout each pane may have its own, at **consistent heights** so they align.

## Accessibility settings you must honor

```jsx
import { AccessibilityInfo } from 'react-native';

export function useA11ySettings() {
  const [s, setS] = useState({ reduceTransparency: false, increaseContrast: false });

  useEffect(() => {
    AccessibilityInfo.isReduceTransparencyEnabled().then(v =>
      setS(p => ({ ...p, reduceTransparency: v })));
    const subs = [
      AccessibilityInfo.addEventListener('reduceTransparencyChanged', v =>
        setS(p => ({ ...p, reduceTransparency: v }))),
      // iOS Increase Contrast has no dedicated RN event; darker-system-colors
      // is the closest proxy where available.
    ];
    return () => subs.forEach(x => x?.remove());
  }, []);

  return s;
}
```

The rule that matters: **fall back to an opaque surface, not a weaker blur.** Reduce Transparency exists because translucency is a legibility or comfort problem for that person — reducing the intensity doesn't address it.

Also verify your text contrast **against the opaque fallback**, since that's the case people with the setting on will see.

## Android and web

Android has no equivalent material, and faking one is a bad trade:

```jsx
// Don't ship a fake blur on Android — it costs frames and doesn't look native there.
// An elevated opaque surface IS the correct Android idiom.
const Surface = Platform.OS === 'ios' ? BlurView : View;
```

```jsx
// Android equivalent of a floating glass bar.
<View style={{
  backgroundColor: colors.surface,
  elevation: 3,                      // Material elevation, not blur
  borderTopWidth: StyleSheet.hairlineWidth,
  borderTopColor: colors.separator,
}} />
```

On web, `backdrop-filter: blur()` is available and reasonable — but it's a CSS blur, not the material, so treat it the same way as the RN approximation.

## Performance

Blur views are expensive, and the cost is per-frame:

- **Cap the number of blur views on screen.** One toolbar plus one tab bar is fine; a blurred surface per list row is not.
- **Never animate `intensity`.** Animate `opacity` on a container instead — changing blur radius forces a re-render of the effect every frame.
- **Avoid blur inside a scrolling list's items.** The blur recomputes as content moves beneath it.
- **Don't nest blur views.** The inner one samples the outer one's output, which both looks wrong and doubles the cost.
- **Test on the oldest device you support**, in a dark environment (blur cost is independent of appearance, but perceived stutter is worse against dark backgrounds).

```jsx
// Fading a glass bar in and out: animate the wrapper's opacity.
const style = useAnimatedStyle(() => ({ opacity: visible.value ? 1 : 0 }));
<Animated.View style={style}><GlassSurface /></Animated.View>
```

## Review checklist

- [ ] All chrome (headers, tab bars, sheets, menus, segmented controls) uses native components so the real material applies.
- [ ] No `backgroundColor` set on native headers or tab bars.
- [ ] `headerTransparent` + `headerBlurEffect` used so content scrolls under and the scroll edge effect applies.
- [ ] Hand-built glass limited to genuinely custom, important chrome surfaces — never inside the content layer.
- [ ] The only in-content glass is a transient interactive element (slider thumb, toggle knob) during manipulation.
- [ ] `clear` glass over bright media has a 35% dark dimming layer.
- [ ] Reduce Transparency and Increase Contrast fall back to an **opaque** surface, not a weaker blur.
- [ ] Text contrast verified against the opaque fallback as well as the blurred state.
- [ ] Color applied to at most one or two control backgrounds per view; labels and symbols monochrome.
- [ ] Chrome kept monochrome where content is colorful.
- [ ] One scroll edge effect per view; consistent heights across split panes.
- [ ] Gradient overlays used only where no native bar exists, and kept subtle.
- [ ] Android uses an elevated opaque surface, not a fake blur.
- [ ] Blur view count bounded; none inside list items; none nested.
- [ ] `intensity` never animated; opacity animated instead.
- [ ] Verified on the oldest supported device for frame rate.
