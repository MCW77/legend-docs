Legend List is a high-performance virtualized list for **React Native and React DOM**. It is a drop-in replacement for FlatList/FlashList on React Native, and a DOM-native virtual list on the web.

Version 3 is a full cross-platform list system, not just a mobile list with a web adapter. It shares the same virtualization core across renderers while exposing typed entrypoints for each platform.

- ✨ Fast dynamic-size virtualization
- ✨ Accurate `initialScrollIndex` and `initialScrollAtEnd`
- ✨ Chat and AI chat without inverted lists
- ✨ Bidirectional infinite lists with scroll anchoring
- ✨ Floating composer and overlay inset support
- ✨ Optional item recycling with recycling-aware hooks
- 🧲 Sticky headers, SectionList, and always-mounted rows
- 🌐 React Native and DOM-native React support

For more information, check out:
- [Legend List: Optimizing for Mobile & Web | React Universe Conf 2025](https://www.youtube.com/watch?v=Ui3qVl80Pzg)
- [Legend List: Optimizing for Peak List Performance | App.js Conf 2025](https://www.youtube.com/watch?v=4nfxSE9OByQ)
- [Jay's conversation on React Native Radio](https://infinite.red/react-native-radio/rnr-325-legend-list-with-jay-meistrich)

## It's fast

This video was recorded as a performance test scrolling ludicrously fast with heavy items. LegendList handles expensive components with a quick recovery.

<div className="w-[800px] max-w-full mx-auto rounded-xl overflow-hidden">
    <video
        src="/open-source/assets/legendlist2.mp4"
        controls
        width="100%"
        height="100%"
        autoPlay
        loop
        muted
        className="rounded-xl"
    />
</div>

<br />

## It uses less resources

A FlashLight measurement of the above test shows that LegendList uses less CPU while scrolling. And it uses less memory too! See the [FlashLight results](/open-source/list/flashlight.html) for more details.

<img src="/open-source/list/flashlight.png" className="w-[800px] max-w-full mx-auto mt-8 rounded-xl overflow-hidden border border-fd-border" />

## Choose your platform

- **React Native** → import from `@legendapp/list/react-native` and start with [Getting Started (React Native)](../react-native/getting-started).
- **React (Web)** → import from `@legendapp/list/react` and start with [Getting Started (Web)](../react/getting-started).
- **React Native Web** → use the React Native entrypoint so your app keeps React Native component semantics.

The root import (`@legendapp/list`) still works, and all of these imports run the same JS, but each import has platform-specific types.

## Built for Chat and Feeds

Legend List has first-class primitives for timelines that grow at both ends:

- `initialScrollAtEnd` starts at the newest item without rendering the whole history first.
- `initialScrollIndex` supports target indices with `viewOffset` and `viewPosition`.
- `maintainScrollAtEnd` follows new messages while the user is already near the end.
- `maintainVisibleContentPosition` keeps the current viewport stable during prepends and size changes.
- `onStartReached` and `onEndReached` support bidirectional pagination.

For floating composers and AI chat UIs, `contentInsetEndAdjustment` reserves real end space on web, and `KeyboardAwareLegendList` plus `useKeyboardChatComposerInset` do the same for React Native keyboard-driven layouts.

See [Guides](../guides#chat-interfaces) and [Keyboard & Animated](../react-native/keyboard-and-animated#keyboardawarelegendlist).

## Dynamic Layout Without Guesswork

Items can have dynamic heights by default. You can add hints when you have them:

- `estimatedItemSize` for a single rough estimate
- `getFixedItemSize` for fixed-size rows that do not need measuring
- `getItemType` to pool recycled items and size averages by item type
- `onItemSizeChanged` and `getState().getAverageItemSizes()` to tune estimates from real measurements

For large initial scroll targets, v3 can seed the initial render near the requested target instead of walking every item from the beginning when the list has enough information to do so.

## Web Support

The `@legendapp/list/react` entrypoint renders DOM elements directly, with no React Native dependency required. It supports:

- `useWindowScroll` for pages that scroll at the document level
- CSS `style`, `contentContainerStyle`, and `contentContainerClassName`
- `contentInsetEndAdjustment` as real trailing DOM space for overlays
- the same core scroll, measurement, recycling, and viewability behavior as React Native

See the [web examples](../react/examples) for product-style examples and source fixtures.

## Advanced List Features

Legend List v3 also includes:

- `SectionList` from `@legendapp/list/section-list`, with sticky section headers and `scrollToLocation`
- `stickyHeaderIndices` with `stickyHeaderConfig`
- `alwaysRender` for keeping top, bottom, explicit index, or key-based rows mounted
- `numColumns` and `overrideItemLayout` for grid-style lists with spanning items
- `dataVersion` and `itemsAreEqual` for mutable data or semantic equality checks
- `viewabilityConfig`, `viewabilityConfigCallbackPairs`, `useViewability`, and `useViewabilityAmount`
- async imperative ref methods like `scrollToIndex`, `scrollToEnd`, and `scrollToOffset`
- `getState()`, listener helpers, scroll metrics, and `clearCaches` for advanced integrations

## What’s new in v3

- Web support (no React Native dependency required)
- SectionList component (`@legendapp/list/section-list`)
- `alwaysRender` for keeping selected items mounted
- New `maintainVisibleContentPosition` configuration
- `initialScrollAtEnd` and improved initial-scroll targeting
- `contentInsetEndAdjustment` for floating composers and overlay UI
- `KeyboardAwareLegendList` and `useKeyboardChatComposerInset`
- Expanded `getState()` and async ref methods
- `stickyHeaderIndices` (with `stickyIndices` deprecated)

Read the full change summary in [Migration to v3](../migration).
