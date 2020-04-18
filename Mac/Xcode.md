# 问题解决

## Transporter

上传 App Store 卡住

http://blog.sina.com.cn/s/blog_89d594e30102z8v2.html

https://bbs.huaweicloud.com/blogs/143841

## cocopods

卸载重装

```bash
sudo gem uninstall cocoapods
gem install cocoapods
rm -rf ~/.cocoapods/repos/trunk/
pod repo remove trunk
```

## **pod**缓存清理

查看看缓存

```bash
pod cache list
```

清除缓存 

```bash
pod cache clean --all
```

更新本地的索引文件

```bash
pod update --no-repo-update
```

