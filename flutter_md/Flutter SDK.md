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

![image-20200507141836100](https://i.loli.net/2020/05/07/lPU82kNTWfgzLpC.png)





# 设置 iOS 开发环境

```shell
sudo xcode-select --switch /Applications/Xcode.app/Contents/Developer
sudo xcodebuild -runFirstLaunch
```

```shell
sudo xcode-select --switch /Applications/Xcode-beta.app/Contents/Developer
sudo xcodebuild -runFirstLaunch
```

