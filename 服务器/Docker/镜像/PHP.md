# 配置

> ./php/config.d:/usr/local/etc/php/conf.d

timezone.ini

```ini
date.timezone = "Asia/Shanghai"
```

docker-php-ext-sodium.ini

```ini
extension=sodium.so
```



```shell
docker run \
		--name php \
		--restart=always \
		-p 9000:9000 \
		-v /php/conf.d:/usr/local/etc/php/conf.d \
		-v /www/wwwroot:/wwwroot \
		php:fpm
```



# 装mysqli支持

#### 1. 进入容器

```shell
docker exec -it php /bin/bash
```

#### 2 . 切换目录到

```shell
cd /usr/local/bin
```

#### 3. 安装扩展

```shell
./docker-php-ext-install pdo_mysql
./docker-php-ext-install mysqli
exit
```

```shell
docker restart php
```



> 测试模块是否安装,添加文件test.php

```php
<?php 
if (!function_exists('mysqli_init') && !extension_loaded('mysqli')) {
    echo 'We don\'t have mysqli!\n';
} else {
    echo 'Oh yeah, we have it!';
}
```

