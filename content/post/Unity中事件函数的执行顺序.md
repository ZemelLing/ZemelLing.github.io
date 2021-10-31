---
title: "Unity中事件函数的执行顺序"
date: 2021-04-14T09:37:25+08:00
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

![脚本生命周期流程图](https://docs.unity3d.com/cn/2020.3/uploads/Main/monobehaviour_flowchart.svg)

## 阶段

### 主要阶段

1. 初始化阶段，包含 Awake() Start()
2. 物理系统更新阶段，包含 FixedUpdate()、动画更新、OnTriggerXXX、OnCollisionXXX，物理系统可能一帧内更新多次。
3. 输入事件，OnMouseXXX
4. 游戏逻辑阶段，Update、从协程恢复、动画更新、LateUpdate
5. 游戏渲染阶段，OnWillRenderObject、OnPreCull、。。。
6. Gizmo 渲染阶段
7. GUI 渲染阶段
8. 

### 加载第一个场景

场景开始时将调用一下函数，且场景中的每个对象调用一次

* Awake 在所以对象的 Start 函数之前被调用，并且如果对象此时未激活则不调用，直到激活后才调用。

* OnEnable 只有对象处于激活状态时才可以调用，且启用对象后立即调用此函数。在创建 MonoBehaviour 实例时会执行此函数。

### 编辑器（Editor）

* Reset 当脚本首次被附加到一个游戏对象或使用 Reset 命令时，该函数被调用，用于初始化脚本数据。

* OnValidate 脚本被加载或脚本属性值被修改时调用。

###  首次更新帧之前

* Start 脚本实例化后的第一帧更新前，此函数被调用。

### 帧之间

* OnApplicationPause 在帧的结尾处调用此函数（在正常帧更新之间有效检测到暂停）。在调用 OnApplicationPause 之后，将发出一个额外帧，从而允许游戏显示图形来指示暂停状态。

## 脚本

### 脚本执行顺序

脚本既可以在运行时动态添加在游戏对象上，也可以运行游戏前预制挂在游戏对象上。动态添加的脚本按添加的先后顺序决定执行顺序。但是静态脚本因为提前挂在了游戏对象上，所以初始化的顺序就不一样了。在Script Execution Order中可以设置脚本的执行顺序。

Awake适合做初始化，Start适合安全的访问其他脚本数据。

避免无效的生命周期函数。

### 脚本序列化

### ScriptableObject

开发中有时候需要序列化一些编辑器数据，仅仅是数据，完全没必要依赖游戏对象使用游戏脚本，此时ScriptableObject就是最佳选择了。

### 自定义脚本特性(Attribute)
分别继承 PropertyAttribute和PropertyDrawer

### CustomYieldInstruction
继承 CustomYieldInstruction 可实现自定义协程。

### 工作线程

使用 Job System 系统
