---
title: "Docker常用命令"
date: 2021-04-30T17:40:59+08:00
draft: false
tags: ["docker",]
categories: ["docker",]
---

## 进入容器

    docker exec -it [容器ID] /bin/bash  

## 创建卷
    docker volume create my-vol
## 列出所有卷
    docker volume ls
## 查看卷详情
    docker volume inspect my-vol
## 移除卷
    docker volume rm my-vol

## 修改 Docker 的默认存储路径

```
sudo docker info
sudo nano /etc/docker/daemon.json

{
  "data-root": "/www/docker"
}
```