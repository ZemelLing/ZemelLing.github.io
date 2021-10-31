---
title: "使用Docker部署Trilium"
date: 2021-03-25T13:26:38+08:00
draft: false
tags: ["docker",]
categories: ["其他方面",]
series:
description:
toc: true
authors:
  - zemelling
lastmod: 2021-08-29T13:09:56+08:00
featuredImage:
---

## docker-compose
```yaml
version: "3.7"

services:
  trilium:
    image: zadam/trilium
    restart: unless-stopped
    ports:
      - "8080:8080"
    volumes:
      - /data/trilium/data:/home/node/trilium-data
```

```shell
docker-compose up -d
```

## 遇到容器里的权限问题

```
internal/fs/utils.js:269
    throw err;
    ^
Error: EACCES: permission denied, mkdir '/home/node/trilium-data/log'
    at Object.mkdirSync (fs.js:921:3)
    at Object.<anonymous> (/usr/src/app/src/services/log.js:7:8)
    at Module._compile (internal/modules/cjs/loader.js:1015:30)
    at Object.Module._extensions..js (internal/modules/cjs/loader.js:1035:10)
    at Module.load (internal/modules/cjs/loader.js:879:32)
    at Function.Module._load (internal/modules/cjs/loader.js:724:14)
    at Module.require (internal/modules/cjs/loader.js:903:19)
    at require (internal/modules/cjs/helpers.js:74:18)
    at Object.<anonymous> (/usr/src/app/src/app.js:1:13)
    at Module._compile (internal/modules/cjs/loader.js:1015:30) {
  errno: -13,
  syscall: 'mkdir',
  code: 'EACCES',
  path: '/home/node/trilium-data/log'
}
```
解决办法
```
chown -R 1000:1000 /data/trilium/data/
```