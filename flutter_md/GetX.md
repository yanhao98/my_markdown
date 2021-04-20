[getx/state_management.md at master · jonataslaw/getx (github.com)](https://github.com/jonataslaw/getx/blob/master/documentation/zh_CN/state_management.md)

https://fluttertutorial.in/getx-in-flutter/

# GetX

 什么时候用obx,getx,getBuilder






### Workers

Workers将协助你在事件发生时触发特定的回调。


```dart
@override
onInit(){
  ever(isLogged, fireRoute);
  isLogged.value = await Preferences.hasToken();
}

fireRoute(logged) {
  if (logged) {
   Get.off(Home());
  } else {
   Get.off(Login());
  }
}
```

如果 "hasToken "是 "false"，"isLogged "就不会有任何变化，所以 "ever() "永远不会被调用。 为了避免这种问题，_observable_的第一次变化将总是触发一个事件，即使它包含相同的`.value`。

如果你想删除这种行为，你可以使用： `isLogged.firstRebuild = false;`。

```dart
///每次`count1`变化时调用。
ever(count1, (_) => print("$_ has been changed"));

///只有在变量$_第一次被改变时才会被调用。
once(count1, (_) => print("$_ was changed once"));

///防DDos - 每当用户停止输入1秒时调用，例如。
debounce(count1, (_) => print("debouce$_"), time: Duration(seconds: 1));

///忽略1秒内的所有变化。
interval(count1, (_) => print("interval $_"), time: Duration(seconds: 1));
```


