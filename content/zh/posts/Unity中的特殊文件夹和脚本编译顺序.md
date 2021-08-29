---
title: "Unity中的特殊文件夹和脚本编译顺序"
date: 2021-04-05T13:24:47+08:00
draft: false
tags: ["unity",]
categories: ["unity",]
---

## 特殊文件夹

### Assets

Assets 文件夹是包含 Unity 项目使用的资源的主文件夹。Editor 中的 Project 窗口的内容直接对应于 Assets 文件夹的内容。大多数 API 函数都假定所有内容都位于 Assets 文件夹中，因此不要求显式提及该文件夹。但是，有些函数需要将 Assets 文件夹作为路径名的一部分添加（例如，AssetDatabase 类中的一些函数）。

### Editor

只要脚本位于 Editor 文件夹下，不论此 Editor 文件夹位于何处，都将此文件夹下的所有脚本视作编辑器脚本（Editor Scripts）。这类脚本只在编辑器模式下执行，而不在运行时模式下执行。此文件夹不会在发布时被打包。

### Editor default resources

编辑器脚本可通过 EditorGUIUtility.Load 函数按需加载资源文件，但是这些资源文件必须放在 Editor default resources 文件夹下，同时此文件夹必须位于 Project 视图的根目录下，也就是 Assets 文件夹下。此文件夹不会在发布时被打包。

### Gizmos

Gizmos 文件夹下放的是用于标识场景中对象的可视化标记图片等资源，通常经由 Gizmos.DrawIcon 函数加载显示在场景中，且该文件夹必须放置在 Project 视图根目录下，与 Editor default resources 一样。

### Plugins

如果做手机游戏开发一般 andoird 或者 ios 要接一些 sdk 可以把 sdk 依赖的库文件 放在这里，比如 .so .jar .a 文件。这样打完包以后就会自动把这些文件打在你的包中。

### Resources

与 Editor 类似，Resources 文件夹也可有多个，也是可以放在任意位置。同时 Resources 下的资源会全部被打包。
为了避免包体过大，特别是手机平台，尽量使用 AssetBoundle。

### Streaming Assets

Streaming Assets 通常用于存放一些原始格式不想要被压缩的文件，例如，用于播放 CG 的视频文件等。此文件夹只能存在一个，且必须位于 Project 视图根目录。此文件夹中的文件将直接被打包。

### 在 Assets 中隐藏文件或文件夹

During the import process, Unity ignores the following files and folders in the Assets folder (or a sub-folder within it):

Hidden folders.
Files and folders which start with ‘.’.
Files and folders which end with ‘~’.
Files and folders named cvs.
Files with the extension .tmp.

## 预定义的程序集与编译顺序

Unity 根据脚本文件在项目文件夹结构中的位置，以四个不同的阶段编译脚本。Unity 为每个阶段创建一个单独的 CSharp 项目文件 (.csproj) 和一个预定义的程序集。（如果没有符合编译阶段的脚本，Unity 不会创建相应的项目文件或程序集。）

当脚本引用在不同阶段编译的类（因此位于不同的程序集中）时，编译顺序很重要。基本规则是无法引用在当前阶段之后的阶段编译的任何内容。在当前阶段或早期阶段编译的所有内容则是完全可用的。

编译阶段如下：

|阶段	|  程序集名称 |	脚本文件 |
|---|---|---|
|1	|Assembly-CSharp-firstpass|	名为 Standard Assets、Pro Standard Assets 和 Plugins 的文件夹中的运行时脚本。|
|2	|Assembly-CSharp-Editor-firstpass|	名为 Editor 的文件夹（位于名为 Standard Assets、Pro Standard Assets 和 Plugins 的顶级文件夹中的任意位置）中的 Editor 脚本。|
|3	|Assembly-CSharp|	不在名为 Editor 的文件夹中的所有其他脚本。|
|4	|Assembly-CSharp-Editor|	其余所有脚本（位于名为 Editor 的文件夹中的脚本）。|

* Standard Assets 仅在 Assets 根文件夹中有效。
* 可通过创建程序集定义文件创建自定义的程序集。