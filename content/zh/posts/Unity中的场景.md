---
title: "Unity中的场景"
date: 2021-04-04T16:18:51+08:00
draft: false
tags: ["unity", "scene"]
categories: ["unity",]
series:
description:
toc: true
authors:
  - zemelling
lastmod: 2021-08-29T13:09:56+08:00
featuredImage:
---

## Scene

场景是 Unity 中由于放置游戏对象和UI元素的资源文件。

### 创建场景

* 使用新建场景对话框（ New Scene Dialog）根据指定的场景模板创建场景。
* 使用菜单或项目窗口根据项目基本的场景模板创建场景（这是最常见的方式）。
*

## Scene 模板

## 加载场景

无论是同步加载还是异步加载，都需要将 Scene 在 BuildSettings 中设置好。 

### 同步加载场景

```c#
public static void LoadScene(int sceneBuildIndex, SceneManagement.LoadSceneMode mode = LoadSceneMode.Single);
public static void LoadScene(string sceneName, SceneManagement.LoadSceneMode mode = LoadSceneMode.Single);
public static SceneManagement.Scene LoadScene(int sceneBuildIndex, SceneManagement.LoadSceneParameters parameters);
public static SceneManagement.Scene LoadScene(string sceneName, SceneManagement.LoadSceneParameters parameters);
```

* SceneManager.LoadScene 方法不会在调用方法的那一帧加载场景，而是下一帧加载。
* 此方法会强制之前的所有异步操作完成。
* 参数 sceneName 可以传递场景名称或路径，都不需要带后缀。
* sceneName 不区分大小写（除了从 AssetBundle 中加载场景）
* LoadSceneMode 指定场景的加载模式，Single 表示会替换当前场景，Additive 表示将指定场景加载到当前场景中。

### 异步加载场景

```c#
public static AsyncOperation LoadSceneAsync(string sceneName, SceneManagement.LoadSceneMode mode = LoadSceneMode.Single);
public static AsyncOperation LoadSceneAsync(int sceneBuildIndex, SceneManagement.LoadSceneMode mode = LoadSceneMode.Single);
public static AsyncOperation LoadSceneAsync(string sceneName, SceneManagement.LoadSceneParameters parameters);
public static AsyncOperation LoadSceneAsync(int sceneBuildIndex, SceneManagement.LoadSceneParameters parameters);
```

```c#
using System;
using System.Collections;
using UnityEngine;
using UnityEngine.SceneManagement;

public class MyTest : MonoBehaviour
{
    private Scene _scene;
    
    IEnumerator Start()
    {
        var parameters = new LoadSceneParameters(LoadSceneMode.Additive);

        var operation = SceneManager.LoadSceneAsync("MainMenuScene");

        while (!operation.isDone)
        {
            yield return null;
        }
    }
}
```
