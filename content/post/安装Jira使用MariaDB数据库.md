---
title: "安装Jira使用MariaDB数据库"
date: 2021-07-02T12:40:56+08:00
draft: false
tags: ["jira", "MariaDB"]
categories: ["其他方面",]
series:
description:
toc: true
authors:
  - zemelling
lastmod: 2021-08-29T13:09:56+08:00
featuredImage:
---

# Jira 安装

## 安装环境及软件

* Windows Server 2016
* MariaDB 10.3.14
* Jira 8.1.0
* mysql-connector-java-5.1.47-bin.jar

## 步骤

### 安装 MariaDB

1. 将 MariaDB 的 zip 安装包解压到指定目录，例如 C:\\Program Files\\MariaDB
2. 新建数据库存储文件夹，例如 C:\\Program Files\\MariaDB\\DB
3. 以管理员身份运行 CMD，运行如下命令：

    mysql_install_db.exe --datadir=C:\Program Files\MariaDB\DB --service=MariaDB --password=mima
    > --service 指定 MariaDB 的服务名
    > --password 指定 root 密码
4. 启动 MariaDB 服务

    sc start MariaDB 
5. 进入到 MariaDB

    mysql -u root -p
6. 为 Jira 创建专用数据库用户

    CREATE USER 'jira'@'%' IDENTIFIED BY 'some_password';
    GRANT ALL PRIVILEGES ON *.* TO 'jira'@'%' WITH GRANT OPTION;
    FLUSH PRIVILEGES;
    > % 表示从任何地方都可连接数据库，若为 localhost 只可从 MaraiDB 所在服务器连接数据库
7. 创建 Jira 使用的数据库

    CREATE DATABASE jiradb CHARACTER SET utf8 COLLATE utf8_bin;
    > 此处使用普通的 utf8 编码就行，不要用 utf8mb4 。
    > utf8mb4 会导致后续配置完成的 Jira 无法识别排序规则

### 安装 Jira

1. 以管理员身份运行 Jira 安装包，按需配置即可
2. 下载  [MySQL Connector/J 5.1 driver][1] 并解压。
3. 将解压后的文件夹中的 mysql-connector-java-5.1.47-bin.jar 文件复制到 jira 安装路径的 lib 文件夹下
4. 重启 Jira 服务
5. 通过 IP:PORT 访问 Jira 的配置界面，按需配置，但注意 *** 数据库选择 Mysql 5.6 ***

### 注意事项

1. 远程访问时，要开放数据库和 Jira 相应的端口，Windows 开放端口参考以下连接：
    http://www.xitongcheng.com/jiaocheng/win10_article_12908.html





  [1]: https://dev.mysql.com/downloads/connector/j/5.1.html