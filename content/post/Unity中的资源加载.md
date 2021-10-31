---
title: "Unity中的资源加载"
date: 2021-04-04T16:18:57+08:00
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

## 加载方式

### 将 Prefab 拖拽到 Inspector 的脚本上

这是最简单的方式，但是在实际项目中通常不使用，因为这种方式不够灵活，拖拽的方式低效，存在大量的隐式耦合。

```c#
public class Test : MonoBehaviour
{
    public GameObject prefab;
    
    void Start()
    {
        var go = GameObject.Instantiate(prefab);
    }
}
```

### 将 Prefab 放到 Resources 文件夹下，通过 Resources.Load 方法加载

这种方式实现也很简单，虽然比上一种方式好些，但实际项目中也不大使用，原因在于打包时会将 Resources 下的内容一块打包造成包体过大，同时存在最大容量为2GB的限制。

```c#
public class ResourceTest : MonoBehaviour
{
    void Start()
    {
        var prefab = Resources.Load("prefab");
        var go = GameObject.Instantiate(prefab);
    }
}
```

### 使用 AssetDataBase.Load 方法加载

### 使用 AssetBundle 加载

