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

# 端口转发

```nginx
  location / {
    proxy_pass http://ziyifarm-api:8080/ziyifarm/;
  }
```

# https

```nginx
	listen 443 ssl;
	#证书文件
  ssl_certificate /cert_https/api.ziyifarm.com/fullchain.pem;
  #私钥文件
  ssl_certificate_key /cert_https/api.ziyifarm.com/privkey.pem;
```

# 资源缓存

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

