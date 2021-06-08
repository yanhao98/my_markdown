

```yaml
dependencies:
  json_annotation: ^4.0.1
dev_dependencies:
  build_runner: ^1.12.2
  json_serializable: ^4.1.0
```



```dart
/// Tell json_serializable that "registration_date_millis" should be
/// mapped to this property.
@JsonKey(name: 'registration_date_millis')
final int registrationDateMillis;

```

客户端和服务端最好保持同样的命名规则。 `@JsonSerializable()` 提供了 `fieldRename` 枚举，用于将 dart 字段完整转换为 JSON 键值。

定义 `@JsonSerializable(fieldRename: FieldRename.snake)` 与添加 `@JsonKey(name: '<snake_case>')` 到每一个字段是同样的效果。



服务端的数据有时无法确认，所以在客户端很有必要进行数据校验和保护。其他常见的 `@JsonKey` 声明方法包括：

```dart
/// Tell json_serializable to use "defaultValue" if the JSON doesn't
/// contain this key or if the value is `null`.
@JsonKey(defaultValue: false)
final bool isAdult;

/// When `true` tell json_serializable that JSON must contain the key, 
/// If the key doesn't exist, an exception is thrown.
@JsonKey(required: true)
final String id;

/// When `true` tell json_serializable that generated code should 
/// ignore this field completely. 
@JsonKey(ignore: true)
final String verificationCode;

```





```shell
flutter pub run build_runner build
```

```
flutter pub run build_runner watch
```

```dart
Map userMap = jsonDecode(jsonString);
var user = User.fromJson(userMap);
```

```dart
String json = jsonEncode(user);
```

