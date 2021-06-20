

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

