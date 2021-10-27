---
title: "V2ray的安装与配置"
date: 2021-07-02T12:54:57+08:00
draft: true
series:
description:
toc: true
authors:
  - zemelling
lastmod: 2021-08-29T13:09:56+08:00
featuredImage:
---

# V2ray的安装与配置

## 安装V2ray

### Linux

docker pull v2ray/official

  使用官方一键安装脚本

  ```
  bash <(curl -L https://raw.githubusercontent.com/v2fly/fhs-install-v2ray/master/install-release.sh)
  ```

  此脚本会安装如下文件:

  - /usr/bin/v2ray/v2ray：V2Ray 程序；
  - /usr/bin/v2ray/v2ctl：V2Ray 工具；
  - /etc/v2ray/config.json：配置文件；
  - /usr/bin/v2ray/geoip.dat：IP 数据文件
  - /usr/bin/v2ray/geosite.dat：域名数据文件

## 启动
systemctl start v2ray

## 停止
systemctl stop v2ray

## 重启
systemctl restart v2ray

## 开机自启
systemctl enable v2ray

### Windows

  从官方GitHub仓库中下载压缩包：

  ```
  https://github.com/v2ray/v2ray-core/releases
  ```

  解压后运行v2ray.exe。

## 配置V2ray

  Linux环境下的配置文件位于 `/etc/v2ray/config.json`  
  Windows环境下的配置文件与v2ray.exe 在同一目录下。

### Vmess协议服务器与客户端配置

#### 服务端

```json
{
  "inbounds": [
    {
      "port": 16823,
      "protocol": "vmess",
      "settings": {
        "clients": [
          {
            "id": "GUID",
            "alterId": 64
          }
        ]
      }
    }
  ],
  "outbounds": [
    {
      "protocol": "freedom",
      "settings": {}
    }
  ]
}
```

#### 客户端

```json
{
  "inbounds": [
    {
      "port": 1080,
      "protocol": "socks",
      "domainOverride": ["tls","http"],
      "settings": {
        "auth": "noauth"
      }
    }
  ],
  "outbounds": [
    {
      "protocol": "vmess",
      "settings": {
        "vnext": [
          {
            "address": "104.168.163.52",
            "port": 16823,
            "users": [
              {
                "id": "GUID",
                "alterId": 64
              }
            ]
          }
        ]
      }
    }
  ]
}
```

### Shadowsocks协议服务器配置

```json
{
  "inbounds": [{
    "port": 15788,
    "listen": "0.0.0.0",
    "protocol": "shadowsocks",
    "settings": {
      "email": "admin@zling.info",
      "method": "chacha20-ietf-poly1305",
      "password": "mima",
      "level": 0,
      "network": "tcp"
    }
  }],
  "outbounds": [
    {
      "protocol": "freedom",
      "settings": {}
    }
  ]
}

mkdir -p /etc/v2ray

nano /etc/v2ray/config.json

docker run -d --restart=always --name v2ray -v /etc/v2ray:/etc/v2ray -p 15788:15788 v2ray/official  v2ray -config=/etc/v2ray/config.json

```

## 其他

### 一键开启BBR

  使用[秋水逸冰](https://teddysun.com/489.html)的一键脚本开启BBR

  ```
  wget --no-check-certificate https://github.com/teddysun/across/raw/master/bbr.sh && chmod +x bbr.sh && ./bbr.sh
  ```


### 新版

mkdir -p /etc/v2ray

nano /etc/v2ray/config.json

docker network create v2raynet

```json
{
  "inbounds": [
    {
      "port": 9706,
      "protocol": "vmess",
      "settings": {
        "clients": [
          {
            "id": "guid",
            "alterId": 64
          }
        ]
      },
      "streamSettings": {
        "network": "ws",
        "wsSettings": {
          "path": "/ray"
        }
      }
    }
  ],
  "outbounds": [
    {
      "protocol": "freedom",
      "settings": {}
    }
  ]
}
```

docker run -d --name v2ray --restart always \
-v /etc/v2ray:/etc/v2ray --net v2raynet \
v2ray/official v2ray -config=/etc/v2ray/config.json

mkdir -p ~/caddy/data
mkdir -p ~/caddy/config
mkdir -p ~/caddy/site

nano ~/caddy/Caddyfile

```json
v2.zling.biz
{
  log ./caddy.log
  reverse_proxy /ray v2rays:9706 {
    header_up -Origin
  }
}
```

docker run -d --name caddy --restart always --net v2raynet -p 80:80 -p 443:443 \
-v ~/caddy/site:/usr/share/caddy -v ~/caddy/Caddyfile:/etc/caddy/Caddyfile \
-v ~/caddy/data:/data -v  ~/caddy/config:/config caddy:2.0.0

docker run -d --rm --name v2ray -p 443:443 -p 80:80 \
-v $HOME/.caddy:/root/.caddy  pengchujin/v2ray_ws:0.11 \
v2.zling.biz V2RAY_WS ab616110-167a-4edc-9c7e-464177a817c1 && \
sleep 3s && \
docker logs v2ray
