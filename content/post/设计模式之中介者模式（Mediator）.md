---
title: "设计模式之中介者模式（Mediator）"
date: 2021-05-01T12:15:01+08:00
draft: false
tags: ["design pattern", "mediator"]
categories: ["编程基础",]
series:
description:
toc: true
authors:
  - zemelling
lastmod: 2021-08-29T13:09:56+08:00
featuredImage:
---

# 中介者模式

## 定义

将子系统间的互动委托给中介者，减少子系统相互调用的耦合，子系统通过中介者交互。

## UML

![中介者模式UML](images/designPattern/Mediator.jpg)

## 代码

```c#
public abstract class Mediator
    {
        public abstract void SendMessage(Colleague colleague, string message);
    }

    public class ConcreteMediator : Mediator
    {
        private ConcreteColleague1 _colleague1 = null;
        private ConcreteColleague2 _colleague2 = null;

        public void SetConcreteColleague1(ConcreteColleague1 colleague1)
        {
            _colleague1 = colleague1;
        }
        
        public void SetConcreteColleague2(ConcreteColleague2 colleague2)
        {
            _colleague2 = colleague2;
        }
        
        public override void SendMessage(Colleague colleague, string message)
        {
            if (colleague == _colleague1)
            {
                _colleague2.Request(message);
            }
            
            if (colleague == _colleague2)
            {
                _colleague1.Request(message);
            }
        }
    }
    
    public abstract class Colleague
    {
        protected Mediator Mediator;

        public Colleague(Mediator mediator)
        {
            Mediator = mediator;
        }

        public abstract void Request(string message);
    }

    public class ConcreteColleague1 : Colleague
    {
        public ConcreteColleague1(Mediator mediator) : base(mediator)
        {
        }

        public void Action()
        {
            Mediator.SendMessage(this, $"{nameof(ConcreteColleague1)}发送通知");
        }

        public override void Request(string message)
        {
            Debug.Log($"{nameof(ConcreteColleague1)}.Request: {message}");
        }
    }
    
    public class ConcreteColleague2 : Colleague
    {
        public ConcreteColleague2(Mediator mediator) : base(mediator)
        {
        }

        public void Action()
        {
            Mediator.SendMessage(this, $"{nameof(ConcreteColleague2)}发送通知");
        }

        public override void Request(string message)
        {
            Debug.Log($"{nameof(ConcreteColleague2)}.Request: {message}");
        }
```

## 优点

* 对于子系统或子模块而言，减少了互相间的依赖，只需依赖中介者类就可以与其他子系统交互。
* 同时子系统被依赖的程度也降低了（只被中介者依赖），当子系统变动时，只会影响中介者类。

## 注意事项

* 通过结合其他设计模式以避免中介者过于臃肿。
