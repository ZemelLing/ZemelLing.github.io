---
title: "Unity之动态将场景添加到BuildSettings中"
date: 2021-09-11T19:07:09+08:00
draft: false
tags: ["unity",]
categories: ["unity",]
series:
description:
toc: true
authors:
  - zemelling
lastmod: 2021-09-11T19:07:09+08:00
featuredImage:
---

# 代码

```c#
#region 将场景添加到 Build Settings

// 场景存放目录
var sceneDir = $"{Application.dataPath}/Scenes";
var sceneFiles = Directory.GetFiles(sceneDir, "*.unity", SearchOption.AllDirectories);

var scenes = new EditorBuildSettingsScene[sceneFiles.Length];
for (var i = 0; i < sceneFiles.Length; i++)
{
    // 替换斜杠
    var sceneFile = sceneFiles[i].Replace("\\", "/");
    // 使用相对路径
    var assetsFolderIndex = sceneFile.IndexOf("Assets", StringComparison.Ordinal);
    sceneFile = sceneFile.Substring(assetsFolderIndex);
    var scene = new EditorBuildSettingsScene(sceneFile, true);
    scenes[i] = scene;
}

EditorBuildSettings.scenes = scenes;

#endregion
```