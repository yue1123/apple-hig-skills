# Tab Bars, Sidebars & Search Fields

Source: HIG › Components › Navigation and search — [Tab bars](https://developer.apple.com/design/human-interface-guidelines/tab-bars), [Sidebars](https://developer.apple.com/design/human-interface-guidelines/sidebars), [Search fields](https://developer.apple.com/design/human-interface-guidelines/search-fields), [Token fields](https://developer.apple.com/design/human-interface-guidelines/token-fields), [Path controls](https://developer.apple.com/design/human-interface-guidelines/path-controls)

Read this when choosing a navigation structure or placing search.

## Contents

- [Tab bars](#tab-bars)
- [Sidebars](#sidebars)
- [Choosing between them](#choosing-between-them)
- [Search fields](#search-fields)
- [Search placement](#search-placement)
- [Token fields and path controls](#token-fields-and-path-controls)
- [React Native mapping](#react-native-mapping)
- [Review checklist](#review-checklist)

## Tab bars

**A tab bar is for navigation, never for actions.** It moves people among sections of the app — Alarm, Stopwatch, Timer. Controls that act on the current view belong in a toolbar. This is the most commonly violated tab bar rule, and putting an action tab in the bar breaks the mental model that every tab is a place.

**Keep the tab bar visible as people navigate sections.** Hiding it makes people lose track of where they are. The one exception is a modal covering it, since a modal is temporary and self-contained.

**Use as few tabs as the hierarchy allows.** Weigh the complexity of another tab against how often people need that section; fewer tabs are easier to navigate. For a complex information structure, consider a sidebar or an adaptive tab bar instead of more tabs.

**Avoid overflow tabs.** When horizontal space runs out on iOS/iPadOS, the trailing tab becomes **More**, hiding the rest in a list — which makes that content harder to reach and to notice. Limit the scenarios where this can happen.

**Never disable or hide tab bar buttons**, even when their content is unavailable. Tabs appearing and disappearing makes the app feel unstable. If a section is empty, **explain why** inside it.

**Include tab labels**, single words where possible. In compact views the icon sits above the label; in regular views they sit side by side.

**Use SF Symbols, preferring filled variants**, so icons adapt between regular and compact tab bars automatically.

**Use badges only for critical information** — a red oval with a number or exclamation point. Overuse dilutes their meaning to nothing.

**Avoid tab label colors similar to your content-layer background.** With colorful content, prefer a monochromatic tab bar or an accent color with real separation.

### Platform notes

- **iPadOS** — prefer a tab bar, and offer conversion to a **sidebar** for wider navigation. **Let people customize it** (Music lets someone put a favorite playlist in the tab bar). If tabs are customizable, keep the default list to **five or fewer** so compact and regular views stay continuous.
- **tvOS** — the tab bar sits at the **top of the screen**, not the bottom; don't carry an iOS bottom tab bar over unchanged. It **scrolls offscreen** by default when the tab holds a single main view; it stays **pinned** when the screen contains a split view. Pressing Menu on the remote always returns focus to the tab bar.
- **visionOS** — supply a symbol **and** a text label: the symbol is always visible, and the system reveals labels when people look at the bar. Keep labels short enough to read at a glance. A **sidebar inside a tab** can support secondary navigation for deep hierarchies — but sidebar selections must not change which tab is open.

## Sidebars

**Extend content beneath the sidebar.** Like toolbars and tab bars, sidebars can float in the Liquid Glass layer on iOS, iPadOS, and macOS. Reinforce the separation either by letting content scroll horizontally under it or with a **background extension effect**, which mirrors adjacent content to look like it stretches underneath.

**Let people customize the contents and order** where possible — the sidebar is their navigation, so their priorities should shape it.

**Group hierarchy with disclosure controls** if there's a lot of content, to keep vertical space manageable.

**Use familiar symbols**; prefer a custom SF Symbol over a bitmap for custom icons.

**Let people hide the sidebar**, using the platform's existing interactions — the edge swipe on iPadOS, a show/hide button plus Show/Hide Sidebar in the View menu on macOS. On visionOS the window expands to accommodate a sidebar, so hiding is rarely needed. **Don't hide it by default**, or it won't be discoverable.

**Show at most two levels of hierarchy.** Deeper than that, use a split view with a **content list column between** the sidebar and the detail view. If you do use two levels, give each group a **succinct descriptive title**.

**Make sidebar icon colors purposeful.** By default sidebar icons use your app accent color — and on macOS, people can change the system accent color and expect **all** sidebar icons to follow it. Fixed colors are for meaning, used sparingly: Mail's VIP icon is yellow specifically to mark its importance.

### Platform notes

- **iOS/iPadOS** — **consider a tab bar first.** It gives more room for content and enough flexibility for most apps' main areas. If you have more areas than fit, the tab bar's **convertible sidebar appearance** exposes the less frequently used ones.
- **macOS** — **auto-hide and reveal on window resize** (shrinking a Mail viewer collapses its sidebar to give the message more room). **Don't put critical information or actions at the bottom** — people often position windows so the bottom edge is off-screen.

## Choosing between them

The decision order Apple implies:

1. **Tab bar** — few main sections, all frequently used.
2. **Adaptive/convertible tab bar** — more sections than fit, or you want tab bar on iPhone and sidebar on iPad without maintaining two structures.
3. **Sidebar** — many navigable areas, up to two levels, benefiting from customization.
4. **Split view with a content list** — hierarchy deeper than two levels.

## Search fields

**Use placeholder text to convey scope** — what people can search and what content search reaches.

**Start searching as people type**, if possible. Continuously refined results feel responsive in a way that a submit-then-wait search doesn't.

**Show suggested terms** — recent searches before typing begins, predictive suggestions during. Useful even when search itself is immediate.

**Put the most relevant results first**, and consider categorizing them so people don't scroll to find their answer.

**Let people filter results.** A **scope bar** in the results area moves people from a broad scope to a narrow one — Mail on iPhone goes from the whole mailbox to the one being viewed.

**Default to the broader scope.** Broad results provide the context that guides people toward a useful narrowing. Starting narrow hides the existence of the rest.

**Use tokens to filter by common terms or items.** A token gets a visual treatment marking it as a single selectable, editable unit — filtering by a specific contact in Mail, or by photos in Messages.

**Pair tokens with search suggestions**, since people otherwise don't know which tokens exist.

## Search placement

### iOS

Two tab styles, for different purposes:

- **Standard tab style** — a dedicated landing page for search that can show content and suggestions before anyone types. Good for apps with varied rich content worth exploring (Apple TV shows genres and categories first).
- **Button appearance** — the keyboard appears immediately with the field above it, and exiting returns to the previous tab. A transient experience for when search should resolve quickly.

Placement:

- **Bottom** — when search is a priority, either alone in a toolbar (Settings) or alongside other controls (Mail, Notes). Keeps it easy to reach.
- **Top** — when content at the bottom must stay visible, or there's no bottom toolbar (Wallet keeps its pass stack at the bottom).
- **Inline** — when adjacency to the content strengthens the relationship. Use for filtering within a single view, and when the app has more than one search field with different scopes. Music has search as a tab *and* an inline field in the library for filtering songs and albums. When at the top, put the inline field **above the list it searches**, and consider pinning it to the top toolbar on scroll.

### iPadOS and macOS

- **Trailing side of the toolbar** — the familiar pattern, especially for split views searching across columns (Mail, Notes, Voice Memos). Good use of space: people navigate results while the selection stays visible in the detail view.
- **Top of the sidebar** — when filtering sidebar content or navigation. Settings uses this to surface sections several levels deep. Suits apps with a rich detail view needing clear separation between the filtered sidebar and the adjacent view.
- **An item in the sidebar or tab bar** — when you want a dedicated discovery area with suggestions, categories, or content that needs space (Music, TV). Also guarantees search stays available across sections.

**In a dedicated search area, focus the field on arrival** — *except* on iPad with only a virtual keyboard, where auto-focus would unexpectedly cover the view with the keyboard.

**Account for window resizing.** On iPad the field resizes fluidly like on Mac, but in **compact** views make sure search stays where it's contextually useful — Notes and Mail move it above the content-list column.

### tvOS

**Provide suggestions.** People don't want to type on a TV. Offer popular and context-specific suggestions, plus recent searches.

## Token fields and path controls

**Token fields (macOS):**

- **Add a context menu** with options or information about a token.
- **Offer more ways to create tokens.** Typing a comma creates one by default; consider also Return.
- **Consider delaying suggested tokens.** They appear immediately by default, which can distract while typing.

**Path controls (macOS):** use them in the **window body, not the window frame** — not in toolbars or status bars. The Finder's path control sits at the bottom of the window body, not in the status bar.

## React Native mapping

### Tab bar

```jsx
import { createBottomTabNavigator } from '@react-navigation/bottom-tabs';

<Tab.Navigator
  screenOptions={{
    // Native-feeling bar; on iOS prefer the native tab bar via
    // react-native-bottom-tabs for a real UITabBar (Liquid Glass, blur, haptics).
    tabBarActiveTintColor: PlatformColor('systemBlue'),
    tabBarInactiveTintColor: PlatformColor('secondaryLabel'),
    tabBarStyle: { position: 'absolute' },   // content scrolls under the bar
  }}
>
  <Tab.Screen
    name="Library"
    component={Library}
    options={{
      tabBarLabel: 'Library',                        // single word
      // Filled variant when selected — the platform convention.
      tabBarIcon: ({ focused, color, size }) => (
        <Icon name={focused ? 'books.vertical.fill' : 'books.vertical'} color={color} size={size} />
      ),
      tabBarBadge: unread > 0 ? unread : undefined,   // critical info only
      tabBarAccessibilityLabel: 'Library',
    }}
  />
</Tab.Navigator>
```

`react-native-bottom-tabs` wraps the real `UITabBarController`, which is what gets you the Liquid Glass material, the scroll-under behavior, and the iPadOS sidebar-adaptable style. The JS tab bar approximates it and is visibly different on iOS 26.

Two rules to enforce in code review:

```jsx
// Wrong: a tab that performs an action.
<Tab.Screen name="Compose" component={Compose} />   // opens a modal — not a place

// Right: actions live in a toolbar; tabs are destinations.
```

```jsx
// Wrong: conditionally removing a tab.
{isPremium && <Tab.Screen name="Stats" component={Stats} />}

// Right: always present; explain emptiness inside.
<Tab.Screen name="Stats" component={isPremium ? Stats : StatsUpsell} />
```

Keep the bar visible on nested screens — a common RN mistake is hiding it on push:

```jsx
// Only hide for modals, never for regular pushes.
<Stack.Screen name="Detail" options={{ tabBarStyle: undefined }} />   // inherits, stays visible
```

### Sidebar and adaptive navigation

```jsx
// Drive the structure from available width, not device type — see layout.md.
const { width } = useWindowDimensions();
const useSidebar = width >= 768;

// react-navigation drawer with permanent type is the closest primitive to a sidebar.
<Drawer.Navigator
  screenOptions={{
    drawerType: useSidebar ? 'permanent' : 'front',   // permanent = sidebar; front = overlay
    drawerStyle: { width: 260 },
    headerShown: true,
  }}
/>
```

For a genuine iPadOS convertible tab bar (`sidebarAdaptable`), `react-native-bottom-tabs` exposes the native option — worth using rather than hand-switching between a drawer and a tab bar, which produces a jarring transition on resize.

Two levels maximum; deeper hierarchies get a middle list column:

```jsx
// Sidebar → content list → detail, for >2 levels.
{useSidebar ? (
  <View style={{ flexDirection: 'row', flex: 1 }}>
    <Sidebar width={260} />
    <ContentList width={320} />
    <Detail style={{ flex: 1 }} />
  </View>
) : <StackNavigator />}
```

Hide tertiary columns first as width shrinks, per [layout.md](../foundations/layout.md).

### Search

```jsx
// Native search bar in the header — gives you the real UISearchController.
<Stack.Screen
  name="Library"
  options={{
    headerSearchBarOptions: {
      placeholder: 'Songs, Albums, Artists',   // conveys scope
      onChangeText: e => setQuery(e.nativeEvent.text),
      hideWhenScrolling: false,
      autoFocus: false,                        // true only in a dedicated search area, and not on iPad+virtual keyboard
      obscureBackground: false,
    },
  }}
/>
```

Search-as-you-type with debounce and cancellation:

```jsx
const [query, setQuery] = useState('');
const deferred = useDeferredValue(query);

useEffect(() => {
  if (!deferred) return;
  const controller = new AbortController();
  const t = setTimeout(() => search(deferred, { signal: controller.signal }).then(setResults), 250);
  return () => { clearTimeout(t); controller.abort(); };
}, [deferred]);
```

Recents before typing, results after — and a clear affordance for privacy:

```jsx
{query === ''
  ? <RecentSearches items={recents} onClear={clearRecents} onSelect={setQuery} />
  : <>
      <ScopeBar scopes={SCOPES} value={scope} onChange={setScope} />{/* defaults to the broadest */}
      <SectionList sections={categorize(results)} />{/* most relevant first, categorized */}
    </>}
```

Tokens are a native `UISearchToken` feature with no RN binding; approximate them with chips inside the field's accessory area, and pair them with suggestions so people discover they exist.

### Inline vs. global search

```jsx
// Two search surfaces with different scopes is legitimate — label them distinctly.
<Tab.Screen name="Search" component={GlobalSearch} />     {/* dedicated discovery area */}
// …and inside Library:
<TextInput placeholder="Filter Your Library" />            {/* inline, scoped to this view */}
```

The placeholder difference is what makes the scope legible — "Search" vs. "Filter Your Library".

### tvOS

```jsx
// Suggestions matter far more than the field on tvOS.
<SearchScreen suggestions={popular.concat(recents)} />
```

Focus management for the tab bar and search results is covered in [platforms/tvos.md](../platforms/tvos.md).

## Review checklist

- [ ] Every tab is a destination; no tab performs an action or opens a modal.
- [ ] Tab bar remains visible on pushed screens; hidden only under modals.
- [ ] Tab count small enough to avoid a More/overflow tab at any supported size.
- [ ] No tab conditionally hidden or disabled; empty sections explain themselves.
- [ ] Tab labels present, ideally single words; SF Symbols with filled variants when selected.
- [ ] Badges reserved for critical information.
- [ ] Native tab bar implementation used on iOS so Liquid Glass and scroll-under behavior are real.
- [ ] Tab labels not colored similarly to content-layer backgrounds.
- [ ] Sidebar shows at most two levels; deeper hierarchies use a middle content-list column.
- [ ] Sidebar not hidden by default; hideable via the platform's own gesture/command.
- [ ] Content extends beneath the sidebar (scroll-under or background extension effect).
- [ ] Sidebar icons follow the app/system accent color; fixed colors only where they carry meaning.
- [ ] macOS: sidebar auto-collapses on window shrink; nothing critical at its bottom.
- [ ] Navigation structure chosen by available width, not device type.
- [ ] Search field uses the native search bar, not a hand-built header input.
- [ ] Placeholder text conveys the scope.
- [ ] Search runs as people type, debounced, with in-flight requests cancelled.
- [ ] Recent searches shown before typing, with a clear-history option.
- [ ] Suggestions offered — mandatory on tvOS.
- [ ] Results ordered by relevance and categorized where helpful.
- [ ] Scope bar defaults to the broadest scope.
- [ ] Auto-focus only in a dedicated search area, and not on iPad with a virtual keyboard.
- [ ] Search placement follows the platform pattern (bottom/top/inline on iOS; toolbar trailing, sidebar top, or dedicated area on iPadOS/macOS).
- [ ] Multiple search surfaces distinguished by placeholder wording.
- [ ] macOS: path controls in the window body, not the toolbar or status bar.
