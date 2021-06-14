# Plugin 开发

## 数据类型

https://flutter.cn/docs/development/platform-integration/platform-channels?tab=android-channel-java-tab#codec



## podspec

[CocoaPods Guides - Podspec Syntax Reference](https://guides.cocoapods.org/syntax/podspec.html)

[cocoaPods 进行SDK二次包装(cocoapods-packager完成 framework静态库打包,避免第三方库冲突) (juejin.cn)](https://juejin.cn/post/6844903928321015815)



[Pod 创建私有库之引用三方Framework和libs的配置方法_mingmingsuper的专栏-CSDN博客_vendored_frameworks](https://blog.csdn.net/mingmingsuper/article/details/79558406) 

[iOS Flutter插件 导入第三方Framework和添加自定义Assets_酸柠檬的博客-CSDN博客](https://blog.csdn.net/sinat_31177681/article/details/95476489)

```
s.libraries = "c++", "sqlite3.0", "z"
```

你可能要问为什么libz.tbd写成z,很简单这是一个规则要**去除lib和后边的.tbd**，如果遇到配置系统library就需要这么简写，记住就行了。

