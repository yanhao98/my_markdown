# Haproxy

https://visonnn.gitbooks.io/haproxy-1-7/content/

[HAProxy 日志输出及配置](https://www.cnblogs.com/jiagoushi/p/10186949.html)

[使用haproxy中转Shadowsocks流量](https://libertyleadingnetwork.blogspot.com/2019/03/haproxyshadowsocks.html)

[Haproxy基础知识 -运维小结](https://www.cnblogs.com/kevingrace/p/6138150.html)

[V2Ray 多协议多服务器情况使用 HAProxy 负载均衡](https://xzos.net/load-balancing-v2ray-with-haproxy-and-docker/#V2Ray_%E5%85%A5%E5%8F%A3%E9%85%8D%E7%BD%AE)

```
yum list|grep haproxy
```

```
yum install haproxy18u -y
```

开机启动

```
systemctl enable haproxy18
```



配置文件：

```
/etc/haproxy/haproxy.cfg
```

启动：

```
service haproxy start
```

停止：

```
service haproxy stop
```

重启：

```
service haproxy restart
```

状态：

```
service haproxy status
```



## Vmess 和 Web 共用 80端口

```
global
    log         127.0.0.1 local2
    chroot      /var/lib/haproxy18
    # pidfile     /var/run/haproxy18.pid
    # maxconn     4000
    user        haproxy
    group       haproxy
    daemon

    # turn on stats unix socket
    stats socket /var/lib/haproxy18/stats

#---------------------------------------------------------------------
# common defaults that all the 'listen' and 'backend' sections will
# use if not designated in their block
#---------------------------------------------------------------------
defaults
    mode                    tcp
    log                     global
    # option                  httplog
    option                  dontlognull
    # option http-server-close
    # option forwardfor       except 127.0.0.0/8
    # option                  redispatch
    # retries                 3
    # timeout http-request    10s
    # timeout queue           1m
    timeout connect         10s
    timeout client          300s
    timeout server          300s
    # timeout client          1m
    # timeout server          1m
    # timeout http-keep-alive 10s
    # timeout check           10s
    # maxconn                 3000

#---------------------------------------------------------------------
# main frontend which proxys to the backends
#---------------------------------------------------------------------
frontend main
    bind *:80
    tcp-request inspect-delay 5s
    tcp-request content accept  if HTTP
    use_backend web             if HTTP
    default_backend             vmess


backend web
    server server1 127.0.0.1:888

backend vmess
    server server1 127.0.0.1:9000
```





## demo

```


  acl is-draw hdr_dom(host) -i draw.example.org

  acl valid_http_method method GET HEAD OPTIONS





hdr_beg(host) www.    #前缀匹配，header中host字段以www.开头
hdr_end(host) .com    #后缀匹配，header中host字段以.com结尾
hdr_dom(host) www.ssy.org  #域匹配，header中host字段为www.ssy.org
hdr_dir(host)  #路径匹配，header的uri路径
hdr_sub(User-agent)   #子串匹配，包含,header中的模糊匹配
```

