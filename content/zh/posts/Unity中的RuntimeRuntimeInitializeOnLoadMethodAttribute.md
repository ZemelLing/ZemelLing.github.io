---
title: "Unity中的RuntimeRuntimeInitializeOnLoadMethodAttribute"
date: 2021-04-26T22:27:49+08:00
draft: false
tags: ["unity", "unity api", "attribute"]
categories: ["unity",]
---

被此特性标注的方法将在 Awake 方法之后被 Unity 循环调用，而且被此特性标记的所有方法，它们之间的执行顺序不是固定的。

示例：
```c#
using UnityEngine;

public class MyTest : MonoBehaviour
{
    [RuntimeInitializeOnLoadMethod]
    static void OnRuntimeMethodLoad()
    {
        Debug.Log("After Scene is loaded and game is running");
    }

    [RuntimeInitializeOnLoadMethod]
    static void OnSecondRuntimeMethodLoad()
    {
        Debug.Log("SecondMethod After Scene is loaded and game is running.");
    }
        
    private void Awake()
    {
        Debug.Log("Awake method");
    }

    private void Start()
    {
        Debug.Log("Start method");
    }
}
```

结果：
![result](/images/Unity中的RuntimeRuntimeInitializeOnLoadMethodAttribute-20210426.png)