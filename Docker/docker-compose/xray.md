# Xray + Adguard Home

## 创建项目目录

```shell
mkdir -p /xray && mkdir -p /xray/adguardhome/conf
```

## Xray配置文件

```json
cat > /xray/config.json <<EOF
{
    "log": {
        "access": "/var/log/xray/access.log",
        "error": "/var/log/xray/error.log",
        "loglevel": "debug",
        "dnsLog": true
    },
    "inbounds": [
        {
            "port": 80,
            "protocol": "vless",
            "settings": {
                "clients": [
                    {
                        "id": "36e7e680-7e6c-4270-bcfc-d954dcb9d0e0", // 填写你的 UUID
                        "level": 0,
                        "email": "http@yanhao.ren"
                    }
                ],
                "decryption": "none",
                "fallbacks": [
                    {
                        "dest": "3000"
                    },
                    {
                        "path": "/chinaunicom", // 必须换成自定义的 PATH
                        "dest": 1234,
                        "xver": 1
                    }
                ]
            },
            "streamSettings": {
                "network": "tcp",
                "security": "none" /* ,
                "security": "none",
                "tlsSettings": {
                    "alpn": [
                        "http/1.1"
                    ],
                    "certificates": [
                        {
                            "certificateFile": "/path/to/fullchain.crt", // 换成你的证书，绝对路径
                            "keyFile": "/path/to/private.key" // 换成你的私钥，绝对路径
                        }
                    ]
                } */
            }
        },
        {
            "port": 1234,
            "listen": "127.0.0.1",
            "protocol": "vless",
            "settings": {
                "clients": [
                    {
                        "id": "36e7e680-7e6c-4270-bcfc-d954dcb9d0e0", // 填写你的 UUID
                        "level": 0,
                        "email": "ws@yanhao.ren"
                    }
                ],
                "decryption": "none"
            },
            "streamSettings": {
                "network": "ws",
                "security": "none",
                "wsSettings": {
                    "acceptProxyProtocol": true, // 提醒：若你用 Nginx/Caddy 等反代 WS，需要删掉这行
                    "path": "/chinaunicom" // 必须换成自定义的 PATH，需要和上面的一致
                }
            }
        }
    ],
    "outbounds": [
        // 5.1 第一个出站是默认规则，freedom就是对外直连（vps已经是外网，所以直连）
        {
            "tag": "direct",
            "protocol": "freedom"
        },
        // 5.2 屏蔽规则，blackhole协议就是把流量导入到黑洞里（屏蔽）
        {
            "tag": "block",
            "protocol": "blackhole"
        }
    ],
    "routing": {
        "domainStrategy": "AsIs",
        "rules": [
            // 3.1 防止服务器本地流转问题：如内网被攻击或滥用、错误的本地回环等
            {
                "type": "field",
                "ip": [
                    "geoip:private" // 分流条件：geoip文件内，名为"private"的规则（本地）
                ],
                "outboundTag": "block" // 分流策略：交给出站"block"处理（黑洞屏蔽）
            },
            // 3.2 屏蔽广告
            {
                "type": "field",
                "domain": [
                    "geosite:category-ads-all" // 分流条件：geosite文件内，名为"category-ads-all"的规则（各种广告域名）
                ],
                "outboundTag": "block" // 分流策略：交给出站"block"处理（黑洞屏蔽）
            }
        ]
    }
}
EOF
```



## Xray国内中转配置文件

