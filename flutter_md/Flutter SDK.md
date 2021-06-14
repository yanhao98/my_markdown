# 安装升级 SDK

```bash
flutter channel	#查看当前渠道
flutter channel beta / flutter channel master	# 更改渠道
flutter packages get	#获取pubspec.yaml中所有的依赖关系
flutter packages upgrade	# 获取pubspec.yaml中所有列表中的依赖项的最新版本

flutter upgrade # 更新 SDK

flutter --version
```

```bash
open ~/.zshrc
source .zshrc
```

```bash
export PUB_HOSTED_URL=https://pub.flutter-io.cn
export FLUTTER_STORAGE_BASE_URL=https://storage.flutter-io.cn
```

<img src="https://tva1.sinaimg.cn/large/00831rSTgy1gdnax935lxj30fe05uwfr.jpg" alt="image-20200409102250243" align="left" style="zoom:50%;" />

```shell
flutter pub outdated
```

```shell
dart pub get
```

![image-20200507141836100](http://mdpic.yanhao.ren/c5d40e74e87dae22a4da6e328c5c1b2e.png)

```
flutter channel master
flutter upgrade
```

```
flutter channel stable
flutter upgrade
```

