# 函数传参

```dart
fun(a) {
  print(a);
}

main() {
  fun(1);
}
```

## 命名参数

```dart
fun({a}) {
  print(a);
}

main() {
  fun(a: 1);
}
```

## 可选位置参数

在函数参数中用[]符号包裹可选的位置参数：

```dart
String say(String from, String msg, [String device] ) {
  var result = '$from says $msg';
  if (device != null) {
    result = '$result with a $device';
  }
  return result;
}
```



# 判断数据类型

```dart
obj.runtimeType.toString()
```



# Future.delayed

```dart
Future<void> main() async {
  print('1');
  print(await request());
  print('2');
}

request() async {
  await Future.delayed(const Duration(seconds: 1));
  return 'ok';
}
```



# Future函数

```dart
Future<String> testFuture() {
   return Future.value("Hello");
//  return Future.error("yxjie 创造个error");//onError会打印
//  throw "an Error";
}
```



# Future.wait

```dart
Future afterTwoThings(Future first, Future second) {
  return Future.wait([first, second]);
}

Future first() {
  return Future.value(['first']);
}

Future second() {
  return Future.value('second');
}

main() async {
  print(await first());
  print(await second());
  List values = await afterTwoThings(first(), second());
  print(values[0]);
}
```



# 生成器

## 自己的理解

### 1.普通函数调用

```dart
void main() {
  fun();
}

// 自己的理解：调用者只负责调用
// 被调用函数 对数据进行处理。
void fun() {
  for (var i = 0; i < 2; i++) {
    print('i:$i');
  }
}
```

### 2.同步函数生成器

```dart
import 'dart:io';

void main() {
  // 自己的理解：调用者只需要数据。
  // 调用者拿到数据后自己处理。
  getRange().forEach((data) {
    print(data);
  });
}

Iterable<int> getRange() sync* {
  yield 1;
  sleep(Duration(seconds: 1));
  yield 2;
}
```

## sync async

### sync* : 同步

```dart
void main() {
  print('create iterator');
  Iterable<int> numbers = getNumbers(3);
  print('starting to iterate...');
  for (int val in numbers) {
    print('$val');
  }
  print('end of main');
}

Iterable<int> getNumbers(int number) sync* {
  print('generator started');
  for (int i = 0; i < number; i++) {
    yield i;
  }
  print('generator ended');
}
```

### async* : 异步

```dart
void main() {
  print('create iterator');
  Stream<int> numbers = getNumbers(3);
  print('starting to listen...');
  numbers.listen((int value) {
    print('$value');
  });
  print('end of main');
}

Stream<int> getNumbers(int number) async* {
  print('waiting inside generator a bit :)');
  await new Future.delayed(new Duration(seconds: 5)); //sleep 5s
  print('started generating values...');
  for (int i = 0; i < number; i++) {
    await new Future.delayed(new Duration(seconds: 1)); //sleep 1s
    yield i;
  }
  print('ended generating values...');
}
```

### yield* : 调用另一个 生成器

```dart
Iterable<int> outer() sync* {
  yield 1;
  yield* inner();
  yield 2;
}

Iterable<int> inner() sync* {
  yield 11;
  yield 22;
}

void main() {
  outer().forEach((data) {
    print(data);
  });
}
```

### yield* : 递归

It delegates to another generator and after another generator stop producing values, then it resumes generating its own values.

它委派给另一个生成器，然后在另一个生成器停止生成值之后，它将继续生成自己的值。

```dart
import 'dart:async';

main() async {
  await for (int i in numbersDownFrom(5)) {
    print('$i bottles of beer');
  }
}

Stream numbersDownFrom(int n) async* {
  if (n >= 0) {
    await new Future.delayed(new Duration(milliseconds: 100));
    yield n;
    yield* numbersDownFrom(n - 1);
  }
}
```



# 时间相关

## *DateTime*

### 当前时间

```dart
DateTime.now();
```

#### 时间戳

```dart
DateTime.now().millisecondsSinceEpoch
```



## Duration

```dart
new Duration(seconds: 1,...);
```

- days:天

- hours:小时

- minutes:分钟
- seconds:秒
- milliseconds:毫秒
- microseconds:微秒





# JOSN

## json.decode

json字符串转化成Map<String, dynamic>的类型。



# 数组操作

## List.from()

*克隆一个数组*

```dart
List arr = [1, 3, 5, 2, 7, 9];

var clonedArr = List.from(arr);
print(clonedArr);
// [1, 3, 5, 2, 7, 9]
```



# 单例

## demo1

*创建一个单例的Manager类*

```dart
class Manager {
  // 工厂模式
  factory Manager() =>_getInstance()
  static Manager get instance => _getInstance();
  static Manager _instance;
  Manager._internal() {
    // 初始化
  }
  static Manager _getInstance() {
    if (_instance == null) {
      _instance = new Manager._internal();
    }
    return _instance;
  }
}
```

调用

```dart
// 无论如何初始化，取到的都是同一个对象
Manager manager = new Manager();
Manager manager2 = Manager.instance;
identical(manager,manager2);
```



## demo2

```dart
class MyBloc {
  // -------  SINGLETON ------
  static final MyBloc _instance = MyBloc._internal();
  factory MyBloc() => _instance;
  MyBloc._internal();
}
MyBloc myBloc = MyBloc();
```

