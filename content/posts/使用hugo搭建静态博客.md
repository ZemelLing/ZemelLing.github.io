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
# 开始部署到VPS上的工作流
      - name: Cache Private Key
        uses: webfactory/ssh-agent@v0.4.1
        with:
          ssh-private-key: ${{ secrets.BLOG_DEPLOY_KEY }}
      - name: Scan public keys
        run: ssh-keyscan zling.site >> ~/.ssh/known_hosts
      - name: Deploy
        run: rsync -av --delete ./public root@zling.site:/data/caddy/site/hugo-web
```

## 自动部署至腾讯云

前提条件，在CVM上已安装好Caddy

基本思路是：通过rsync将生成的静态内容复制到VPS上

1. 生成无密码的密钥
```
ssh-keygen -t ed25519 -f ~/.ssh/blog_deploy_key
```
2. 将生成的公钥内容添加到VPS上
```
~/.ssh/authorized_keys
```
3. 将生成的私钥内容添加到项目的Secrets中，命名为 blog_deploy_key

4. 工作流文件中使用 webfactory/ssh-agent 实现私钥的缓存
```
- name: Cache Private Key
  uses: webfactory/ssh-agent@v0.4.1
  with:
    ssh-private-key: |
      ${{ secrets.BLOG_DEPLOY_KEY }}
```

5. 工作流文件中还使用 ssh-keyscan 命令扫描 VPS 的公钥并保存到虚拟环境的 ~/.ssh/known_hosts 中
```
- name: Scan public keys
  run: |
    ssh-keyscan your_vps_ip_or_domain >> ~/.ssh/known_hosts
```

6. 部署到VPS
```
- name: Deploy
  run: |
    rsync -av --delete public ./public root@your_vps_ip_or_domain:/data/caddy/site/hugo-web
```

7. Caddyfile配置

```
blog.zling.site {
    root * /srv/hugo-web
    file_server
}
```
