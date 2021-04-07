---
title: "Rider用法记录"
date: 2021-04-06T12:31:02+08:00
draft: false
tags: ["rider", ]
categories: ["rider", ]
---

## 设置Snippet

File->Setting->Editor->livetemplates

![设置Snippet](/images/rider设置snippet-20210406.png)

* 右上角的按钮从上到下分别是：新建Snippet，复制已有的Snippet，删除Snippet
* 可通过设置变量（$variablename$）来占位，使用时在变量的位置填入内容即可，或编辑变量，使用默认的宏。

示例
```c#
private static $classname$ _instance;

public static $classname$ Instance => _instance ??= new $classname$();

private $classname$ ()
{
    $END$
}
```

## 关闭 Ctrl+S 时自动同步到 Unity 的功能，避免卡顿

Settings -> Languages&Frameworks -> Unity Engine -> 取消勾选 Automatically refresh assets in Unity