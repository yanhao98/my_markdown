```yaml

version: "3"
services:
  # 项目 item
  sauceapp-portal:
    user: "$USERID:$GROUPID"
    image: tomcat:8
    container_name: sauceapp-portal
    depends_on:
      - sauceapp-mysql
      - sauceapp-redis
    environment:
      TZ: Asia/Shanghai
    ports:
      - 8081:8080
    volumes:
      - /projects/sauceapp/log_app/portal_api_logs:/logs
      - /projects/sauceapp/log_tomcat/portal_tomcat_logs:/usr/local/tomcat/logs
      - /projects/sauceapp/apps_tomcat/portal_api_webapps:/usr/local/tomcat/webapps
    restart: always
    networks:
      - sauceapp-network
networks:
  sauceapp-network:
    external: true
EOF
```



```yaml
version: "3.3"
services:
  nginx:
    image: nginx
    # container_name: nginx
    environment:
      TZ: Asia/Shanghai
    ports:
      - 80:80
      - 443:443
    restart: always
    volumes:
      - /etc/localtime:/etc/localtime:ro
      - ./nginx/log:/var/log/nginx
      - ./nginx/conf.d:/etc/nginx/conf.d
      - ./letsencrypt:/letsencrypt
  mysql:
    image: mysql:5.7.30
    # container_name: mysql
    restart: always
    environment:
      TZ: Asia/Shanghai
      MYSQL_ROOT_PASSWORD: n2S6q62gIAauaECW
    ports:
      - 3306:3306
    volumes:
      - /etc/localtime:/etc/localtime:ro
      - ./mysql/data:/var/lib/mysql
  php:
    # container_name: php
    image: php:fpm
    environment:
      TZ: Asia/Shanghai
    restart: always
    volumes:
      - /etc/localtime:/etc/localtime:ro
      - ./nginx/wwwroot:/wwwroot
      - ./php/config.d:/usr/local/etc/php/conf.d

```

