---
title: "Unity中使用Addressable"
date: 2021-09-06T18:56:37+08:00
draft: false
tags: ["unity", "data save"]
categories: ["游戏开发",]
series:
description:
toc: true
authors:
  - zemelling
lastmod: 2021-09-06T18:56:37+08:00
featuredImage:
---

# 使用步骤

## 标记资源为Addressable

### 方式一：在资源的Inspector界面

### 方式二：在 Addressables Groups 窗口

## 构建可寻址内容(Addressable Content)

### 方式一：使用 Editor

在 Addressables Groups 窗口右上角选择 Build > New Build > Default Build Script.

### 方式二：使用脚本 API

`AddressableAssetSettings.BuildPlayerContent()`

## 使用 Addressable 资源

### 方式一：通过 address 加载或实例化资源

* 加载资源 `Addressables.LoadAssetAsync<GameObject>("AssetAddress");`
* 实例化资源 `Addressables.InstantiateAsync("AssetAddress");`

示例：
```c#
using System.Collections;
using System.Collections.Generic;
using UnityEngine.AddressableAssets;
using UnityEngine;

public class AddressablesExample : MonoBehaviour {

    GameObject myGameObject;

        ...
        Addressables.LoadAssetAsync<GameObject>("AssetAddress").Completed += OnLoadDone;
    }

    private void OnLoadDone(UnityEngine.ResourceManagement.AsyncOperations.AsyncOperationHandle<GameObject> obj)
    {
        // In a production environment, you should add exception handling to catch scenarios such as a null result.
        myGameObject = obj.Result;
    }
}

```

### 方式二：使用 AssetReference 类

# 构建时注意事项

## 在 StreamingAssets 文件夹中的资源

## 提前下载

远程内容目录的优先级高于本地内容目录。