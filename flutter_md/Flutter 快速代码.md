# future builder


```dart
if (snapshot.connectionState != ConnectionState.done) {
  // future 未完成
  return Center(child: CircularProgressIndicator());
} else {
  if (snapshot.hasError) {
    // 加载出错
    return Center(child: Text(snapshot.error.toString()));
  } else {
    // 加载成功
    return Text(snapshot.data.toString());
  }
}
```

