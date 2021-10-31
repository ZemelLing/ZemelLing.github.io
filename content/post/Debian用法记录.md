---
title: "Debian用法记录"
date: 2021-05-01T10:17:40+08:00
draft: false
tags: ["linux", "debian"]
categories: ["其他方面",]
series:
description:
toc: true
authors:
  - zemelling
lastmod: 2021-08-29T13:09:56+08:00
featuredImage:
---

## 关机
```
1. systemctl poweroff
```

## 重启

```
1. systemctl reboot
```

## 设置临时环境变量

```
export PATH=$PATH:/home/xyz/Tesseract/bintesseract
```

## 设置永久环境变量

```
1. 对所有用户
// 编辑 /etc/profile
export PATH="$PATH:/home/xyz/Tesseract/bin"

2. 对当前用户
// 编辑 ~/.bashrc
export PATH="$PATH:/home/xyz/Tesseract/bin"

最后需要
source ~/.bashrc
```

## 查看版本

```
cat /etc/issue
```

## 挂载新硬盘

```shell

sudo fdisk -l // 查看硬盘设备及分区

sudo fdisk /dev/sda // 操作硬盘

sudo mkfs -t ext4 -c /dev/sda1 // 格式化分区 -c 用于检查坏道，比较耗时。

// 挂载分区
sudo mkdir /mnt/data1
sudo mount /dev/sda1 /mnt/data1

// 开机自动挂载
sudo echo "/dev/sda1 /mnt/data1 ext4 defaults 0 0">>/etc/fstab

```