---
title: "Unity中引用第三方程序集"
date: 2021-04-05T22:32:04+08:00
draft: false
tags: ["unity",]
categories: ["unity",]
---

在 Unity 中是通过插件（注意，此处的插件特指预编译的程序集或原生库，与编辑器插件代码区分开）的形式引用第三方程序集或原生库，其中引用的 .NET 程序集称作托管插件，原生库为原生插件。

## 插件的存放位置影响其导入设置

|文件夹|默认设置|
|-|-|
|Assets/../Editor|将插件设置为仅与 Editor 兼容，在构建到平台时不会使用。|
|Assets/../Editor/（x86 或 x86_64 或 x64）|将插件设置为仅与 Editor 兼容，根据子文件夹分配 CPU 值。|
|Assets/../Plugins/（x86_64 或 x64）|将 x64 独立平台插件设置为兼容。|
|Assets/../Plugins/x86|将 x86 独立平台插件设置为兼容。|
|Assets/Plugins/iOS|将插件设置为仅与 iOS 兼容。|
|Assets/Plugins/WSA/（x86 或 ARM）|将插件设置为仅与通用 Windows 平台兼容，如果存在 CPU 子文件夹，还会设置 CPU 值。可使用 Metro 关键字代替 WSA。|
|Assets/Plugins/WSA/（SDK80 或 SDK81 或 PhoneSDK81）|同上，还会设置 SDK 值，此后也可以添加 CPU 子文件夹。出于兼容性原因，SDK81 - Win81，PhoneSDK81 - WindowsPhone81。|
|Assets/Plugins/Tizen|将插件设置为仅与 Tizen 兼容。|
|Assets/Plugins/PS4|将插件设置为仅与 Playstation 4 兼容。|
|Assets/Plugins/Android|将插件设置为仅与 Android  兼容。|

## 托管插件

在创建托管插件的dll时需要注意API的兼容性问题，为了更好的跨平台，推荐使用 .Net Standard 2.0 级别的 API。
