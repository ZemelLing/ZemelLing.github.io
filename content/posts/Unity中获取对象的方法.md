---
title: "Unity中获取对象的方法"
date: 2021-04-04T17:39:33+08:00
draft: false
tags: ["unity",]
categories: ["unity",]
---

# 直接关联
  在脚本中创建public的想要关联的游戏对象类型的变量，随后在inspector中关联对应的游戏对象即可
```c#
public class Enemy : MonoBehaviour
{
    public GameObject player;
    
    // 其他变量和函数...
}
```
![inspector](https://docs.unity.cn/cn/2020.1/uploads/Main/GameObjectPublicVar.png)
# 子游戏对象
使用父游戏对象的变换组件来检索子游戏对象（因为所有游戏对象都具有隐式变换）
```C#
using UnityEngine;

public class WaypointManager : MonoBehaviour {
    public Transform[] waypoints;
    
    void Start() 
    {
        waypoints = new Transform[transform.childCount];
        int i = 0;
        
        foreach (Transform t in transform)
        {
            waypoints[i++] = t;
        }
    }
}
```
还可以使用 Transform.Find 函数按名称查找特定子对象： transform.Find("Gun");
# 通过Tag
可使用 GameObject.Find  GameObject.FindWithTag 和 GameObject.FindGameObjectsWithTag  函数按名称或Tag检索各个对象
```C#
GameObject player;

void Start() 
{
    player = GameObject.Find("MainHeroCharacter");
}

GameObject player;
GameObject[] enemies;

void Start() 
{
    player = GameObject.FindWithTag("Player");
    enemies = GameObject.FindGameObjectsWithTag("Enemy");
}
```