```json
cat > /xray/config.json <<EOF
{
    "log": {
        "access": "/var/log/xray/access.log",
        "error": "/var/log/xray/error.log",
        "loglevel": "debug",
        "dnsLog": true
    },
    "inbounds": [
        {
            "port": 80,
            "protocol": "vless",
            "settings": {
                "clients": [
                    {
                        "id": "36e7e680-7e6c-4270-bcfc-d954dcb9d0e0", // 填写你的 UUID
                        "level": 0,
                        "email": "http@yanhao.ren"
                    }
                ],
                "decryption": "none",
                "fallbacks": [
                    {
                        "dest": "3000"
                    },
                    {
                        "path": "/chinaunicom", // 必须换成自定义的 PATH
                        "dest": 1234,
                        "xver": 1
                    }
                ]
            },
            "streamSettings": {
                "network": "tcp",
                "security": "none"
            }
        },
        {
            "port": 1234,
            "listen": "127.0.0.1",
            "protocol": "vless",
            "settings": {
                "clients": [
                    {
                        "id": "36e7e680-7e6c-4270-bcfc-d954dcb9d0e0", // 填写你的 UUID
                        "level": 0,
                        "email": "ws@yanhao.ren"
                    }
                ],
                "decryption": "none"
            },
            "streamSettings": {
                "network": "ws",
                "security": "none",
                "wsSettings": {
                    "acceptProxyProtocol": true, // 提醒：若你用 Nginx/Caddy 等反代 WS，需要删掉这行
                    "path": "/chinaunicom" // 必须换成自定义的 PATH，需要和上面的一致
                }
            }
        }
    ],
    "outbounds": [
        // 腾讯云-香港-ws
        {
            "tag": "腾讯云-香港-ws",
            "protocol": "vless",
            "settings": {
                "vnext": [
                    {
                        "address": "43.132.176.104", // 换成你的域名或服务器 IP（发起请求时无需解析域名了）
                        "port": 80,
                        "users": [
                            {
                                "id": "36e7e680-7e6c-4270-bcfc-d954dcb9d0e0", // 填写你的 UUID
                                "encryption": "none"
                            }
                        ]
                    }
                ]
            },
            "streamSettings": {
                "network": "ws",
                "wsSettings": {
                    "path": "/chinaunicom" // 必须换成自定义的 PATH，需要和服务端的一致
                }
            }
        },
        // 5.1 第一个出站是默认规则，freedom就是对外直连（vps已经是外网，所以直连）
        {
            "tag": "成都直连",
            "protocol": "freedom"
        },
        // 5.2 屏蔽规则，blackhole协议就是把流量导入到黑洞里（屏蔽）
        {
            "tag": "block",
            "protocol": "blackhole"
        }
    ],
    "routing": {
        // https://xtls.github.io/config/base/routing/
        "domainStrategy": "IPIfNonMatch",
        "rules": [
            // 3.1 防止服务器本地流转问题：如内网被攻击或滥用、错误的本地回环等
            {
                "type": "field",
                "ip": [
                    "geoip:private" // 分流条件：geoip文件内，名为"private"的规则（本地）
                ],
                "outboundTag": "block" // 分流策略：交给出站"block"处理（黑洞屏蔽）
            },
            {
                "type": "field",
                "outboundTag": "腾讯云-香港-ws",
                "domain": [
                    "geosite:geolocation-!cn"
                ]
            },
            // 3.2 屏蔽广告
            {
                "type": "field",
                "domain": [
                    "geosite:category-ads-all" // 分流条件：geosite文件内，名为"category-ads-all"的规则（各种广告域名）
                ],
                "outboundTag": "block" // 分流策略：交给出站"block"处理（黑洞屏蔽）
            },
            // 配置国内网站和IP直连规则
            {
                "type": "field",
                "domain": [
                    "geosite:apple-cn",
                    "geosite:google-cn",
                    "geosite:tld-cn",
                    "geosite:category-games@cn"
                ],
                "outboundTag": "成都直连"
            },
            {
                "type": "field",
                "ip": [
                    "geoip:cn"
                ],
                "outboundTag": "成都直连"
            },
            {
                "type": "field",
                "outboundTag": "成都直连",
                "domain": [
                    "geosite:cn"
                ]
            }
        ]
    }
}
EOF
```

## 创建 Adguard Home 配置文件

