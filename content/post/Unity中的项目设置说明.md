---
title: "Unity中的项目设置说明"
date: 2021-04-05T14:11:24+08:00
draft: false
tags: ["unity",]
categories: ["游戏开发",]
series:
description:
toc: true
authors:
  - zemelling
lastmod: 2021-08-29T13:09:56+08:00
featuredImage:
---

## 项目设置

Edit > Project Settings 此处打开项目设置窗口。

### Player

Player 界面用于设置最终生成的游戏包相关的设置。多数与具体平台有关。

#### Android

##### Other Settings

###### Configuration

* API Compatibility Level
    * .Net Standard 2.0 兼容 .NET Standard 2.0。生成较小的构建并具有完整的跨平台支持。
    * 兼容 .NET Framework 4（包括 .NET Standard 2.0 配置文件中的所有内容以及其他 API）。如果使用的库需要访问 .NET Standard 2.0 中未包含的 API，请选择此选项。生成更大的构建，并且任何可用的其他 API 不一定在所有平台上都受支持。