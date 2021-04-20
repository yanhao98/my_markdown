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
curl --socks5 127.0.0.1:1086 ipinfo.io/json

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



# 安装brew

```shell
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

```shell
brew upgrade
```

