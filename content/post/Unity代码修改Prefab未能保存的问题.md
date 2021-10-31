---
title: "Unity代码修改Prefab未能保存的问题"
date: 2021-09-06T15:37:19+08:00
draft: false
tags: ["unity", "android"]
categories: ["游戏开发",]
series:
description:
toc: true
authors:
  - zemelling
lastmod: 2021-09-06T15:37:19+08:00
featuredImage:
---

## 解决办法

使用 `EditorUtility.SetDirty(thePrefabObjectOrComponent);` 将其设置为Dirty，之后就可以保存了。