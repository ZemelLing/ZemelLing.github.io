---
title: "Unity中游戏对象产生碰撞的条件"
date: 2021-06-22T22:29:17+08:00
draft: false
tags: ["unity",]
categories: ["unity",]
series:
description:
toc: true
authors:
  - zemelling
lastmod: 2021-08-29T13:09:56+08:00
featuredImage:
---

其中至少一个物体（必须运动的）必须带有碰撞器（collider）+刚体(Rigidbody)，另一个物体（可以静止也可以运动）也必须至少带有collider

1. 静态碰撞器

没有刚体而有碰撞器的物体，此时该碰撞器就是静态碰撞器。

2. 刚体碰撞器

如果游戏对象既有刚体又有碰撞器，则该碰撞器为刚体碰撞器。