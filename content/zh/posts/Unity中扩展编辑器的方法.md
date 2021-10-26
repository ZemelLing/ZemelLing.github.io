---
title: "Unity中扩展编辑器的方法"
date: 2021-10-24T11:26:36+08:00
draft: true
tags: ["unity", "editor"]
categories: ["unity",]
series:
description:
toc: true
authors:
  - zemelling
lastmod: 2021-10-24T11:26:36+08:00
featuredImage:
---

## 拓展 Project 视图
  ### 拓展 Project 右键菜单
  使用 MenuItem 特性标记任意类的静态方法，此方法就成为菜单项触发的对应方法，菜单路径以 Assets 开头的将拓展 Project 右键菜单。
  ### 拓展布局
  主要是设置 EditorApplication.projectWindowItemOnGUI 委托，同时使用 InitializeOnLoadMethod 特性确保所标记的方法能够在Unity编辑器加载或重新编译时执行。
  ### 监听事件
## 拓展 Hierarchy 视图
  ### 拓展菜单
  使用MenuItem，路径开头为 GameObject。
  ### 拓展布局
  EditorApplication.hierarchyWindowItemOnGUI 类似 Project视图的扩展布局
  ### 重写菜单
  EditorUtility.DisplayPopupMenu
## 拓展 Inspector 视图
  ### 拓展原生组件
  自定义类需要继承Editor，打上CustomEditor()特性表示自定义哪个组件，OnInspectorGUI()可以对它进行重新绘制，base.OnInspectorGUI()表示是否绘制父类原有元素。
  ### 拓展继承组件
  Unity将大量的Editor绘制方法封装在内部的DLL文件里，开发者无法调用它的方法。如果想解决这个问题，可以使用C# 反射的方式调用内部未公开的方法。
  ### 组件不可编辑
  1. GUI.enable = false; 
  2. 可以给游戏对象或组件设置hideFlags
  HideFlags:
  * HideFlags.None：清除状态。
  * HideFlags.DontSave：设置对象不会被保存（仅编辑模式下使用，运行时剔除掉）。
  * HideFlags.DontSaveInBuild：设置对象构建后不会被保存。
  * HideFlags.DontSaveInEditor：设置对象编辑模式下不会被保存。
  * HideFlags.DontUnloadUnusedAsset：设置对象不会被Resources.UnloadUnusedAssets()卸载无用资源时卸掉。
  * HideFlags.HideAndDontSave：设置对象隐藏，并且不会被保存。
  * HideFlags.HideInHierarchy：设置对象在层次视图中隐藏。
  * HideFlags.HideInInspector：设置对象在控制面板视图中隐藏。
  * HideFlags.NotEditable：设置对象不可被编辑。
  ### Context 菜单

  ```
  [MenuItem("CONTEXT/Transform/New Context 1")]
  private static void NewContext1(MenuCommand command)
  {
      Debug.Log(command.context.name);
  }
  ```
  MenuCommand 可用与获取脚本对象
## 拓展 Scene 视图
  ### 辅助元素
  重写 OnDrawGizmosSelected() 或 OnDrawGizmos() 方法
  ### 辅助UI
  需要继承 Editor 类型，重写 OnSceneGUI 方法，使用 Handles、GUI、GUILayout等类绘制。还要为类打上 CustomEditor 特性
  ### 常驻辅助UI
  重写SceneView.onSceneGUIDelegate，在Handles.BeginGUI()和Handles.EndGUI()中间绘制完成
  ### 禁用选中对象

  直接在Scene视图中很容易选择到子节点，此时可以给它绑定一个[SelectionBase]标记，这样该脚本下的所有节点都会定位到绑定这个标记的对象身上

## 拓展 Game 视图

在脚本类上标记[ExecuteInEditMode]，表示此脚本可以在编辑模式中生效。

## MenuItem菜单

  ### 覆盖系统菜单
  只要确保 MenuItem 特性的路径与要覆盖的系统菜单路径一致就行。
  ### 自定义菜单
  MenuItem 的路径自定义就行，当然可以使用 priority 排序。
  ### 原生自定义菜单
  EditorUtility.DisplayCustomMenu()
  ### 拓展全局自定义快捷键
  * %：表示Windows 下的 Ctrl键和macOS下的Command键。
  * #：表示Shift键。
  * &：表示Alt键。
  * LEFT/RIGHT/UP/DOWN：表示左、右、上、下4个方向键。
  * F1…F12：表示F1至F12菜单键。
  * HOME、END、PGUP和PGDN键。

## 面板拓展
  ### Inspector面板
  ### EditorWindows窗口
  继承 EditorWindow
  ### EditorWindows下拉菜单
  继承 EidtorWindow 的同时继承 IHasCustomMenu
  ### 预览窗口
  继承 ObjectPreview,设置[CustomPreview]特性。
  ### 获取预览信息
  
