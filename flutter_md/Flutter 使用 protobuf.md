```shell
brew install protobuf
pub global activate protoc_plugin
export PATH="$PATH":"$HOME/.pub-cache/bin"
```

将`.proto`文件编译成dart文件

```
protoc -I=. --dart_out=. filename.proto
```



vscode格式化`.proto`文件

```
brew install clang-format
```

安装扩展

https://marketplace.visualstudio.com/items?itemName=xaver.clang-format

https://marketplace.visualstudio.com/items?itemName=zxh404.vscode-proto3

```json
"clang-format.style": "{ IndentWidth: 4, BasedOnStyle: Google, AlignConsecutiveAssignments: true, ColumnLimit: 0 }"
```





```yaml
dependencies:
  protobuf: ^2.0.0
```

```dart
import 'dart:io';

import 'protobuf/any.pb.dart' as anyPB;
import 'protobuf/vs.pb.openquote3.pb.dart' as openquotePB;

main(List<String> args) async {
  openquotePB.VsBaseMessage_Req vsBaseMessage_Req = openquotePB.VsBaseMessage_Req();
  vsBaseMessage_Req.method = openquotePB.EnumMethods.REQ_LOGIN;
  vsBaseMessage_Req.reqId = 'login_${DateTime.now().millisecondsSinceEpoch}';
  vsBaseMessage_Req.data = anyPB.Any.pack(openquotePB.ReqLoginField(userName: 'app_pt', password: 'a123456', protocVersion: '3.0'));

  var _ws = await WebSocket.connect('wss://quote.vs.com:8889');
  _ws.add(vsBaseMessage_Req.writeToBuffer());

  _ws.listen(
    (messageBuffer) {
      print('=========================================');
      var packages = openquotePB.VsBaseMessage.fromBuffer(messageBuffer).packages;
      var package = packages[0];

      if (package.canUnpackInto(openquotePB.VsBaseMessage_Rsp())) {
        var response = package.unpackInto<openquotePB.VsBaseMessage_Rsp>(openquotePB.VsBaseMessage_Rsp());
        // print(response.toProto3Json(typeRegistry: TypeRegistry([openquotePB.RspLoginField()])));

        // print(response.errorCode);
        // print(response.data);
        var anyMessage = response.data;

        if (anyMessage.canUnpackInto(openquotePB.RspLoginField())) {
          print('RspLoginField ⬇️⬇️⬇️⬇️⬇️');
          print(anyMessage.unpackInto(openquotePB.RspLoginField()).toProto3Json());
        }
        print(anyMessage.canUnpackInto(openquotePB.RspSubscribedField_ErrContractField()));
      }

      // print(package.canUnpackInto(openquotePB.VsBaseMessage_Rtn.getDefault()));
    },
    onDone: () {
      print('onDone');
    },
    onError: (e, stackTrace) {
      print('onError');
    },
  );
}

```

