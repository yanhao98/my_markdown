# 生成key

```shell
docker run --rm sentry config generate-secret-key
```



# 创建compose文件

**注意**替换 `SENTRY_SECRET_KEY`和`邮箱信息`

```yaml
version: "3"
services:
  redis:
    image: redis:latest
    restart: always
    volumes: 
      - redisdb:/data
  postgres:
    image: postgres:latest
    restart: always
    environment:
      - POSTGRES_USER=sentry
      - POSTGRES_PASSWORD=sentry
      - POSTGRES_DBNAME=sentry
      - POSTGRES_DBUSER=sentry
      - POSTGRES_DBPASS=sentry
    volumes:
      - postgresdb:/var/lib/postgresql/data
  sentry:
    image: sentry:latest
    restart: always
    links:
      - redis
      - postgres
    ports:
      - 9000:9000
    environment:
      SENTRY_SECRET_KEY: "=suv4b3_3m&th5=ufq%6ez#db)#!hm0z^1xp@rooo%+yjs8_0!"
      SENTRY_POSTGRES_HOST: postgres
      SENTRY_DB_USER: sentry
      SENTRY_DB_PASSWORD: sentry
      SENTRY_REDIS_HOST: redis
      SENTRY_EMAIL_HOST: smtp.mxhichina.com
      SENTRY_EMAIL_PORT: 25
      SENTRY_SERVER_EMAIL: sentry@yanhao.ren
      SENTRY_EMAIL_USER: sentry@yanhao.ren
      SENTRY_EMAIL_PASSWORD: <password>
  cron:
    image: sentry:latest
    restart: always
    links:
      - redis
      - postgres
    command: "sentry run cron"
    environment:
      SENTRY_SECRET_KEY: "=suv4b3_3m&th5=ufq%6ez#db)#!hm0z^1xp@rooo%+yjs8_0!"
      SENTRY_POSTGRES_HOST: postgres
      SENTRY_DB_USER: sentry
      SENTRY_DB_PASSWORD: sentry
      SENTRY_REDIS_HOST: redis
      SENTRY_EMAIL_HOST: smtp.mxhichina.com
      SENTRY_EMAIL_PORT: 25
      SENTRY_SERVER_EMAIL: sentry@yanhao.ren
      SENTRY_EMAIL_USER: sentry@yanhao.ren
      SENTRY_EMAIL_PASSWORD: <password>
  worker:
    image: sentry:latest
    restart: always
    links:
      - redis
      - postgres
    command: "sentry run worker"
    environment:
      SENTRY_SECRET_KEY: "=suv4b3_3m&th5=ufq%6ez#db)#!hm0z^1xp@rooo%+yjs8_0!"
      SENTRY_POSTGRES_HOST: postgres
      SENTRY_DB_USER: sentry
      SENTRY_DB_PASSWORD: sentry
      SENTRY_REDIS_HOST: redis
      SENTRY_EMAIL_HOST: smtp.mxhichina.com
      SENTRY_EMAIL_PORT: 25
      SENTRY_SERVER_EMAIL: sentry@yanhao.ren
      SENTRY_EMAIL_USER: sentry@yanhao.ren
      SENTRY_EMAIL_PASSWORD: <password>
volumes:
  postgresdb:
  redisdb:
```

# 初始化数据库

```shell
docker-compose up -d
docker exec -it sentry_sentry_1 sentry upgrade
docker exec -it sentry_sentry_1 sentry createuser
```

```
me@yanhao.ren
KPHMCJMLGOFHANSW
```

```shell
docker-compose down
```



