---
title: "Unity中的程序集定义"
date: 2021-04-05T13:15:45+08:00
draft: false
tags: ["unity", "assembly"]
categories: ["unity",]
series:
description:
toc: true
authors:
  - zemelling
lastmod: 2021-08-29T13:09:56+08:00
featuredImage:
---

## 程序集定义（Assembly Definitions）

Unity 使用程序集定义和程序集引用（Assembly References）组织代码结构，同时 Unity 默认使用预定义的程序集，Assembly-CSharp.dll，来组织代码。

但是这种结构并不适合中大型项目，单一程序集将导致代码结构不够清晰，耦合高，难以重构，对任意脚本文件都会重新编译所有文件等问题，因此，Unity 提供了程序集定义文件来自定义程序集，以此组织代码。

## 创建自定义程序集

1. 将所有需要组织到同一程序集的脚本文件移动到任意非特殊文件夹中。
2. 在上面指定的文件夹下创建 ***程序集定义资源（Assembly Definition Assets）***

注意：
* 包含程序集定义资源的文件夹会单独生成一个DLL，同时包含该文件夹下的子文件夹，除非子文件夹中包了程序集定义资源或程序集引用资源

* 为了将非子文件夹下的脚本添加到一个已存在的程序集中，可在此文件夹下创建程序集引用资源。

* 可通过设置程序集定义文件中的 Platforms 设置来创建特定平台的程序集或只用于 Editor 的程序集。


## 引用（References）和依赖（Dependencies）

当类型 A 使用到类型 B 时，可称类型 A ***依赖*** 与类型 B，于此同时，如果 A 和 B 分属不同的程序集，则程序集 A 必须 ***引用*** 程序集 B，否则类型 A 就无法使用类型 B，这就是引用和依赖的区别与联系。

可以通过程序集定义资源设置程序集的引用关系。

使用程序集定义的选项控制项目中使用的程序集之间的引用。程序集定义设置包括：

* [Auto Referenced] – 预定义程序集是否引用相应程序集
* [Assembly Definition References] – 对使用程序集定义创建的其他项目程序集的引用
* [Override References] + [Assembly References] – 对预编译（插件）程序集的引用
* [No Engine References] – 对 UnityEngine 程序集的引用

### 默认引用

预定义程序集将引用所有其他程序集（程序集定义资源和预编译程序集），而程序集定义资源又引用所有预编译程序集。

![默认引用关系](https://docs.unity3d.com/cn/2020.3/uploads/Main/AssemblyDependencies.png)

引用关系也决定了编译顺序，所以要避免自定义程序集使用预定义程序集中的类。

通过关闭 ***自动引用选项（Auto Referenced Option）*** 可以避免预定义程序集引用相应的自定义程序集。
同样的，可以通过关闭自动引用选项避免插件程序集被自定程序集引用。

*注意避免循环引用*

### 程序集定义引用（Assembly Definition Reference）

"程序集定义引用"用于将指定文件夹下的所有脚本关联至由其指定的"程序集定义（Assembly Definition）"的自定义程序集下，并从原有的程序集下移除。