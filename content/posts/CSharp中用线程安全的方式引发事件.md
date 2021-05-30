---
title: "CSharp中用线程安全的方式引发事件"
date: 2021-05-29T23:47:02+08:00
draft: false
tags: ["csharp", "event", "thread"]
categories: ["csharp",]
---

在不考虑线程安全的情况下常常会写如下引发事件的代码:

```c#
// 版本1
public event EventHandle<EventArgs> Something;

protected virtual void OnSomething(EventArgs e)
{
    if(Something != null) Something(this, e);
}
```

以上代码在单线程环境下能够正常运行，但是在多线程环境下就有可能导致问题。虽然对 Something 做了空引用的判断，但是在调用 Something 前有可能被另一个线程从委托链中移除委托，使得 Something 为空，导致空引用调用的异常。这种情况下就产生了以下写法：

```c#
// 版本2
public event EventHandle<EventArgs> Something;

protected virtual void OnSomething(EventArgs e)
{
    EventHandle<EventArgs> tmp = Something;
    if(tmp != null) tmp(this, e);
}
```

这个方法利用了委托不可变的特性，将 Something 的引用复制到 tmp 变量中，此时 tmp 引用赋值发生时的委托链，在通过空引用检查后，即使 Something 取消了委托链中的订阅，由于委托不可变的特性，tmp 不会被影响，tmp 也就能够正常被调用。

C# 6.0 以后版本 2 的代码可以写成以下形式：

```c#
// 版本3
public event EventHandle<EventArgs> Something;

protected virtual void OnSomething(EventArgs e)
{
    Something?.Invoke(this, e);
}
```

版本 2 和版本 3 的代码通常是够用的，因为 JIT 编译器通常不会将不可变类型的局部变量优化掉。以下展示了最严格的多线程环境下引发事件、委托的代码：

```c#
// 版本4
public event EventHandle<EventArgs> Something;

protected virtual void OnSomething(EventArgs e)
{
    var tmp = Volatile.Read(ref Something);
    tmp?.Invoke(this, e);
}
```