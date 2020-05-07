# cli 命令

> ```bash
> nginx -s reload|reopen|stop|quit  #重新加载配置|重启|停止|退出 nginx
> nginx -t   #测试配置是否有语法错误
> 
> nginx [-?hvVtq] [-s signal] [-c filename] [-p prefix] [-g directives]
> 
> -?,-h           : 打开帮助信息
> -v              : 显示版本信息并退出
> -V              : 显示版本和配置选项信息，然后退出
> -t              : 检测配置文件是否有语法错误，然后退出
> -q              : 在检测配置文件期间屏蔽非错误信息
> -s signal       : 给一个 nginx 主进程发送信号：stop（停止）, quit（退出）, reopen（重启）, reload（重新加载配置文件）
> -p prefix       : 设置前缀路径（默认是：/usr/local/nginx/）
> -c filename     : 设置配置文件（默认是：/usr/local/nginx/conf/nginx.conf）
> -g directives   : 设置配置文件外的全局指令
> ```

# 重载配置

```bash
docker-compose exec nginx nginx -s reload
docker exec nginx nginx -s reload
```



```bash
docker exec -it nginx bash
nginx -t
nginx -s reload
```



# 配置文件

```nginx
server {
  listen 80;
  server_name _;
  index index.html;
  root /usr/share/nginx/html;
  # error_page 404 /404.html;

  access_log /var/log/nginx/_.access.log;
  error_log /var/log/nginx/_.error.log;
}
```

## 反向代理

```nginx
  location / {
    proxy_pass http://ziyifarm-api:8080/ziyifarm/;
    proxy_set_header Host api.sauceapp.yanhao.ren;
    proxy_set_header X-Forwarded-For $remote_addr;
  }
```

## https

```nginx
	# SSL-START
  # 使用HSTS
  listen 443 ssl;
	#证书文件
  ssl_certificate /cert_https/api.ziyifarm.com/fullchain.pem;
  #私钥文件
  ssl_certificate_key /cert_https/api.ziyifarm.com/privkey.pem;
  ssl_protocols TLSv1.1 TLSv1.2 TLSv1.3;
  ssl_ciphers ECDHE-RSA-AES128-GCM-SHA256:ECDHE:ECDH:AES:HIGH:!NULL:!aNULL:!MD5:!ADH:!RC4:!DH:!DHE;
  ssl_prefer_server_ciphers on;
  # SSL-END
```

## 强制 https

```nginx
  #HTTP_TO_HTTPS_START
  if ($server_port !~ 443) {
    rewrite ^(/.*)$ https://$host$1 permanent;
  }
  #HTTP_TO_HTTPS_END
```



## 资源缓存

```nginx
  location ~ .*\.(gif|jpg|jpeg|png|bmp|swf)$ {
    expires 30d;
    error_log off;
    access_log off;
  }

  location ~ .*\.(js|css)?$ {
    expires 12h;
    error_log off;
    access_log off;
  }
```

## `SSL`证书验证目录

```NGINX
    #一键申请SSL证书验证目录相关设置
    location ~ \.well-known {
        allow all;
        root /wellknow_dir;
    }
```





# certbot

https://letsencrypt.org/zh-cn/docs/staging-environment/



## certbot 证书步骤

https://certbot.eff.org/docs/using.html#webroot

> **测试环境**
>
> ```bash
> --dry-run
> ```

## 1.NGINX

```nginx
  location ~ /.well-known {
    autoindex on;
    allow all;
    root /wellknown_root;
  }
```

## 2.颁发签证

```bash
docker run -it --rm --name certbot \
 -v "/docker/letsencrypt/etc:/etc/letsencrypt" \
 -v "/docker/letsencrypt/lib:/var/lib/letsencrypt" \
 -v "/docker/letsencrypt/log:/var/log/letsencrypt" \
 -v "/docker/letsencrypt/wellknown_root:/wellknown_root" \
 certbot/certbot certonly --dry-run
```

![image-20200428101448390](https://i.loli.net/2020/04/28/KTPF8fWQRBCSNsu.png)

## 3.重签

```bash
docker run -it --rm --name certbot \
 -v "/docker/letsencrypt/etc:/etc/letsencrypt" \
 -v "/docker/letsencrypt/lib:/var/lib/letsencrypt" \
 -v "/docker/letsencrypt/log:/var/log/letsencrypt" \
 -v "/docker/letsencrypt/wellknown_root:/wellknown_root" \
 certbot/certbot renew --dry-run
# --force-renewal
```

## 自动续签

定时任务`shell`脚本注意 `-d` 执行。

`renew_cert.sh`

添加可执行权限

```bash
chmod +x renew_cert.sh
```

```bash
#!/bin/sh
# Renew Certificate
重签

# reload nginx
重载 NGINX
```

```bash
0 1 */7 * * /docker/renew_cert.sh
# 每隔 7 天续签一次。
```



## certbot 命令

https://blog.ibaoger.com/2017/03/07/certbot-command-line-tool-usage-document/

查看当前的所有的证书信息

```bash
docker run -it --rm --name certbot \
 -v "/docker/letsencrypt/etc:/etc/letsencrypt" \
 -v "/docker/letsencrypt/lib:/var/lib/letsencrypt" \
 -v "/docker/letsencrypt/log:/var/log/letsencrypt" \
 -v "/docker/letsencrypt/wellknown_root:/wellknown_root" \
 certbot/certbot certificates
```

![image-20200428105502724](https://i.loli.net/2020/04/28/WZQKPXYr9pRvfd2.png)

