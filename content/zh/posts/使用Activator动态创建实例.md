---
title: "使用Activator动态创建实例"
date: 2021-09-27T11:07:17+08:00
draft: false
tags: ["csharp",]
categories: ["csharp",]
series:
description:
toc: true
authors:
  - zemelling
lastmod: 2021-09-27T11:07:17+08:00
featuredImage:
---

# Activator

Activator 包含能够创建本地或远程类型对象或获取对现有远程对象的引用的方法。

## 重要的方法

### CreateInstance

此方法通过调用与给定参数最匹配的类构造函数创建类的实例。默认情况下，如果被创建实例的类没有Public的构造函数或找不到匹配的构造函数，此方法将抛出异常。