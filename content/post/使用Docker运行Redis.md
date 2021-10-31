---
title: "使用Docker运行Redis"
date: 2021-04-23T14:43:14+08:00
draft: false
tags: ["docker", "redis"]
categories: ["其他方面",]
series:
description:
toc: true
authors:
  - zemelling
lastmod: 2021-08-29T13:09:56+08:00
featuredImage:
---

## Use Command

```shell
docker run -d --restart=always --name redis -p 6379:6379 redis --requirepass 123456
```