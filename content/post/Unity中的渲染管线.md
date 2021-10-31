---
title: "Unity中的渲染管线"
date: 2021-03-31T16:30:07+08:00
draft: false
tags: ["unity", "渲染管线", "画质"]
categories: ["游戏开发",]
series:
description:
toc: true
authors:
  - zemelling
lastmod: 2021-08-29T13:09:56+08:00
featuredImage:
---

一个渲染管线通过执行一系列操作获取场景中需要展示的内容并将其显示到场景中。这些操作包括：

* 剔除
* 渲染
* 后期处理

## Unity提供的渲染管线

### 内置渲染管线

内置渲染管线是 Unity 的旧版渲染管线，并且不是基于可编程渲染管线实现的。
内容渲染管线可通过设置不同的渲染管线进行设置，同时可以通过命令缓存（Command Buffers）和回调（Callbacks）进行功能扩展。

#### 渲染路径

存在以下四种渲染路径：

* 向前渲染（Forward Rendering）
* 延迟着色（Deferred Shading）
* 旧版延迟（Legacy Deferred）
* 顶点光照（Legacy Vertex Lit）

### 通用渲染管线

通用渲染管线是 Unity 预先构建的可编程渲染管线。

### 高清渲染管线

高清渲染管线是 Unity 借助 HDRP 预先构建的可编程渲染管线，能够创建高保真画面。

### 自定渲染管线