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
1. 切换至站点的themes目录并将hugo-vitae添加为当前工程的子模块
```shell
cd themes
git submodule add https://github.com/dataCobra/hugo-vitae
```
2. 将hugo-vitae的exampleSite config.toml 复制到根目录稍作改动

## 创建文章
```shell
hugo new post/first.md
```
## 运行Hugo Server
```shell
hugo server
```
## 创建GitHub仓库
创建GitHub仓库时需要注意仓库名称必须为：yourgithubusername.github.io

## 使用GitHub Actions自动部署

1. 创建部署用的Token
2. 在刚刚创建的仓库的设置中添加 Secrets
3. 在仓库下创建文件 .github/workflows/gh-pages.yml
```yaml
name: github pages

on:
  push:
    branches:
      - main  # Set a branch to deploy

jobs:
  deploy:
    runs-on: ubuntu-18.04
    steps:
      - uses: actions/checkout@v2
        with:
          submodules: true  # Fetch Hugo themes (true OR recursive)
          fetch-depth: 0    # Fetch all history for .GitInfo and .Lastmod

      - name: Setup Hugo
        uses: peaceiris/actions-hugo@v2
        with:
          hugo-version: 'latest'
          extended: true

      - name: Build
        run: hugo --minify

      - name: Deploy
        uses: peaceiris/actions-gh-pages@v3
        with:
          github_token: ${{ secrets.HUGO_TOKEN }}
          publish_branch: gh-pages  # 推送到当前 gh-pages 分支
          publish_dir: ./public # 推送生成的public文件下的内容到 gh-pages 分支
          commit_message: ${{ github.event.head_commit.message }}
```