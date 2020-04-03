# State 的生命周期

 * **initState** ：初始化，理论上只有初始化一次，第二篇中会说特殊情况下。
 * **didChangeDependencies**：在 initState 之后调用，此时可以获取其他 State 。
 * **dispose** ：销毁，只会调用一次。



# ScrollController

## 属性：position

- `pixels`：当前滚动位置。
- `maxScrollExtent`：最大可滚动长度。
- `extentBefore`：滑出ViewPort顶部的长度。
- `extentInside`：ViewPort内部长度。
- `extentAfter`：列表中未滑入ViewPort部分的长度。
- `atEdge`：是否滑到了可滚动组件的边界（此示例中相当于列表顶或底部）。



# NotificationListener

## 监听滚动

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







# 工具

## json_to_dart

https://javiercbk.github.io/json_to_dart/



## 拖动组件生成代码

https://flutterstudio.app/



# Provider

*https://flutter.cn/docs/development/data-and-backend/state-mgmt/simple#changenotifierprovider*

## 1.创建共享数据的模型

### 1-1.创建一个 model object

注意：***extends ChangeNotifier***

这是需要共享的数据。

```dart
class Counter with ChangeNotifier {
  int value = 0;

  void increment() {
    value += 1;
    notifyListeners();
  }
}
```

### 1-2.ChangeNotifierProvider

用 ChangeNotifierProvider 包裹需要使用 model 的 widget 之上。

```dart
ChangeNotifierProvider(
  create: (context) => Counter(),
  child: MyApp(),
),
```



## 2.展示值的 Widget

```dart
Consumer<Counter>(
  builder: (context, counter, child) => Text(
    '${counter.value}',
    style: Theme.of(context).textTheme.display1,
  ),
),
```

### child 用法

```dart
return Consumer<CartModel>(
  builder: (context, cart, child) => Stack(
        children: [
          // Use SomeExpensiveWidget here, without rebuilding every time.
          child,
          Text("Total price: ${cart.totalPrice}"),
        ],
      ),
  // Build the expensive widget here.
  child: SomeExpensiveWidget(),
);
```



## 3.改变值

```dart
Provider.of<Counter>(context, listen: false).increment(),
```

 listen 参数，它代表了是否监听数据变化，默认为 true。



# 路由

可以这样设置路由别名。

```dart
class PageC extends StatelessWidget {
  /*路由别名*/
  static const routeName = '/pageC';
```



# Podfile

https://github.com/flutter/flutter/blob/master/packages/flutter_tools/templates/cocoapods/Podfile-ios-objc

https://github.com/flutter/flutter/blob/master/packages/flutter_tools/templates/cocoapods/Podfile-ios-swift

