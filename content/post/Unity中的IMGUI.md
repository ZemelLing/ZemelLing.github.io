---
title: "Unity中的IMGUI"
date: 2021-10-25T11:18:17+08:00
draft: false
tags: ["unity", "gui"]
categories: ["游戏开发",]
series:
description:
toc: true
authors:
  - zemelling
lastmod: 2021-10-25T11:18:17+08:00
featuredImage:
---

## 什么是 IMGUI
即时模式 GUI 系统（也称为 IMGUI）是一个完全独立的功能系统，不同于 Unity 基于游戏对象的主 UI 系统，IMGUI 需在实现脚本上调用 OnGUI 函数，并在其中实现绘制 UI 的代码。

## IMGUI 作用

* 创建游戏内调试显示和工具。
* 为脚本组件创建自定义检视面板。
* 创建新的编辑器窗口和工具以扩展 Unity 本身。

## 控件

* Label
* Button
* RepeatButton
* TextField
* TextArea
* Toggle
* ToolBar
* SelectionGrid
* HorizontalSlider
* VerticalSlider
* HorizontalScrollbar
* VerticalScrollar
* ScrollView
* Window

## 自定义控件

控件外观由 GUIStyle 决定，可在单个 GUISkin 中定义这些样式。GUISkin 只不过是 GUIStyle 的集合。

## 布局模式

### 固定布局与自动布局

GUI为使用固定布局，GUILayout为使用自动布局。

### 排列控件

在固定布局中，可将不同的控件放入Group中；在自动布局中，可将不同的控件放入Area、HorizontalGroup和VerticalGroup中。

#### 固定布局-Group

使用 GUI.BeginGroup() 和 GUI.EndGroup() 函数将要排列的空间包围。

#### 自定布局-Area

使用 GUILayout.BeginArea() 和 GUILayout.EndArea() 函数将要排列的空间包围。

#### 自定布局-HorizontalGroup/VeticalGroup

使用 GUILayout.BeginHorizontal/Vetical() 和 GUILayout.EndHorizontal/Vertical() 函数将要排列的空间包围。

### 使用 GUILayoutOption 定义一些控件

## 扩展编辑器

### 编辑器窗口

创建自定义编辑器窗口涉及以下简单步骤：

* 创建一个派生自 EditorWindow 的脚本。
* 使用代码触发窗口自行显示。
* 实现工具的 GUI 代码。

为了创建编辑器窗口，必须将脚本存储在称为“Editor”的文件夹中。

### 属性绘制器

借助属性绘制器，可以通过使用脚本上的特性或通过控制特定 Serializable 类来自定义 Inspector 窗口中某些控件的外观。

属性绘制器有两种用途：

* 自定义 Serializable 类的每个实例的 GUI。
* 通过自定义的属性特性来自定义脚本成员的 GUI。

#### 自定义 Serializable 类的 GUI

可以使用 CustomPropertyDrawer 属性将属性绘制器附加到 Serializable 类，然后传入属性绘制器所针对的 Serializable 类的类型。

#### 使用属性特性来自定义脚本成员的 GUI

### 自定义编辑器

[CustomEditor(typeof(LookAtPoint))]标记类同时继承Editor。