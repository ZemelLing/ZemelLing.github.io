---
title: "设计模式之模板方法模式（TemplateMethod）"
date: 2021-05-10T11:36:42+08:00
draft: false
tags: ["design pattern", "bridge"]
categories: ["编程基础",]
series:
description:
toc: true
authors:
  - zemelling
lastmod: 2021-08-29T13:09:56+08:00
featuredImage:
---

## 定义

在一个操作方法中定义算法流程，其中某些步骤由子类完成。模板方法模式让子类在不变更原有流程的情况下，还能够重新定义其中的步骤。