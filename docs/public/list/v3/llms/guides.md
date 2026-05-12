Practical recipes for common Legend List use cases.

## Chat Interfaces

Use this when your messages should start at the bottom without using `inverted`.

```ts
alignItemsAtEnd?: boolean;
maintainScrollAtEnd?: boolean;
maintainScrollAtEndThreshold?: number;
```

```tsx
<LegendList
  data={items}
  renderItem={({ item }) => <Text>{item.title}</Text>}
  estimatedItemSize={320}
  alignItemsAtEnd
  maintainScrollAtEnd
  maintainScrollAtEndThreshold={0.1}
/>
```

Pitfalls:
- Avoid `inverted`; it can cause animation and scroll edge cases.
- Tune `maintainScrollAtEndThreshold` for your UX.

## Floating Composer / Overlay Insets

Use this when a composer or input bar visually covers the end of the list but should stay outside the list's normal content.

```ts
contentInsetEndAdjustment?: number;
anchoredEndSpace?: {
  anchorIndex: number;
  anchorOffset?: number;
  anchorMaxSize?: number;
  onSizeChanged?: (size: number) => void;
};
```

On web, measure the overlay and pass that height to `contentInsetEndAdjustment`.

```tsx
<LegendList
  data={messages}
  renderItem={renderMessage}
  keyExtractor={(item) => item.id}
  initialScrollAtEnd
  maintainScrollAtEnd
  anchoredEndSpace={sentIndex !== undefined ? { anchorIndex: sentIndex } : undefined}
  contentInsetEndAdjustment={composerHeight + 24}
/>
```

On React Native, use the keyboard-aware integration and pass the shared value returned by `useKeyboardChatComposerInset`.

```tsx
const listRef = useRef<LegendListRef>(null);
const composerRef = useRef<View>(null);
const { contentInsetEndAdjustment, onComposerLayout } =
  useKeyboardChatComposerInset(listRef, composerRef);

<KeyboardAwareLegendList
  ref={listRef}
  data={messages}
  renderItem={renderMessage}
  keyExtractor={(item) => item.id}
  initialScrollAtEnd
  contentInsetEndAdjustment={contentInsetEndAdjustment}
/>

<KeyboardStickyView>
  <View ref={composerRef} onLayout={onComposerLayout}>
    <Composer />
  </View>
</KeyboardStickyView>
```

Pitfalls:
- Prefer `contentInsetEndAdjustment` over padding the list content when the overlay size changes dynamically.
- Use `anchoredEndSpace` for the row you want to land near the start after sending.
- Keep a stable `keyExtractor`; changing keys while adjusting overlay inset will discard size and position caches.

## Infinite Scrolling

Use `onEndReached` for standard feeds and `onStartReached` for prepending older items.

```ts
onStartReached?: ((info: { distanceFromStart: number }) => void) | null | undefined;
onStartReachedThreshold?: number;
onEndReached?: ((info: { distanceFromEnd: number }) => void) | null | undefined;
onEndReachedThreshold?: number;
```

```tsx
<LegendList
  data={data}
  renderItem={({ item }) => <MessageItem item={item} />}
  keyExtractor={(item) => item.id}
  onEndReached={loadMoreAtEnd}
  onStartReached={loadMoreAtStart}
  onEndReachedThreshold={0.5}
  onStartReachedThreshold={0.5}
  maintainVisibleContentPosition={{ data: true }}
  recycleItems
/>
```

Pitfalls:
- Guard against duplicate loads (`loading` state or request dedupe).
- For prepend flows, keep `maintainVisibleContentPosition={{ data: true }}`.

## Always Render

Use `alwaysRender` to keep specific rows mounted outside the virtualized window.

```tsx
<LegendList
  data={data}
  keyExtractor={(item) => item.id}
  estimatedItemSize={48}
  alwaysRender={{ top: 2, bottom: 2 }}
  renderItem={({ item, index }) => (
    <Row label={item.title} pinned={index < 2 || index >= data.length - 2} />
  )}
/>
```

`alwaysRender` accepts:
- `top` / `bottom`: keep first/last N items mounted
- `indices`: keep explicit indices mounted
- `keys`: keep specific keys mounted (requires `keyExtractor`)

## Maintain Visible Content Position

Use this when data or size changes above the viewport should not move what the user is reading.

```ts
maintainVisibleContentPosition?:
  | boolean
  | {
      data?: boolean;
      size?: boolean;
      shouldRestorePosition?: (item: ItemT, index: number, data: ItemT[]) => boolean;
    };
```

Defaults:
- `size: true` stabilizes scroll during size/layout changes
- `data: false` does not anchor on data changes unless enabled

Common setup for prepend-heavy feeds:

```tsx
<LegendList
  data={messages}
  maintainVisibleContentPosition={{ data: true, size: true }}
  onStartReached={loadOlderMessages}
/>
```

## SectionList patterns

For grouped data with headers/footers per section, use `SectionList`.

```tsx
import { SectionList } from "@legendapp/list/section-list";

<SectionList
  sections={sections}
  keyExtractor={(item) => item.id}
  renderSectionHeader={({ section }) => <Header title={section.title} />}
  renderItem={({ item }) => <Row item={item} />}
  stickySectionHeadersEnabled
  estimatedItemSize={48}
/>
```

For full prop and method details (including `scrollToLocation`), see [API Reference](../api#sectionlist).
