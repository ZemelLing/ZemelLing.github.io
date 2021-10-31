---
title: "Unity中使用CustomEditor自定义脚本的Inspector面板"
date: 2021-09-05T20:43:12+08:00
draft: false
tags: ["unity", "data save"]
categories: ["游戏开发",]
series:
description:
toc: true
authors:
  - zemelling
lastmod: 2021-09-05T20:43:12+08:00
featuredImage:
---

1. 自定义脚本

```c#
using UnityEngine;
public class LookAtPoint : MonoBehaviour
{
    public Vector3 lookAtPoint = Vector3.zero;

    void Update()
    {
        transform.LookAt(lookAtPoint);
    }
}
```

2. 使用 CustomEditor 特性

```c#
using UnityEngine;
using UnityEditor;

[CustomEditor(typeof(LookAtPoint))] // 指定为哪个组件自定义编辑器界面
[CanEditMultipleObjects]  // 指示Unity可以为多个对象的同一组件同时编辑
public class LookAtPointEditor : Editor 
{
    SerializedProperty lookAtPoint;

    void OnEnable()
    {
        lookAtPoint = serializedObject.FindProperty("lookAtPoint");
    }

    // 此处自定义组件的 Inspector 界面
    public override void OnInspectorGUI()
    {
        serializedObject.Update();
        EditorGUILayout.PropertyField(lookAtPoint);
        if (lookAtPoint.vector3Value.y > (target as LookAtPoint).transform.position.y)
        {
            EditorGUILayout.LabelField("(Above this object)");
        }
        if (lookAtPoint.vector3Value.y < (target as LookAtPoint).transform.position.y)
        {
            EditorGUILayout.LabelField("(Below this object)");
        }
        
            
        serializedObject.ApplyModifiedProperties();
    }

    // 在 Scene 添加信息
    public void OnSceneGUI()
    {
        var t = (target as LookAtPoint);

        EditorGUI.BeginChangeCheck();
        Vector3 pos = Handles.PositionHandle(t.lookAtPoint, Quaternion.identity);
        if (EditorGUI.EndChangeCheck())
        {
            Undo.RecordObject(target, "Move point");
            t.lookAtPoint = pos;
            t.Update();
        }
    }
}
```