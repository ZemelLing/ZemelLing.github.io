---
title: "Unity中的RnderingPath"
date: 2021-03-27T19:06:07+08:00
draft: false
tags: ["unity", "render"]
categories: ["unity",]
---

## 什么是渲染路径

Unity的内置渲染管线支持不同的渲染路径，渲染路径表示了与光照和阴影相关的一系列操作。

## 何处设置渲染路径

1. 在 Graphics 窗口
2. Camera 组件

## 渲染路径的类型

当前共存在四种渲染路径：Forward Rendering、Deferred Shading、Legacy Deferred、Legacy Vertex Lit，其中后两个已过时。其中 Legacy Deferred 与 Deferred Shading 类似，但不支持基于物理的标准着色器。Legacy Vertex Lit 只是 Forward Rendering 的子集。

### Forward Rendering

正向渲染是 Unity 内置渲染管线的默认渲染路径，是一种常规渲染路径。它对于实时光照的渲染将带来昂贵的开销，为此可以通过设置 Unity 为每个像素一次可渲染多少灯光来抵消此影响，而其余灯光将用低保真的方式渲染。

### Deferred Shading

延迟阴影是 Unity 的内置渲染管线中具有最大光照和阴影保真度的渲染路径。其需要 GPU 的支持并且存在一些限制。
* 不支持半透明对象( Forward Rendering 支持)
* 不支持正交投影( Forward Rendering 支持)
* 硬件级别的抗锯齿(可用后期处理实现类似的效果)