---
title: "Lua学习记录（一）"
date: 2021-08-18T22:20:23+08:00
draft: false
tags: ["lua",]
categories: ["lua",]
---

# 标识符

由任意字母、数字和下划线组成，同时首字符不能是数字，大小写敏感。

下划线加全大写字母（例如：_VERSION）组成的标识符，通常是特殊标识符（最好不要定义相同的标识符）。

一下是 lua 保留字

```lua
and break do else elseif end false goto for function if in local nil not or repeat return then true until while
```

# 注释

以下为单行注释

```lua
-- 单行注释
```

以下是多行注释

```lua
--[[
    多行注释
]]
```

# 分隔符

在 lua 中可使用分号 `;` 作为语句结束符，也可以不使用任何符号，通常只要保持风格一致就行

```lua
a = 1
b = a * 2

a = 1;
b = a * 2;

a = 1; b = a * 2
a = 1 b = a = 2 -- 可读性不佳，但写法也正确
```

# 变量

## 全局变量

全局变量无需声明就可使用，未初始化的全局变量值为 nil。将 nil 赋给全局变量时，Lua 会回收此全局变量。

# 类型

Lua 由于是动态语言，因此没有类型定义，每个值自包含类型信息。

## 八种基本类型

nil（空）、boolean（布尔）number（数值）、string（字符串）、userdata（用户数据）、function（函数）、thread（线程）和table（表）

```lua
type(nil) -- 函数type可获取值的类型信息
```

userdata 允许把任意的 C 语言数据保存在 Lua 变量中。
变量可以包含任何类型的值，其数据类型由其包含的值决定。

 ## nil

 nil 表示无效值

 ## Boolean

 Boolean的值为 true 和 false。Lua 中布尔不是条件测试的唯一类型，Lua 将 false 和 nil 之外的值是为真。 

 ### 逻辑运算符

 Lua 中的逻辑运算符包括 and 、 or 和 not，这些运算符都将 false 和 nil 视为假，其他值为真。and 和 or 都支持短路，not 的结果始终为 boolean 类型。

 and 操作符的运行结果为，第一个操作数为假时，输出第一个操作数，反之，输出第二个参数。
```lua
4 and 5 -- 输出 5
nil and 13 -- 输出 nil
false and 13 -- 输出 nil
```

or 操作符的运行结果为，第一个操作数为真时，输出第一个操作数，反之，输出第二个操作数。
```lua
0 or 5 -- 0  注意0和空字符串都表示真
false or "hi" -- "hi"
nil or false -- false
```



