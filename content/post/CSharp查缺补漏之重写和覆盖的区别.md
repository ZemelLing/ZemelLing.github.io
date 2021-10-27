---
title: "CSharp查缺补漏之重写(override)和覆盖(new)的区别"
date: 2021-10-25T09:54:30+08:00
draft: false
tags: ["CSharp", ]
categories: ["CSharp",]
series:
description:
toc: true
authors:
  - zemelling
lastmod: 2021-10-25T09:54:30+08:00
featuredImage:
---

* 不管是重写还是覆盖都不会影响父类自身的功能（废话，肯定的嘛，除非代码被改）。

* 当用子类创建父类的时候，如 C1 c3 = new C2()，重写会改变父类的功能，即调用子类的功能；而覆盖不会，仍然调用父类功能。

* 虚方法、实方法都可以被覆盖（new），抽象方法，接口 不可以。

* 抽象方法，接口，标记为virtual的方法可以被重写（override），实方法不可以。

* 重写使用的频率比较高，实现多态；覆盖用的频率比较低，用于对以前无法修改的类进行继承的时候。

原文完整内容地址：http://www.cnblogs.com/xumingxiang/archive/2012/04/14/override_new.html