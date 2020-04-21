

# State 的生命周期

 * **initState** ：初始化，理论上只有初始化一次，第二篇中会说特殊情况下。
 * **didChangeDependencies**：在 initState 之后调用，此时可以获取其他 State 。
 * **dispose** ：销毁，只会调用一次。



# clipBehavior

裁剪方式

| 裁剪方式               |                                                              |
| ---------------------- | ------------------------------------------------------------ |
| none                   | 不裁剪                                                       |
| hardEdge               | 裁剪但**不应用抗锯齿**，裁剪速度比none模式慢一点，但比其他方式快。 |
| antiAlias              | 裁剪而且**抗锯齿**，以实现更平滑的外观。裁剪速度比antiAliasWithSaveLayer快，比hardEdge慢。 |
| antiAliasWithSaveLayer | 带有抗锯齿的剪辑，并在剪辑之后立即保存saveLayer              |



# 工具





## 拖动组件生成代码

https://flutterstudio.app/





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

