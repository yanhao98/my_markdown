# 刷新DNS缓存

```
sudo killall -HUP mDNSResponder
```



# 设置终端代理

```bash
export http_proxy=http://127.0.0.1:1087;export https_proxy=http://127.0.0.1:1087;
```

检查

```
curl --socks5 127.0.0.1:1081 ipinfo.io/json

curl ipinfo.io/json

ip.gs
cip.cc
ip.sb
```

![image-20200406125736016](http://mdpic.yanhao.ren/0ce8473b38f8905bc4b91c8a4c657eab.jpg)



# Curl

```bash
curl ip.sb
```

## 查看耗时

```bash
-w '\ntime_connect\t\t%{time_connect}\ntime_starttransfer\t%{time_starttransfer}\ntime_total\t\t%{time_total}\n'
```

![image-20200418152743251](http://mdpic.yanhao.ren/25403c6dbe5b875f2b17ea6a155fe137.png)



# 环境变量

```bash
open -a TextEdit ~/.bash_profile
open -a 'Visual Studio Code.app' .zshrc
open .zshrc

# 修改文件后需要刷新
source .zshrc
```

```
# source .zshrc

# Add RVM to PATH for scripting. Make sure this is the last PATH variable change.
export PATH="$PATH:$HOME/.rvm/bin"

# =========================================    flutter start

# 🐞 dev SDK
# export PATH=/Users/yanhao/development/flutter_sdk_dev/bin:$PATH
# ✅ stable SDK
export PATH=/Users/yanhao/development/flutter_sdk_stable/bin:$PATH

# =========================================    flutter end

# export PUB_HOSTED_URL=https://pub.flutter-io.cn
# export FLUTTER_STORAGE_BASE_URL=https://storage.flutter-io.cn


NO_PROXY=localhost,127.0.0.1
# HomeBrew
export HOMEBREW_BOTTLE_DOMAIN=https://mirrors.ustc.edu.cn/homebrew-bottles
export PATH="/usr/local/bin:$PATH"
export PATH="/usr/local/sbin:$PATH"
# HomeBrew END

# 📱adb
export PATH="/Users/yanhao/Library/Android/sdk/platform-tools:$PATH"

```



# 安装brew

```shell
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

```shell
brew upgrade
```

