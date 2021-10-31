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

```
sudo fdisk -l
```

## 使用 fdisk 分区

```
sudo fdisk /dev/sdb
```

## 格式化分区

```
sudo mkfs -t ext4 /dev/sdb1
```

## 挂载硬盘

创建挂载点，新建目录
```
mkdir ~/disk1
```

挂载
```
sudo mount /dev/sdb1 ~/disk1
```

## 自动挂载

查找硬盘 UUID
```
ls -l  /dev/disk/by-uuid/
```

修改 /etc/fstab 文件，实现自动挂载
```
UUID=b543f8f7-579c-45b5-96d6-31de6fa1a55e /home/lgd/disk1 ext4 defaults 1 2
```