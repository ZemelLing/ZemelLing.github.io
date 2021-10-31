---
title: "使用acmesh申请LetsEncrypt泛域名证书"
date: 2021-07-02T12:43:35+08:00
draft: false
tags: ["web", "certificate"]
categories: ["其他方面",]
series:
description:
toc: true
authors:
  - zemelling
lastmod: 2021-08-29T13:09:56+08:00
featuredImage:
---

# 使用 acme.sh 申请 Let's Encrypt 泛域名证书

以下申请证书的过程以 NameSilo ，这个域名服务商为例，其它域名服务商的申请细节请结合以下链接参考：

    https://github.com/Neilpang/acme.sh/wiki/dnsapi

## 安装 acme.sh

    curl  https://get.acme.sh | sh

## 获取域名服务商的api key

NameSilo
    https://www.namesilo.com/account_api.php    

## 设置api key 环境变量

以下是 NameSilo 的 api key 设置环境变量的方法，其他域名服务商请参考上方的链接

    export Namesilo_Key="xxxxxxxxxxxxxxxx"

## 申请证书

    acme.sh --issue --dns dns_namesilo --dnssleep 900 -d yourdomain -d *.yourdomain

此处要等待 900 秒
申请成功，证书将放在 `/root/.acme.sh/yourdomain/` 目录下

## 安装证书

    acme.sh --installcert -d yourdomain -d *.yourdomain \
            --key-file /etc/nginx/ssl/yourdomain.key \
            --fullchain-file /etc/nginx/ssl/fullchain.cer \
            --reloadcmd "service nginx force-reload" 
