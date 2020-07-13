```dart
final RenderBox renderBox = context.findRenderObject();
        final position = renderBox.localToGlobal(Offset.zero);
        print("POSITION of Item: $position ");
        print("SIZE of Item：${renderBox.size}");


  //用 showOnScreen 滚动到 Item
  final RenderBox renderBox = context.findRenderObject();
        renderBox.showOnScreen(
          duration: Duration(milliseconds: 300),
          rect: Rect.fromLTRB(
            0,
            0.0, // 如果需要滚动到底部显示需要计算 item 到顶部的距离，
            0,
            525.0 + 160.0, 
          ),
          curve: Curves.fastOutSlowIn,
        );
```

160是Item的高度。

![image-20200709112729565](https://i.loli.net/2020/07/09/OFCbVLKZuq4Ugnp.png)