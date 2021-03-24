---
title: "使用hugo搭建静态博客"
date: 2021-03-23T22:36:20+08:00
draft: false
tags: [ "hugo",]
---

## 安装Go
[Go安装地址](https://golang.org/dl/)，下载相应版本的Go安装器，按步骤安装即可。
## 安装Hugo
去[Hugo的Github](https://github.com/gohugoio/hugo/releases)上下载，hugo_extended版的压缩包，然后解压到指定目录，并将Hugo执行文件所在目录加入到Path环境变量中。
在终端中执行以下命令判断Hugo是否安装成功：
```shell
hugo version
```
## 创建Hugo Site
```shell
hugo new site yoursitename
```
## 安装Hugo-Vitae主题
1. 切换至站点的themes目录下执行以下语句。
```shell
git clone https://github.com/dataCobra/hugo-vitae hugo-vitae
```
2. 将hugo-vitae的exampleSite

## 创建文章
## 运行Hugo Server
## 创建GitHub仓库
## 使用GitHub Actions自动部署

