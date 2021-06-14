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


