---
title: "Ubuntu使用记录"
date: 2021-07-12T12:55:26+08:00
draft: false
tags: ["ubuntu", "linux"]
categories: ["其他方面", ]
series:
description:
toc: true
authors:
  - zemelling
lastmod: 2021-08-29T13:09:56+08:00
featuredImage:
---

## 查看硬盘状况

```sh
sudo fdisk -l
```

## 使用 fdisk 分区

```sh
sudo fdisk /dev/sdb
```

## 格式化分区

```sh
sudo mkfs -t ext4 /dev/sdb1
```

## 挂载硬盘

创建挂载点，新建目录
```sh
mkdir ~/disk1
```

挂载
```sh
sudo mount /dev/sdb1 ~/disk1
```

## 自动挂载

查找硬盘 UUID
```sh
ls -l  /dev/disk/by-uuid/
```

修改 /etc/fstab 文件，实现自动挂载
```
UUID=b543f8f7-579c-45b5-96d6-31de6fa1a55e /home/lgd/disk1 ext4 defaults 1 2
```

## 设置静态IP

修改 /etc/netplan/ 下的yaml配置文件
```yaml
network:
  version: 2
  renderer: NetworkManager
  ethernets:
    enp0s3:
      dhcp4: no
      addresses: [192.168.2.205/24]
      routes:
        - to: default
          via: 192.168.2.1
      nameservers:
        addresses: [8.8.8.8, 114.114.114.114]
```

应用配置修改
```sh
sudo netplan apply
```