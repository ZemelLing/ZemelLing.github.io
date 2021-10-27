---
title: "Unity的Camera组件"
date: 2021-03-27T14:04:16+08:00
draft: false
tags: ["unity", "camera"]
categories: ["unity",]
series:
description:
toc: true
authors:
  - zemelling
lastmod: 2021-08-29T13:09:56+08:00
featuredImage:
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

ClearFlags 用于清除相机不同的缓冲区信息集合。

##### Skybox

使用天空盒填充相机视口未存在游戏对象遮挡的区域，如果相机没有天空盒，则使用 Lighting Window 中的天空盒，如果还未设置，则退回为使用 Background 的颜色填充。

##### Solid color

相机视口未存在游戏对象遮挡的区域将填充为指定颜色

##### Depth only



##### Don’t clear

不会清除缓冲区的信息

#### Culling Mask

通过 Layer 的方式指定要渲染到相机视口的内容。

#### FOV Axis & Field of view

FOV Axis 表示相机的视场角的轴，Field of view 表示相机的视场角大小，视场角延轴改变大小。

#### Physical Camera

启用此项后，此相机将模拟真实世界里的相机，并使用真实世界相机的参数。

#### Viewport Rect

相机视口在屏幕上的显示矩形框。

#### Target Texture

将相机拍摄到的内容渲染到指定的 Texture 上，这可用于制作监视器、小地图等。

### 其他相关概念

#### HDR

高动态范围，只有内置渲染管线支持 HDR 输出。

#### MSAA

多重采样抗锯齿

#### Dynamic Resolution

动态分辨率是一种摄像机设置，允许动态缩放单个渲染目标，以便减少 GPU 上的工作负载。