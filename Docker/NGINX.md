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

https://xuexb.github.io/learn-nginx/

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
location /portal/ {
  proxy_pass http://sauceapp-portal:8080/;
  proxy_set_header X-Forwarded-Prefix /portal/;
  proxy_set_header Host api.sauceapp.yanhao.ren;
  proxy_set_header X-Forwarded-For $remote_addr;
}
```

## https

```nginx
  # / SSL
  # 使用HSTS
  add_header Strict-Transport-Security "max-age=31536000";
  listen 443 ssl;
  ssl_certificate /letsencrypt/etc/live/api.sauceapp.yanhao.ren/fullchain.pem;
  ssl_certificate_key /letsencrypt/etc/live/api.sauceapp.yanhao.ren/privkey.pem;
  ssl_protocols TLSv1.1 TLSv1.2 TLSv1.3;
  ssl_ciphers ECDHE-RSA-AES128-GCM-SHA256:ECDHE:ECDH:AES:HIGH:!NULL:!aNULL:!MD5:!ADH:!RC4:!DH:!DHE;
  ssl_prefer_server_ciphers on;
  # // SSL
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



# certbot

https://letsencrypt.org/zh-cn/docs/staging-environment/

https://certbot.eff.org/docs/using.html



## certbot 证书步骤

> **测试环境**
>
> ```bash
> --dry-run
> --staging
> ```

## 1.NGINX

```
-v "./letsencrypt:/letsencrypt"
```

```nginx
location ~ /.well-known {
  autoindex on;
  allow all;
  root /letsencrypt/wellknown_root;
}
```

## 2.颁发签证

**注意目录**

```bash
docker run -it --rm --name certbot \
-v "/projects/LNMP/letsencrypt/etc:/etc/letsencrypt" \
-v "/projects/LNMP/letsencrypt/lib:/var/lib/letsencrypt" \
-v "/projects/LNMP/letsencrypt/log:/var/log/letsencrypt" \
-v "/projects/LNMP/letsencrypt/wellknown_root:/wellknown_root" \
certbot/certbot certonly \
--webroot -w /wellknown_root \
-m "gt.yanhao.98@icloud.com" \
--agree-tos \
--no-eff-email \
--dry-run
```

## 3.重签

```bash
docker run -d --rm --name certbot \
-v "/projects/LNMP/letsencrypt/etc:/etc/letsencrypt" \
-v "/projects/LNMP/letsencrypt/lib:/var/lib/letsencrypt" \
-v "/projects/LNMP/letsencrypt/log:/var/log/letsencrypt" \
-v "/projects/LNMP/letsencrypt/wellknown_root:/wellknown_root" \
certbot/certbot renew \
--force-renewal \
--dry-run \
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
重签代码

# reload nginx
重载 NGINX
```

```bash
0 1 */7 * * /projects/LNMP/letsencrypt/renew_cert.sh
# 每隔 7 天续签一次。
```

## 其他命令

```
manage:
  Various subcommands and flags are available for managing your
  certificates:

  certificates          List certificates managed by Certbot
  delete                Clean up all files related to a certificate
  renew                 Renew all certificates (or one specified with --cert-
                        name)
  revoke                Revoke a certificate specified with --cert-path or
                        --cert-name
  update_symlinks       Recreate symlinks in your /etc/letsencrypt/live/
                        directory
```

