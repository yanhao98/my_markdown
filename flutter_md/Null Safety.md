https://blog.csdn.net/windcao/article/details/114789031

### 空安全里面有“?”

变量类型声明后面的问号：

```dart
//如果想声明一个变量既可以是个int，也可以为空，只需要给这个变量的类型声明加个问号
int? aNullableInt = null;
```

函数返回值类型后面的问号：

```dart
//返回值类型加问号代表可以返int，也可返回空
int? getIndex() {
  return 1;
}
```



### 空安全里面有 ??

??操作符放在可空变量后面，操作符后面跟一个操作数。

表示当变量为空时变量值为该操作数。

```dart
//当aNullableInt为空时 value = 0
int value = aNullableInt ?? 0;
```



### 空安全里面有“!”

在某个场景中，如果开发者能够确保一个可空变量是非空变量可以给这个变量加！

例如

```dart
int? getIndex() {
  return 1;
}

void main() {
  int? aNullableInt = null;

  List numbers = [1, 2, 3];

  aNullableInt = getIndex();

  //可以确保此处的aNullableInt是非空的
  print('numbers[${aNullableInt}]:${numbers[aNullableInt!]}');
}
```



### 空安全里面有“required”

required只能加在函数命名参数类型声明之前。

用来告诉编译器：“我后面会初始化这个变量”。

```dart
class MyHomePage extends StatelessWidget {
  // 当编译器误认为函数的参数是可空的，你可以通过required关键字来纠正它
  MyHomePage({Key? key, required this.title}) : super(key: key);
  //...
}
```



### 空安全里面有“late”

late关键字加在变量类型声明前。

用来告诉编译器：“我后面会初始化这个变量”。

```dart
class IntProvider {
  //你能确保一个变量在使用之前会被初始化为非空，但仍然被报错，你可以在变量的类型之前标记late
  late int aRealInt;

  IntProvider() {
    aRealInt = calculate();
  }
}
```





### List和Map变样了

List<String?>?

![image-20210414103106471](http://mdpic.yanhao.ren/d2980d7525c3c5c6a16a2ab4f5c0c107.png)
Map<String, int?>?

类型	Map可以空	item可以空
![image-20210414103115745](http://mdpic.yanhao.ren/33470f58559f05117d20942c4d8f5dbb.png)



# 升级

```
dart pub get
dart pub outdated --mode=null-safety
```

