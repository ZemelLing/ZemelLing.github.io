---
title: "使用Docker运行Consul"
date: 2021-04-23T16:16:07+08:00
draft: false
tags: ["docker", "consul"]
categories: ["其他方面",]
series:
description:
toc: true
authors:
  - zemelling
lastmod: 2021-08-29T13:09:56+08:00
featuredImage:
---

## 单节点

开发模式

```
mkdir -p /data/consul-data
mkdir -p /data/consul-conf

docker run -d -p 8500:8500 -p 8600:8600/udp -v /data/consul-data:/consul/data -v /data/consul-conf:/consul/config --name=consul_server consul agent -dev -ui -node=consul-server -bootstrap-expect=1 -client=0.0.0.0 -data-dir /consul/data -config-dir /consul/config
```
