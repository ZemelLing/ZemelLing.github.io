---
title: "MariaDB中文数据问号解决办法"
date: 2021-07-02T12:51:41+08:00
draft: false
tags: ["mariadb",]
categories: ["mariadb",]
series:
description:
toc: true
authors:
  - zemelling
lastmod: 2021-08-29T13:09:56+08:00
featuredImage:
---

在`/etc/mysql/conf.d`文件夹下修改如下文件的内容，若文件不存在的话就新建：

1. server.cnf
  ```
[mysqld]
character-set-server=utf8 
collation-server=utf8_general_ci
```
2. client.cnf
  ```
[client]
default-character-set=utf8
```

最后重启MariaDB服务