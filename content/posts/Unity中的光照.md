---
title: "Unity中的光照"
date: 2021-04-02T16:11:55+08:00
draft: false
tags: ["unity", "lighting"]
categories: ["unity",]
---

## Unity中的光照类型

### 直接光照和间接光照

直接光照指照射到物体表面只反射一次后就传入感应器的光照，而间接光照指光线经过多次反射后再传入感应器的关照，这两种光照 Unity 都可以通过计算获取。

### 实时光照和烘培光照

实时光照指 Unity 在运行时计算光照，而烘培光照指将预计算的光照信息存储后有 Unity 在运行时读取恢复的技术。这两种光照技术可以分别单独使用或混合使用。

### 全局光照

Unity 中包含两类全局光照系统，烘培全局光照和实时全局光照，其中实时全局光照系统即将弃用，而烘培全局光照系统包含光照贴图、光照探针和反射探针等，并被所有渲染管线支持。

## Lighting 窗口

### The Scene Tab

![Lighting-scene](./images/unity-lighting-scene-20210402.png)

该界面展示当前打开的场景的光照设置信息。光照设置信息来自 Lighting Settings 指定的 Lighting Settings Asset ，如果未指定则使用默认的光照设置资源（只读，不可修改）。

#### Lighting Settings Asset

Lighting Settings Asset 标识 LightingSettings  类的实例，该实例保存了烘培全局光照系统和实时全局光照系统的信息。当为使用了烘培或实时全局光照系统的场景预计算光照信息时，将使用此实例的数据。

##### Realtime Lighting

实时全局光照系统已废弃。

##### 混合光照（Mixed Lighting）

此部分包含影响使用此光照设置资源的场景中混合光照和烘培光照的效果的内容。

* 全局烘培光照，当启用时，Unity 只对光照贴图做烘培，并且混合光照的行为由 Lighting Mode 决定。而不启用时，Unity 将把所有 Backed 和混合光照当作实时光照处理。

* 光照模式（Lighting Mode）
    * 间接烘培（Backed Indirect）  
    该模式将实时直接光照与烘焙间接光照结合在一起，提供实时阴影。这种光照模式提供逼真的光照和合理的阴影保真度，适用于中档硬件。
    * Subtractive  
    该模式提供烘焙的直射和间接光照，仅针对一个方向光渲染直接实时阴影。这种光照模式不能提供特别逼真的光照效果，适合于风格化的艺术效果或低端硬件。HDRP 渲染管线不支持此模式。
    * Shadowmask  
    该模式将实时直接光照与烘焙间接光照结合在一起，为远处的游戏对象启用烘焙阴影，并将烘焙阴影与实时阴影自动融合。这是最真实但也是最耗费资源的光照模式。

#### 光照贴图设置



#### Workflow Settings

这个部分包含调试相关的信息

* GPU Baking Device  
当光照设置中指定光照贴图（Lightmapper）为  GPU Progressive Lightmapper 时才会出现

### The Environment Tab

![Lighting-environment](./images/unity-lighting-environment-20210402.png)

### The Realtime Lightmaps Tab

将在新版 Unity 中弃用。

### The Backed Lightmaps Tab

![Lighting-scene](./images/unity-lighting-backed-lightmaps-20210402.png)