```yaml
cat > /xray/adguardhome/conf/AdGuardHome.yaml <<EOF
bind_host: 0.0.0.0
bind_port: 3000
beta_bind_port: 0
users:
- name: root
  password: $2a$10$2t8aA97XaEkKJ5.LOvxTn.wczPczi.m2wUcAyo4NZ9Rl8BEhUwWWe
auth_attempts: 5
block_auth_min: 15
http_proxy: ""
language: ""
rlimit_nofile: 0
debug_pprof: false
web_session_ttl: 720
dns:
  bind_hosts:
  - 0.0.0.0
  port: 53
  statistics_interval: 7
  querylog_enabled: true
  querylog_file_enabled: true
  querylog_interval: 7
  querylog_size_memory: 1000
  anonymize_client_ip: false
  protection_enabled: true
  blocking_mode: default
  blocking_ipv4: ""
  blocking_ipv6: ""
  blocked_response_ttl: 10
  parental_block_host: family-block.dns.adguard.com
  safebrowsing_block_host: standard-block.dns.adguard.com
  ratelimit: 0
  ratelimit_whitelist: []
  refuse_any: true
  upstream_dns:
  - https://dns10.quad9.net/dns-query
  upstream_dns_file: ""
  bootstrap_dns:
  - 9.9.9.10
  - 149.112.112.10
  - 2620:fe::10
  - 2620:fe::fe:10
  all_servers: false
  fastest_addr: false
  allowed_clients: []
  disallowed_clients: []
  blocked_hosts:
  - version.bind
  - id.server
  - hostname.bind
  cache_size: 4194304
  cache_ttl_min: 0
  cache_ttl_max: 0
  bogus_nxdomain: []
  aaaa_disabled: false
  enable_dnssec: false
  edns_client_subnet: false
  max_goroutines: 300
  ipset: []
  filtering_enabled: true
  filters_update_interval: 24
  parental_enabled: false
  safesearch_enabled: false
  safebrowsing_enabled: false
  safebrowsing_cache_size: 1048576
  safesearch_cache_size: 1048576
  parental_cache_size: 1048576
  cache_time: 30
  rewrites: []
  blocked_services: []
  local_domain_name: lan
  resolve_clients: true
  local_ptr_upstreams: []
tls:
  enabled: false
  server_name: ""
  force_https: false
  port_https: 443
  port_dns_over_tls: 853
  port_dns_over_quic: 784
  port_dnscrypt: 0
  dnscrypt_config_file: ""
  allow_unencrypted_doh: false
  strict_sni_check: false
  certificate_chain: ""
  private_key: ""
  certificate_path: ""
  private_key_path: ""
filters:
- enabled: true
  url: https://adguardteam.github.io/AdGuardSDNSFilter/Filters/filter.txt
  name: AdGuard DNS filter
  id: 1
- enabled: false
  url: https://adaway.org/hosts.txt
  name: AdAway Default Blocklist
  id: 2
- enabled: false
  url: https://www.malwaredomainlist.com/hostslist/hosts.txt
  name: MalwareDomainList.com Hosts List
  id: 4
- enabled: true
  url: https://anti-ad.net/easylist.txt
  name: anti-AD
  id: 1624184772
- enabled: true
  url: https://gitee.com/halflife/list/raw/master/ad.txt
  name: HalfLife
  id: 1624184773
whitelist_filters: []
user_rules:
- ""
dhcp:
  enabled: false
  interface_name: ""
  dhcpv4:
    gateway_ip: ""
    subnet_mask: ""
    range_start: ""
    range_end: ""
    lease_duration: 86400
    icmp_timeout_msec: 1000
    options: []
  dhcpv6:
    range_start: ""
    lease_duration: 86400
    ra_slaac_only: false
    ra_allow_slaac: false
clients: []
log_compress: false
log_localtime: false
log_max_backups: 0
log_max_size: 100
log_max_age: 3
log_file: ""
verbose: false
schema_version: 10
EOF
```

## 创建网卡

```shell
docker network create --subnet=172.18.0.0/16 adguardhome
```

## docker-compose.yml

```yaml
cat > /xray/docker-compose.yml <<EOF
version: "3"
services:
  # Xray
  xray:
    image: teddysun/xray
    container_name: xray
    # Docker 容器的网络模式需要是 Host
    # https://github.com/XTLS/Xray-core/releases/tag/v1.3.0
    network_mode: "host"
    dns:
      - 172.18.0.31
    volumes:
      - ./config.json:/etc/xray/config.json
      # - ./geoip.dat:/usr/share/xray/geoip.dat
      # - ./geosite.dat:/usr/share/xray/geosite.dat
      - ./geoip.dat:/usr/local/share/xray/geoip.dat
      - ./geosite.dat:/usr/local/share/xray/geosite.dat
      # installed: /usr/local/share/xray/geoip.dat
      # installed: /usr/local/share/xray/geosite.dat
      - ./log:/var/log/xray
    restart: always
    depends_on:
      - adguardhome
  # AdGuard Home
  adguardhome:
    image: adguard/adguardhome
    container_name: adguardhome
    ports:
      - 3000:3000/tcp
    volumes:
      - ./adguardhome/work:/opt/adguardhome/work
      - ./adguardhome/conf:/opt/adguardhome/conf
    restart: always
    networks:
      adguardhome:
        ipv4_address: 172.18.0.31
networks:
  adguardhome:
    external: true
# docker network create --subnet=172.18.0.0/16 adguardhome

# https://docs.docker.com/compose/compose-file/compose-file-v3/#network-configuration-reference
EOF
```

## Bcrypt密码在线生成计算器

http://www.ab126.com/goju/10822.html



## 下载 [rules-dat](https://github.com/Loyalsoldier/v2ray-rules-dat)

```shell
sudo curl -L "https://github.com/Loyalsoldier/v2ray-rules-dat/releases/latest/download/geoip.dat" -o /xray/geoip.dat
```

```shell
sudo curl -L "https://github.com/Loyalsoldier/v2ray-rules-dat/releases/latest/download/geosite.dat" -o /xray/geosite.dat
```

```
wget -O /usr/share/xray/geosite.dat https://github.com/v2fly/domain-list-community/releases/latest/download/dlc.dat
```



## RUN

```shell
cd /xray/ && docker-compose up -d
```

### 编辑 Adguard Home 密码

```
/xray/adguardhome/conf/AdGuardHome.yaml
```

```
docker restart adguardhome
```

