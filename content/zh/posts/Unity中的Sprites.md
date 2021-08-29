---
title: "Unity中的Sprites"
date: 2021-05-17T20:32:36+08:00
draft: false
tags: ["unity", "sprites", "2d"]
categories: ["unity",]
series:
description:
toc: true
authors:
  - zemelling
lastmod: 2021-08-29T13:09:56+08:00
featuredImage:
---

## 什么是 Sprites

Sprites 是 2D 图形对象，在 3D 中本质是标准纹理。

## Unity 中的 Sprite 工具

### Sprite Creator

用于创建占位用的 Sprite，之后有具体的素材后再替换。

### Sprite Editor

用于从一张图片或纹理中切割出多个 Sprite。通常会将关联的 Sprite 放入一张图片中，以形成图集，降低资源消耗。

### Sprite Packer

使用Sprite Packer可以优化项目对视频内存的使用和性能。

## 设置 Sprite

如果项目的模式是 2D，那么导入的图片都将作为 Sprite 存在。3D 项目中默认是 Texture, 可将 Texture 类型改为 Sprite(2D and UI).

