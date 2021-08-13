---
title: "设计模式之状态模式（State）"
date: 2021-04-26T23:41:37+08:00
draft: false
tags: ["design pattern", "state"]
categories: ["design pattern",]
---

## 定义

让一个对象的行为随着内部状态的变更而改变，而该对象就像是换了类一样。

## UML


## Code

```c#
using UnityEngine;

public class Context
{
    private State _state;

    public void Request(int value)
    {
        _state.Handle(value);
    }

    public void SetState(State state)
    {
        Debug.Log($"Context.SetState:{state}");
        _state = state;
    }
}

public abstract class State
{
    protected Context _context;

    public State(Context context)
    {
        _context = context;
    }

    public abstract void Handle(int value);
}

public sealed class ConcreteStateA : State
{
    public ConcreteStateA(Context context) : base(context)
    {
    }

    public override void Handle(int value)
    {
        Debug.Log("ConcreteStateA.Handle");
        if (value > 10)
            _context.SetState(new ConcreteStateB(_context));
    }
}

public sealed class ConcreteStateB : State
{
    public ConcreteStateB(Context context) : base(context)
    {
    }

    public override void Handle(int value)
    {
        Debug.Log("ConcreteStateB.Handle");
        if (value > 20)
            _context.SetState(new ConcreteStateC(_context));
    }
}

public sealed class ConcreteStateC : State
{
    public ConcreteStateC(Context context) : base(context)
    {
    }

    public override void Handle(int value)
    {
        Debug.Log("ConcreteStateC.Handle");
        if (value > 30)
            _context.SetState(new ConcreteStateA(_context));
    }
}
```