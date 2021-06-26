# Xray + Adguard Home

## 创建项目目录

```shell
mkdir -p /xray && mkdir -p /xray/adguardhome/conf
```

## 创建Xray配置文件

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
            "port": 9000,
            "protocol": "vless",
            "settings": {
                "clients": [
                    {
                        "id": "36e7e680-7e6c-4270-bcfc-d954dcb9d0e0",
                        "email": "default@yanhao.ren",
                        "flow": "xtls-rprx-direct"
                    }
                ],
                "decryption": "none"
                // 现阶段需要填 "none"，不能留空。
                /* "fallbacks": [
                    {
                        "dest": 33222
                    }
                ] */
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
                                    "application/x-shockwave-flash",
                                    "video/mpeg"
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

## 创建 Adguard Home 配置文件

```yaml
cat > /xray/adguardhome/conf/AdGuardHome.yaml <<EOF
bind_host: 0.0.0.0
bind_port: 3000
beta_bind_port: 0
users:
- name: root
  password: $2a$10$jBlu7/mhpmgxMGqnE2jOs.2xw4SGZnJZeO4my7uKafHisWylIcjzi
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
docker network create --subnet=172.18.0.0/16 xray-network
```

## docker-compose.yml

```yaml
cat > /xray/docker-compose.yml <<EOF
version: "2"
services:
  # Xray
  xray:
    image: teddysun/xray
    container_name: xray
    dns:
      - 172.18.0.31
    ports:
      - 80:9000/tcp
    volumes:
      - .:/etc/xray
      - ./log:/var/log/xray
    restart: always
    depends_on:
      - adguardhome
    networks:
      - xray-network
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
      xray-network:
        ipv4_address: 172.18.0.31
networks:
  xray-network:
    external: true
EOF
```

## Bcrypt密码在线生成计算器

http://www.ab126.com/goju/10822.html

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

