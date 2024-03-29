---
title: "Unity中的欧拉角和四元数"
date: 2021-06-01T17:18:09+08:00
draft: false
tags: ["unity",]
categories: ["游戏开发",]
series:
description:
toc: true
authors:
  - zemelling
lastmod: 2021-08-29T13:09:56+08:00
featuredImage:
---

在 Unity 有两种方式用于表示旋转，欧拉角和四元数。其中欧拉角用于查看和编辑，引擎内部使用四元数来表示。

## 欧拉角

欧拉角具有三个数字，分别表示绕 x 轴、y 轴和 z 轴旋转的角度。当使用欧拉角旋转物体时，需要按照某个特定的顺序依次绕轴旋转，才能够正确旋转。其最主要的问题是会导致万向锁。

## 万向锁问题

参考陀螺仪，将 Y 轴旋转方向看作是外圈，X 轴旋转方向看作是中圈，Z 轴旋转方向看作是内圈。
当旋转外圈时，其余三个部分都将跟着旋转。
当旋转中圈时，外圈不动，内圈和物体跟着旋转。
当旋转内圈时，只有物体跟着旋转。

当中圈旋转90度时，发生万向锁问题，因为此时的外圈和内圈重合了，此时外圈和内容绕同一轴旋转。

当在 Unity 中将某一物体的旋转x值设为90后，无论如何设旋转y和z的值都将使物体绕同一轴旋转。
直接使用旋转工具不会发生万向锁，这是因为 Unity 内部使用四元数表示旋转。

## 四元数

四元数由四个数字表示，xyzw，这四个数字不代表旋转角度。四元数用于表示物体的朝向和旋转。四元数解决了万向锁的问题，但一个四元数无法表示超过180度的旋转。

代码中可使用 Quaternion.Eular 将欧拉角转为四元数。