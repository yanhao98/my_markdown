[TOC]

# 安装

## 安装docker

```bash
curl -fsSL https://get.docker.com/ | sh
```

## 配置国内镜像源

https://www.daocloud.io/mirror

## Docker Compose

https://docs.docker.com/compose/install/

```bash
sudo curl -L "https://get.daocloud.io/docker/compose/releases/download/1.25.5/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
```

### pip 安装 Docker Compose

```bash
yum -y install epel-release
 
yum -y install python-pip
 
#升级
pip install --upgrade pip

pip install docker-compose
docker-compose -version
```



### docker-compose 使用

```
docker-compose -f /xx.yml up -d
```

```bash
GID="$(id -g)" docker-compose config
```



## 卸载

```bash
yum list installed | grep docker
```

![image-20200425094839457](https://i.loli.net/2020/04/25/zwpWQkU6yV5OcKh.png)





# 服务管理

```
systemctl start docker  #启动docker
systemctl enable docker #开机启动docker
systemctl status docker #查看docker状态
```

**systemctl 方式**

守护进程重启

```
systemctl daemon-reload
```

 重启docker服务

```
systemctl restart docker
```

 关闭docker

```
systemctl stop docker
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

```
docker network --help
```

```
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



# 日志

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

![image-20200415115422979](https://tva1.sinaimg.cn/large/007S8ZIlgy1gdubacwhujj30na02ut9t.jpg)



## cloud-torrent

```
docker run -d -p 3000:3000 --name cloud-torrent --restart=always -v /home/docker/cloud-torrent/downloads:/downloads -v /home/docker/cloud-torrent/torrents:/torrents boypt/cloud-torrent
```



