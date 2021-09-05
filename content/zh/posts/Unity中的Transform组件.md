---
title: "Unity中的Transform组件"
date: 2021-09-05T11:24:21+08:00
draft: false
tags: ["unity", "component",]
categories: ["unity",]
series:
description:
toc: true
authors:
  - zemelling
lastmod: 2021-09-05T11:24:21+08:00
featuredImage:
---

# Transform组件

在 Unity 场景中的游戏对象都有 Transform 组件，用于存储和操作对象的位置、旋转和缩放，同时 Transform 支持枚举。

## 重要的属性

|属性|说明|
|-|-|
|childCount|该transform的直接孩子个数|
|forward|返回归一化向量，该向量在世界空间中为蓝色轴表示|
|right|红色轴|
|up|绿色轴|

## 重要的方法

|方法|说明|
|-|-|
|DetachChildren|将当前 Transform 下的孩子与其脱钩|
|Find|根据给定的游戏对象名称查找孩子（非递归查找）|
|GetSiblingIndex|获取当前 transform 在父亲的孩子中的索引（兄弟索引）。|
|TransformDirection|将方向从局部空间坐标转为世界空间坐标|
|LookAt|将forward方向指向目标|
|Translate|将游戏对象按照给的的方向和距离移动|
