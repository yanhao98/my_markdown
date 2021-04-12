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

