[teddysun/xray (docker.com)](https://hub.docker.com/r/teddysun/xray)

## 配置文件

[XTLS/Xray-examples: Some examples of uses for Xray-core. (github.com)](https://github.com/XTLS/Xray-examples)

You **must create a configuration file** `/etc/xray/config.json` in host at first:

```
mkdir -p /etc/xray
```

A sample in JSON like below:

```shell
cat > /etc/xray/config.json <<EOF
🤖JSON Content...🤖
EOF
```

Or some examples of uses for Xray-core https://github.com/XTLS/Xray-examples

## RUN

There is an example to start a container that listen on port `9000`, run as a Xray server like below:

```shell
docker run -d -p 80:80 --name xray --restart=always -v /etc/xray:/etc/xray teddysun/xray && docker logs xray && docker logs xray
```

```shell
docker exec xray cat /etc/xray/config.json
```

```shell
docker logs xray
```

**删除**

```shell
docker stop xray && docker rm xray
```

**Warning**: The port number must be same as configuration and opened in firewall.