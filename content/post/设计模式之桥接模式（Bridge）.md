---
title: "设计模式之桥接模式（Bridge）"
date: 2021-05-02T10:39:22+08:00
draft: false
tags: ["design pattern", "bridge"]
categories: ["design pattern",]
series:
description:
toc: true
authors:
  - zemelling
lastmod: 2021-08-29T13:09:56+08:00
featuredImage:
---
# 桥接模式

## 定义

将抽象和实现分离，以达到二者独立进化的目的。

## UML

![中介者模式UML](images/designPattern/Bridge.jpg)

## 代码

```c#
public abstract class Implementor
    {
        public abstract void OperatorImp();
    }

    public class ConcreteImplementor1 : Implementor
    {
        public override void OperatorImp()
        {
            Debug.Log($"执行{nameof(ConcreteImplementor1)}.{nameof(OperatorImp)}");
        }
    }
    
    public class ConcreteImplementor2 : Implementor
    {
        public override void OperatorImp()
        {
            Debug.Log($"执行{nameof(ConcreteImplementor2)}.{nameof(OperatorImp)}");
        }
    }

    public abstract class Abstraction
    {
        private Implementor _implementor;

        public void SetImplementor(Implementor imp)
        {
            _implementor = imp;
        }

        public virtual void Operation()
        {
            _implementor?.OperatorImp();
        }
    }

    public class RefinedAbstraction1 : Abstraction
    {
        public override void Operation()
        {
            Debug.Log($"物件{nameof(RefinedAbstraction1)}");
            base.Operation();
        }
    }
    
    public class RefinedAbstraction2 : Abstraction
    {
        public override void Operation()
        {
            Debug.Log($"物件{nameof(RefinedAbstraction2)}");
            base.Operation();
        }
    }
```

## 应用场景

* 需要抽象和实现的分离时。
* 当两个群组因为功能的需求，想要连接合作，但又希望两组类能够各自发展不受彼此影响时。