---
title: "Unity的RuntimeInitializeOnLoadMethodAttribute特性"
date: 2021-09-01T15:27:16+08:00
draft: false
tags: ["unity", "attribute"]
categories: ["unity",]
series:
description:
toc: true
authors:
  - zemelling
lastmod: 2021-09-01T15:27:16+08:00
featuredImage:
---

# RuntimeInitializeOnLoadMethodAttribute

被标记为 RuntimeInitializeOnLoadMethodAttribute 的方法在游戏加载后就被调用，同时在Awake之后调用。
同时需要注意的是，所有被标记为 RuntimeInitializeOnLoadMethodAttribute 的方法，它们之间被调用的顺序是不一定的。

同时该特性还拥有可选参数 loadType，该参数决定了被标记的方法是在场景加载前还是加载后被调用。