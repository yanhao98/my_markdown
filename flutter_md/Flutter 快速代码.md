## future builder


```dart
if (snapshot.connectionState != ConnectionState.done) {
  // future 未完成
  return Center(child: CircularProgressIndicator());
} else {
  if (snapshot.hasError) {
    // 加载出错
    return GestureDetector(
              onTap: () {
                //TODO:
              },
              child: Center(child: Text('加载出错，点击重试')),
            );
  } else {
    // 加载成功
    return Text(snapshot.data.toString());
  }
}
```



## TextStyle

```dart
 style: TextStyle(
   color: Colors.white,
   fontSize: 16,
   fontWeight: FontWeight.w500,
 ),
```

```dart
 Text(
   'XX',
   style: TextStyle(
   color: Color(0xFFE6E6E6),
   fontSize: 14,
   fontWeight: FontWeight.w500,
   ),
   textAlign: TextAlign.center,
 ),
```



## 占位Icon

```
Icon(Icons.grid_view, size: 35, color: Colors.white),
```



## debug persistentFooterButtons

```dart
    persistentFooterButtons: [
        OutlinedButton(
            onPressed: () {
              AppController.to.fetchContractTypeList();
            },
            child: Text('重新拉取 List'))
      ],
```

