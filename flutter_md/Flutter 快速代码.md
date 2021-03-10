# future builder


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

