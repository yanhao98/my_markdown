[ssh_config和sshd_config区别](https://www.cnblogs.com/panda2046/p/5933498.html)

# 用户/用户组

https://www.cnblogs.com/nyfz/p/8557137.html

https://www.runoob.com/linux/linux-comm-useradd.html

## 添加用户

新建用户 yanhao 并增加到 sauceapp 工作组

```
useradd -g sauceapp yanhao
```

## 修改用户

usermod

参数和useradd一样

## 用户密码

```
passwd yanhao
```

```
passwd #修改当前用户
```

## 删除用户

- ```
  userdel -r 用户名
  ```

- 此命令删除用户sam在系统文件中（主要是/etc/passwd, /etc/shadow, /etc/group等）的记录，同时删除用户的主目录。



## 建工作组

```bash
groupadd sauceapp
```

## 添加用户到组

**将userA添加到groupB用户组里面：** 

```bash
gpasswd -a userA groupB
sudo usermod -aG docker your-user
```

**注意**：添加用户到某一个组可以使用 `usermod -G groupB userA` 这个命令可以添加一个用户到指定的组，但是以前添加的组就会清空掉，

将userA设置为groupA的群组管理员：

```
gpasswd -A userA groupA
```

## 从组中删除用户

编辑/etc/group 找到GROUP1那一行，删除 user 

或者用命令 gpasswd -d user GROUP



## 切换群组

```bash
newgrp xx
```



## 用户列表

**所有用户的列表**

```bash
cat /etc/passwd
```

查看当前活跃的用户列表

```bash
w
```

查看用户组

```bash
cat /etc/group
```

当前登录用户的组内成员

```bash
groups
```

### /etc/group 内容分析

```
cat /etc/group
```

- 在/etc/group 中的每条记录分四个字段：
- 第一字段：用户组名称；
- 第二字段：用户组密码；
- 第三字段：GID
- 第四字段：用户列表，每个用户之间用,号分割；本字段可以为空；如果字段为空表示用户组为GID的用户名；

# 系统权限

https://blog.csdn.net/g425680992/article/details/79226826



# 文件属性

https://www.runoob.com/linux/linux-file-attr-permission.html

## chgrp 更改文件属组

语法：

```bash
chgrp [-R] 属组名 文件名
```

参数选项

- -R：递归更改文件属组，就是在更改某个目录文件的属组时，如果加上-R的参数，那么该目录下的所有文件的属组都会更改。

## chown：更改文件属主

**也可以同时更改文件属组**

语法：

```ashb
sudo chown [–R] 属主名 文件名
sudo chown [-R] 属主名：属组名 文件名
```

进入 /root 目录（~）将install.log的拥有者改为bin这个账号：

```bash
[root@www ~] cd ~
[root@www ~]# chown bin install.log
[root@www ~]# ls -l
-rw-r--r--  1 bin  users 68495 Jun 25 08:53 install.log
```

将install.log的拥有者与群组改回为root：

```bash
[root@www ~]# chown root:root install.log
[root@www ~]# ls -l
-rw-r--r--  1 root root 68495 Jun 25 08:53 install.log
```

## chmod

```bash
chmod 700 -R .ssh
chmod 600 .ssh/authorized_keys
```



# 文件操作

## 移动文件 

把d1目录的中的test.txt移动到n1目录下面

**重命名也是`mv`**

```bash
mv /tmp/d1/test.txt /tmp/n1
```



# 配置 RSA 登录

生成`SSH`秘钥

- -t:密钥类型；
- -b:加密位数；
- -C：注释，通常为邮箱便于识别

```
ssh-keygen -t rsa -b 4096 -C "YanhaoMac"
ssh-keygen -t rsa -b 2048 -C "me@yanhao.ren"
```

**将公钥追加到 authorized_keys 中**

```bash
cat id_rsa.pub >> /root/.ssh/authorized_keys 
```

**设置authorized_keys权限**

**要保证.ssh和authorized_keys都只有用户自己有写权限。否则验证无效。**

```bash
chmod 700 -R .ssh
chmod 600 .ssh/authorized_keys
```



修改 sshd_config 文件

```
vi  /etc/ssh/sshd_config
#禁用root账户登录，如果是用root用户登录请开启

PermitRootLogin no
UsePAM no
PasswordAuthentication no
PubkeyAuthentication yes
RSAAuthentication yes

AuthorizedKeysFile .ssh/authorized_keys
MaxAuthTries 3
UsePAM
```

**重启sshd.service服务，则配置成功，完毕**

```bash
sudo systemctl restart sshd.service
sudo systemctl start sshd.service
sudo systemctl stop sshd.service
sudo systemctl restart sshd
sudo systemctl status sshd.service
```

## 注意

添加用户后 **修改用户的登录shell**

```bash
sudo usermod -s /bin/bash yanhao
```

## 问题

### 权限问题

.ssh目录，以及/home/当前用户 需要700权限，参考以下操作调整

sudo chmod 700 ~/.ssh

sudo chmod 700 /home/当前用户
.ssh目录下的authorized_keys文件需要600或644权限，参考以下操作调整

sudo chmod 600 ~/.ssh/authorized_keys

### StrictModes问题

编辑

> sudo vi /etc/ssh/sshd_config

找到

> StrictModes yes

改成

> StrictModes no

如果还不行，可以用ssh -vvv 目标机器ip 查看详情，根据输出内容具体问题具体分析了



# 端口

```bash
yum install net-tools
```

## 查看端口被哪个进程使用

```
sudo netstat -lnp | grep 8031
```

## 查看所有监听的端口

```
sudo netstat -lnpt
```



# 进程

## 进程PID所在的目录

```bash
sudo pwdx 31996
```

![image-20200415120509724](https://tva1.sinaimg.cn/large/007S8ZIlgy1gdublj6n7xj30h601wjrm.jpg)



# 日志

```
/var/log/message` `– 记录系统日志或当前活动日志。
/var/log/auth``.log – 身份认证日志。
/var/log/cron` `– Crond 日志 (``cron` `任务).
/var/log/maillog` `– 邮件服务器日志。
/var/log/secure` `– 认证日志。
/var/log/wtmp` `历史登录、注销、启动、停机日志和，lastb命令可以查看登录失败的用户
/var/run/utmp` `当前登录的用户信息日志，w、``who``命令的信息便来源与此
/var/log/yum``.log Yum 日志。
```



## 登录日志：

```bash
cat /var/log/secure
```

单独执行lastb指令，它会读取位于/var/log目录下，名称为btmp的文件，并把该文件内容

记录的登入失败的用户名单，全部显示出来。



# top

**top 命令，然后按数字“1”可监控每个逻辑CPU的状况：**

默认top命令是3秒刷新一次，可以加参数改成1秒。

即

```bash
top -d 1
```

按cpu排序：
top命令后，输入**大写的P**

按内存排序：
top命令后，输入**大写的M**



# 定时任务

日志：https://www.cnblogs.com/yangzailu/p/10137918.html

```bash
service crond status
systemctl restart crond
```

## 配置文件

- /var/spool/cron/ 目录下存放的是每个用户包括root的crontab任务，每个任务以创建者的名字命名
- /etc/crontab 这个文件负责调度各种管理和维护任务。
- /etc/cron.d/ 这个目录用来存放任何要执行的crontab文件或脚本。
- 我们还可以把脚本放在/etc/cron.hourly、/etc/cron.daily、/etc/cron.weekly、/etc/cron.monthly目录中，让它每小时/天/星期、月执行一次。

## 命令

查看 crontab

```bash
cat /etc/crontab
```

```bash
crontab -u # 设定特定用户的定时服务
crontab -l # 列出当前用户定时服务内容
crontab -r # 删除当前用户的定时服务
crontab -e # 编辑当前用户的定时服务
```

## 概念

crontab -e

\#此时会进入vi的编辑界面让你编辑工作。注意到，每项工作都是一行。

```bash
Minute Hour Day Month Dayofweek   command
```

| 代表意义 | 分钟 | 小时 | 日期 | 月份 | 周   |
| -------- | ---- | ---- | ---- | ---- | ---- |
| 数字范围 | 0~59 | 0~23 | 1~31 | 1~12 | 0~7  |


周的数字为0或7时，都代表“星期天”的意思。另外，还有一些辅助的字符，大概有下面这些：

| 特殊字符 | 代表意义                                                     |
| ------- | ----------------------|
| *(星号)  | 代表任何时刻都接受的意思。举例来说，范例一内那个日、月、周都是*，就代表着不论何月、何日的礼拜几的12：00都执行后续命令的意思。 |
| ,(逗号)  | 代表分隔时段的意思。举例来说，如果要执行的工作是3：00与6：00时，就会是：0 3,6 * * * command时间还是有五列，不过第二列是 3,6 ，代表3与6都适用 |
| -(减号)  | 代表一段时间范围内，举例来说，8点到12点之间的每小时的20分都进行一项工作：20 8-12 * * * command仔细看到第二列变成8-12.代表 8,9,10,11,12 都适用的意思 |
| /n(斜线) | 那个n代表数字，即是每隔n单位间隔的意思，例如每五分钟进行一次，则：*/5 * * * * command用*与/5来搭配，也可以写成0-59/5，意思相同 |

##  例子

### 实例1：每1分钟执行一次myCommand

```
*/5 * * * * /bin/sh /docker/renew_cert.sh
```
