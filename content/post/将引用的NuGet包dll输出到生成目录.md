---
title: "将引用的NuGet包dll输出到生成目录"
date: 2021-08-10T10:58:20+08:00
draft: false
tags: ["nuget","vs"]
categories: ["问题记录",]
series:
description:
toc: true
authors:
  - zemelling
lastmod: 2021-08-29T13:09:56+08:00
featuredImage:
---

修改项目文件，在 ``<PropertyGroup>`` 处添加如下片段：

```
<CopyLocalLockFileAssemblies>true</CopyLocalLockFileAssemblies>
```