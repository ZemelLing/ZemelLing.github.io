---
title: "Unity的Camera组件"
date: 2021-03-27T14:04:16+08:00
draft: false
tags: ["unity", "camera"]
categories: ["unity",]
---

## 透视模式和正交模式

Unity的相机存在两种相机投影模式，其中透视模式实现了现实世界的效果，即近大远小的透视效果，反之，无近大远小效果的为正交模式。

## 可视范围的形状

![Unity相机的透视与正交](/images/Unity相机的透视和正交20210327.png)

## 相机(Camera)组件

![Unity相机组件](https://docs.unity.cn/2021.1/Documentation/uploads/Main/InspectorCamera35.png)  
上图显示的是在内建渲染管线下相机的属性截图

### 属性

#### Clear Flags

#### Culling Mask

#### FOV Axis & Field of view

#### Physical Camera

#### Viewport Rect

#### Rendering Path

#### Target Texture

### 其他相关概念

#### HDR

#### MSAA

#### Dynamic Resolution