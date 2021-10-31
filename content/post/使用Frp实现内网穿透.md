---
title: "使用Frp实现内网穿透"
date: 2021-07-02T12:46:26+08:00
draft: false
tags: ["frp", "内网穿透"]
categories: ["其他方面",]
series:
description:
toc: true
authors:
  - zemelling
lastmod: 2021-08-29T13:09:56+08:00
featuredImage:
---

# 使用 Frp 实现内网穿透

## Frp 程序下载

    https://github.com/fatedier/frp/releases

frp 程序分为服务端和客户端，其中 frps 开头文件为服务端， frpc 开头文件为客户端

## 配置服务端

编辑 frps.ini 配置服务端
```
[common]
bind_port = 7400  #客户端与服务端通信端口，要保持一致
vhost_http_port = 8080 #服务端监听的 http 流量端口
vhost_https_port = 8443 #服务端监听的 https 流量端口

token = 123456 #服务端与客户端验证端口，要保持一致，推荐使用 UUID

subdomain_host = yourdomain.com #用于自定义域名访问，在客户端配置 subdomain，可实现子域名访问内网web服务
```

> 记得要将域名解析到服务端的公网 ip 上

启动 frps

    ./frps -c ./frps.ini

## 配置客户端

编辑 frpc.ini 配置客户端

```
[common]
server_addr = xxx.xxx.xxx.xxx #服务端的公网ip
server_port = 7400 #客户端与服务端通信端口，要保持一致

token = 123456 #服务端与客户端验证端口，要保持一致，推荐使用 UUID
user = zemel #客户端用户名用于标识客户端
login_fail_exit = true #token 不匹配就断开连接

protocol = tcp #指定连接协议

[http]
type = http 
local_port = 80 #客户端本地 web 服务端口
subdomain = a #子域名
```

启动 frpc

    ./frpc -c ./frpc.ini

**之后就可以通过`a.yourdomain.com`访问内网的 web 服务**

## 将 frp 设置为开机自启动

### Linux

```
nano /etc/systemd/system/frps.service
```

在其中填写
```
[Unit]
Description=frps daemon

[Service]
Type=simple
ExecStart=/usr/bin/frps -c /etc/frps/frps.ini

[Install]
WantedBy=multi-user.target
```

执行生效命令

```
systemctl start frps
systemctl enable frps
```

之后就能够通过`systemctl start frps` `systemctl restart frps` `systemctl stop frps` 来控制 frps
frpc 同理

### Windows

使用 winsw 将 frpc 设置成服务
    https://github.com/kohsuke/winsw

* 将下载的 WinSW.NET2.exe 或 WinSW.NET4.exe 移动到与 frpc 同一文件夹下，并改名为 winsw.exe
* 新建名为 winsw.xml 的 xml 文件，并填写如下内容：
```
<service>
    <id>Frpc</id>
    <name>Frpc</name>
    <description>frp client</description>
    <executable>frpc</executable>
    <arguments>-c frpc.ini</arguments>
    <logmode>reset</logmode>
</service>
```
* 将 frpc 所在文件夹写入系统变量 Path 中
* 运行 cmd 切换至 frpc 所在文件夹，执行`winsw.exe install`
* 在**运行**中输入`services.msc`打开服务，找到 Frpc 服务并启动。

frps 同理

