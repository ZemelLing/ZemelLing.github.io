---
title: "UGUI之Canvas"
date: 2021-09-08T11:33:00+08:00
draft: false
tags: ["unity", "ugui"]
categories: ["unity", "ugui"]
series:
description:
toc: true
authors:
  - zemelling
lastmod: 2021-09-08T11:33:00+08:00
featuredImage:
---

# Canvas

Canvas 是UGUI中用于显示其他UI元素的基础，其附加了 Canvas 组件，在场景中可以有多个 Canvas 游戏对象，同时允许 Canvas 嵌套。

## Canvas 组件的重要属性

### 渲染模式

1. Screen Space - Overlay

此模式下UI内容紧贴屏幕。

2. Screen Space - Camera

此模式下 UI 内容面向指定的摄像机一定距离显示。

2. World Space

此模式下 UI 内容与场景中的其他游戏物体一样。

## Canvas Scale 组件

控制画布内所有 UI 元素的比例和像素密度。