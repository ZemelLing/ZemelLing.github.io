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
## 图形相关概念
* 渲染管线: 一个渲染管线通过执行一系列操作获取场景中需要展示的内容并将其显示到场景中。这些操作包括：
    *  剔除, 列出所有需要渲染的对象
        1. 所有需要显示到摄像头范围内的对象（视锥剔除）
        2. 所有未被其他对象挡住的对象（遮挡剔除）
    * 渲染, 将具有正确光照和其他设置的对象写入基于像素的缓冲区
* 后期处理: 在像素缓冲区中执行操作（应用色阶，bloom，景深）以生成发送到设备的最终帧
* Shader: 在GPU中运行的程序
* 全局光照 (GI) 是对直接和间接光照进行建模以提供逼真光照效果的一组技术
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
通用渲染管线 (URP) 是一种快速的单通道前向渲染器；
### 高清渲染管线
高清渲染管线是 Unity 借助 HDRP 预先构建的可编程渲染管线，能够创建高保真画面。高清渲染管线 (HDRP) 是一种混合延迟/前向瓦片/聚类渲染器
### 自定渲染管线
## 全局光照系统
### 实时全局光照：这个系统完全依赖于第三方光照中间件 Enlighten（废弃）
### 烘焙全局光照：光照被烘焙成称为光照贴图的纹理以及光照探针。
## 光照模式（Light组件）
Light 组件的Mode提供了三个选项
* 烘培
* 实时
* 混合
Light 组件的Mode只有在烘培GI系统中才意义。如果不使用任何 GI 系统或只使用实时 GI 系统，那么所有烘焙光源和混合光源的行为就好像它们的 Mode 属性设置为 Realtime 一样。
## 光照模式（Lighting设置）
三种：
1. Subtractive 
2. Baked Indirect 
3. Shadowmask，光照模式有两个质量设置：1.Shadowmask 2.Distance Shadowmask