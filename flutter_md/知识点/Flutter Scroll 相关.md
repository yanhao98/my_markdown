# ScrollController

## 属性：position

- `pixels`：当前滚动位置。
- `maxScrollExtent`：最大可滚动长度。
- `extentBefore`：滑出ViewPort顶部的长度。
- `extentInside`：ViewPort内部长度。
- `extentAfter`：列表中未滑入ViewPort部分的长度。
- `atEdge`：是否滑到了可滚动组件的边界（此示例中相当于列表顶或底部）。



# NotificationListener 监听滚动

```dart
NotificationListener<ScrollNotification>(
  onNotification: (notification) {
    print(notification);
    return false; // 阻止冒泡
  },
  child: ListView.builder(),
);
}
```

### notification.metrics

同 `scrollController` 的 `position`