[teddysun/xray (docker.com)](https://hub.docker.com/r/teddysun/xray)

## 配置文件

You **must create a configuration file** `/etc/xray/config.json` in host at first:

```
mkdir -p /etc/xray
```

A sample in JSON like below:

```
cat > /etc/xray/config.json <<EOF
{
    "inbounds": [
        {
            "port": 80,
            "protocol": "vless",
            "settings": {
                "clients": [
                    {
                        "id": "36e7e680-7e6c-4270-bcfc-d954dcb9d0e0",
                        "level": 0,
                        "email": "v2rayssr@v2rayssr.com",
                        "flow": "xtls-rprx-direct"
                    }
                ],
                "decryption": "none",
                "fallbacks": [
                    {
                        "dest": 33222
                    },
                    {
                        "alpn": "h2",
                        "dest": 33223
                    }
                ]
            },
            "streamSettings": {
                "network": "tcp",
                "tcpSettings": {
                    "header": {
                        "type": "http",
                        "response": {
                            "version": "1.1",
                            "status": "200",
                            "reason": "OK",
                            "headers": {
                                "Content-Type": [
                                    "application/octet-stream",
                                    "application/x-msdownload",
                                    "text/html",
                                    "application/x-shockwave-flash"
                                ],
                                "Transfer-Encoding": [
                                    "chunked"
                                ],
                                "Connection": [
                                    "keep-alive"
                                ],
                                "Pragma": "no-cache"
                            }
                        }
                    }
                }
            }
        }
    ],
    "outbounds": [
        {
            "protocol": "freedom"
        }
    ]
}
EOF
```

Or some examples of uses for Xray-core https://github.com/XTLS/Xray-examples

## RUN

There is an example to start a container that listen on port `9000`, run as a Xray server like below:

```shell
docker run -d -p 80:80 --name xray --restart=always -v /etc/xray:/etc/xray teddysun/xray
```

```shell
docker exec xray cat /etc/xrayconfig.json
```

```shell
docker logs xray -f
```

**Warning**: The port number must be same as configuration and opened in firewall.



## 测速

```
docker run -d -p 6688:80 ilemonrain/html5-speedtest:alpine
```