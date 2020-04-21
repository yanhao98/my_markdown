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

