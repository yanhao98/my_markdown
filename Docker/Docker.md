[TOC]

# 安装

## 安装docker

```shell
curl -fsSL https://get.docker.com/ | sh
```

启动docker

```shell
systemctl start docker && systemctl enable docker && systemctl status docker
```

## Docker Compose

https://docs.docker.com/compose/install/

https://github.com/docker/compose/releases/

```shell
sudo curl -L "https://get.daocloud.io/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose \
&& chmod +x /usr/local/bin/docker-compose && docker-compose -version
```

```shell
sudo curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
```

关闭docker

```shell
systemctl stop docker
```

## 配置国内镜像源

```shell
tee /etc/docker/daemon.json <<-'EOF'
{
  "registry-mirrors": [
    "https://29ra4ouu.mirror.aliyuncs.com",
    "http://hub-mirror.c.163.com",
    "https://docker.mirrors.ustc.edu.cn",
    "https://registry.docker-cn.com"
  ]
}
EOF
```

https://www.daocloud.io/mirror

# 卸载

```bash
yum list installed | grep docker
```

![image-20200425094839457](http://mdpic.yanhao.ren/ea58f8723ba2f9cf78b92810ef32d49f.png)



```shell
yum remove $(rpm -qa | grep docker)
```





# 给用户docker权限

- sudo groupadd docker     #添加docker用户组 
- sudo gpasswd -a $USER docker     #将登陆用户加入到docker用户组中 
  -  usermod -G docker yanhao
- newgrp docker     #更新用户组
-  随便输一个 docker 命令测试是否有权限



```ruby
songyanyan@songyanyan:~$ sudo group add docker
[sudo] songyanyan 的密码： 
sudo: group：找不到命令
songyanyan@songyanyan:~$ sudo groupadd docker #添加docker用户组
groupadd：“docker”组已存在
songyanyan@songyanyan:~$ sudo gpasswd -a $USER docker #将登陆用户加入到docker用户组中
正在将用户“songyanyan”加入到“docker”组中
songyanyan@songyanyan:~$ newgrp docker #更新用户组
songyanyan@songyanyan:~$ docker xxx
```







# 容器操作

## 进入容器

```bash
docker exec -it nginx /bin/bash
docker exec -it mysql /bin/bash
docker exec -it redis redis-cli
```

## 更新容器参数

```bash
docker container update --restart=always mysql3306
docker update --restart=always asp3000_biquge
```

## 查看容器运行的各种数据

```bash
docker inspect redis
```

## 容器列表

```
docker ps --help

Usage:  docker ps [OPTIONS]

List containers

Options:
  -a, --all             Show all containers (default shows just running)
  -f, --filter filter   Filter output based on conditions provided
      --format string   Pretty-print containers using a Go template
  -n, --last int        Show n last created containers (includes all states) (default -1)
  -l, --latest          Show the latest created container (includes all states)
      --no-trunc        Don't truncate output
  -q, --quiet           Only display numeric IDs
  -s, --size            Display total file sizes
```





# 容器内 yum

```bash
# 使用国内镜像
mv /etc/apt/sources.list /etc/apt/sources.list.bak
echo "deb http://mirrors.163.com/debian/ jessie main non-free contrib" >> /etc/apt/sources.list
echo "deb http://mirrors.163.com/debian/ jessie-proposed-updates main non-free contrib" >>/etc/apt/sources.list
echo "deb-src http://mirrors.163.com/debian/ jessie main non-free contrib" >>/etc/apt/sources.list
echo "deb-src http://mirrors.163.com/debian/ jessie-proposed-updates main non-free contrib" >>/etc/apt/sources.list
# 更新apt-get指令
apt-get update
apt-get install yum
```



# 查看资源使用

[docker stats](https://docs.docker.com/engine/reference/commandline/stats/)

```bash
docker stats [OPTIONS] [CONTAINER...]
```

## Options

| Name, shorthand | Default | Description                                            |
| --------------- | ------- | ------------------------------------------------------ |
| `--all , -a`    |         | Show all containers (default shows just running)       |
| `--format`      |         | Pretty-print images using a Go template                |
| `--no-stream`   |         | Disable streaming stats and only pull the first result |
| `--no-trunc`    |         | Do not truncate output                                 |

### 格式化输出的

```bash
docker stats --format "table {{.Name}}\t{{.CPUPerc}}\t{{.MemPerc}}\t{{.MemUsage}}"
docker stats --format "table {{.Name}}\t{{.CPUPerc}}\t{{.MemPerc}}\t{{.MemUsage}}" --no-stream
```

#### 格式化占位符

| 占位符     | 说明                                    |
| ---------- | --------------------------------------- |
| .Container | 根据用户指定的名称显示容器的名称或 ID。 |
| .Name      | 容器名称。                              |
| .ID        | 容器 ID。                               |
| .CPUPerc   | CPU 使用率。                            |
| .MemUsage  | 内存使用量。                            |
| .NetIO     | 网络 I/O。                              |
| .BlockIO   | 磁盘 I/O。                              |
| .MemPerc   | 内存使用率。                            |
| .PIDs      | PID 号。                                |





# 网卡

```bash
docker network --help
```

```bash
Manage networks

Commands:
  connect     Connect a container to a network
  create      Create a network
  disconnect  Disconnect a container from a network
  inspect     Display detailed information on one or more networks
  ls          List networks
  prune       Remove all unused networks
  rm          Remove one or more networks
```

--net是网络模式，这里用的host模式。即和宿主机共用一个网络命名空间，就是使用宿主机的IP和端口。

也可以使用 -p 3306:3306这种方式来映射端口。

但是它俩不能一起使用哦。



# 卷标

```shell
Usage:	docker volume COMMAND

Manage volumes

Commands:
  create      Create a volume
  inspect     Display detailed information on one or more volumes
  ls          List volumes
  prune       Remove all unused local volumes
  rm          Remove one or more volumes
```



# 数据拷贝

```
docker cp /root/www 96f7f14e99ab:/www
```

# Volumes迁移

## 备份

> --volumes-from 后面是容器名字
>
> 备份容器`sentry_postgres_1`内的目录：`/var/lib/postgresql/data`

```shell
mkdir backup
cd backup
docker run --rm -it \
    --volumes-from sentry_postgres_1 \
    -v$(pwd):/backup \
    ubuntu \
    tar cvf /backup/data.tar /var/lib/postgresql/data
```

## 恢复

**恢复的目标**

```shell
docker run -it --name vol_bck -v /data ubuntu /bin/bash
```

执行恢复

> 修改 `容器名` 和 `tar包名`就可以了
>
> -C 后面的目录不用修改

```shell
cd backup
docker run -it --rm \
    --volumes-from sentry_postgres_1 \
    -v $(pwd):/backup \
    ubuntu \
    tar xvf /backup/data.tar -C /
```



# 日志

https://tecadmin.net/truncate-docker-container-logfile/

```bash
$ docker logs [OPTIONS] CONTAINER
  Options:
        --details        显示更多的信息
    -f, --follow         跟踪实时日志
        --since string   显示自某个timestamp之后的日志，或相对时间，如42m（即42分钟）
        --tail string    从日志末尾显示多少行日志， 默认是all
    -t, --timestamps     显示时间戳
        --until string   显示自某个timestamp之前的日志，或相对时间，如42m（即42分钟）
```

**例子：**

查看指定时间后的日志，只显示最后100行：

```shell
$ docker logs -f -t --since="2018-02-08" --tail=100 CONTAINER_ID
```

查看最近30分钟的日志:

```shell
$ docker logs --since 30m CONTAINER_ID
```

查看某时间之后的日志：

```shell
$ docker logs -t --since="2018-02-08T13:23:37" CONTAINER_ID
```

查看某时间段日志：

```shell
$ docker logs -t --since="2018-02-08T13:23:37" --until "2018-02-09T12:23:37" CONTAINER_ID
```







# 镜像

## MySQL

自定义配置文件

![image-20200415115422979](http://mdpic.yanhao.ren/69f664dc27b7238ad73839651381f97a.jpg)



## l2tp

hwdsl2/ipsec-vpn-server

https://hub.docker.com/r/teddysun/l2tp

### 加载 IPsec af_key 内核模块

首先需要在 Docker 主机上加载 IPsec af_key 内核模块

```bash
sudo modprobe af_key
```

### 创建配置文件

```shell
cat > /etc/l2tp.env <<EOF
VPN_IPSEC_PSK=YanhaoVPNPSK
VPN_USER=yanhao
VPN_PASSWORD=vpnpass
VPN_PUBLIC_IP=
VPN_L2TP_NET=
VPN_L2TP_LOCAL=
VPN_L2TP_REMOTE=
VPN_XAUTH_NET=
VPN_XAUTH_REMOTE=
VPN_DNS1=
VPN_DNS2=
VPN_SHA2_TRUNCBUG=
EOF
```

```shell
cat > /etc/l2tp.env <<EOF
VPN_IPSEC_PSK=YanhaoVPNPSK
VPN_USER=yanhao
VPN_PASSWORD=vpnpass
EOF
```

```shell
docker run \
    --name l2tp \
    --env-file /etc/l2tp.env \
    -p 500:500/udp \
    -p 4500:4500/udp \
    -v /lib/modules:/lib/modules:ro \
    -d --privileged \
    --restart=always \
    hwdsl2/ipsec-vpn-server
    
    teddysun/l2tp
```

```shell
docker logs l2tp -f
```

```shell
docker stop l2tp
docker rm l2tp
```
