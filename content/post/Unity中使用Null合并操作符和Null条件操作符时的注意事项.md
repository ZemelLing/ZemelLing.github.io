---
title: "Unity中使用Null合并操作符和Null条件操作符时的注意事项"
date: 2021-08-30T08:37:49+08:00
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

# Unity中使用??和?.操作符时的注意事项

## 不要在继承自 UnityEngine.Object 的对象上使用

这是由于这些操作符并不使用由 UnityEngine.Object 重载的相等操作符，因此有可能导致意外通过 Unity 的生命周期检查。这种情况下最好使用显示Null判断或调用 System.Object.ReferenceEquals 函数。

具体可参考JetBrains提供的这篇文章 "[Possible unintended bypass of lifetime check of underlying Unity engine object](https://github.com/JetBrains/resharper-unity/wiki/Possible-unintended-bypass-of-lifetime-check-of-underlying-Unity-engine-object)"