---
title: "Unity中事件函数的执行顺序"
date: 2021-04-14T09:37:25+08:00
draft: false
tags: ["unity",]
categories: ["unity",]
---

![脚本生命周期流程图](https://docs.unity3d.com/cn/2020.3/uploads/Main/monobehaviour_flowchart.svg)

## 阶段

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

