---
title: "Lua学习记录（一）"
date: 2021-08-18T22:20:23+08:00
draft: false
tags: ["lua",]
categories: ["lua",]
series:
description:
toc: true
authors:
  - zemelling
lastmod: 2021-08-29T13:09:56+08:00
featuredImage:
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

## 数值

### 数值常量

数值常量可用科学计数法表示

使用 math.type 函数可区分 integer 和 float ， 否则，使用 type 则都是 number 。

lua 中除法使用是float相除，而整除算术运算符为 `//`

取模运算：
```lua
a % b == a - ((a // b) * b)
```

支持幂运算，运算符为 `^`

### 数学库

#### 取整函数

math.floor 向负无穷取整
math.ceil 向正无穷取整
math.modf 向0取整, 同时返回小数部分

## 字符串

使用 `..` 拼接字符串

```lua
"Hello " .. "World" --> Hello World
```

使用 `#` 可获取字符串长度

```lua
a = "hello"
print(#a) --> 5 这表示5字节
```

tonumber 函数可以将字符串转换为数值

## 表

表（Table）是 Lua 语言中最主要（事实上也是唯一的）和强大的数据结构。使用表，Lua 语言可以以一种简单、统一且高效的方式表示数组、集合、记录和其他很多数据结构。

使用构造器表达式创建表 {}

```lua
a = {} -- 创建了一个表
k = "x"
a[k] = 10 -- 键：x，值：10
a[20] = "great" -- 键：20，值：great
a["x"] --> 10
```

表的索引可以同时是不同的类型，按需增长空间。

Lua 中使用表存储全局变量

可以把表当作结构体使用

```lua
a = {}
a.x = 10
a.x --> 10
a.y --> nil
```

### 表构造器

1. 列表式

```lua
a = {"a", "b"} -- 索引为 1 2
```

2. 记录式

```lua
a = { x=10, y = 20}
```

3. 通用

```lua
opnames = {["+"] = "add", ["-"]="sub", ["*"] = "mul", ["/"] = "div"}
```

### 列表和序列

序列是不包含 nil 的列表

### 遍历表

## 函数

Lua 函数在调用时可以传入与函数定义不相同个数的参数，此时将抛弃多余的参数或将不足的参数设为 nil。

允许多返回值

```lua
function foo0 () end    -- 不返回结果
function foo1 () return "a" end    -- 返回1个结果
function foo2 () return "a", "b" end    -- 返回2个结果

x, y = foo2() -- x="a", y="b"
x = foo2() -- x="a", "b"被丢弃
x, y, z = 10, foo2() -- x=10, y="a", z="b"

-- 多值函数只有在表达式最末尾时才返回多个值，否则只返回首个值
x,y = foo2(), 20 -- x="a", y=20 ("b" 被丢弃)
x,y = foo0(), 20, 30 -- x=nil, y=20 (30 被丢弃)

return foo2() -- 会返回所有结果

(foo2()) -- 会强制只返回首个结果

function add(...) -- 其中 ... 表示可变长参数

-- 遍历可变参数的方法

-- 1. {...} 当作table，需要确保 ... 中没有 nil
-- 2. talbe.pack(...)  返回列表，同时包含指示参数个数的n
-- 3. select(n, ...) 返回第n个额外参数及其后所有参数 select("#", ...) 返回参数个数

```

### 尾调用

当一个函数的最后一个动作（注意：不是最后一条语句）是调用另一个函数而没有其他动作的时候，就形成了尾调用。

以下都不是尾调用：
```lua
function f(x) g(x) end -- 在调用完g后，f在返回前还有丢弃g返回的结果的动作
return g(x) + 1 -- 必须进行加法
return x or g(x) -- 必须把返回结果限制为1个
return (g(x)) -- 必须把返回结果限制为1个
```

# 局部变量

```lua
local i = 1 -- i此时就是局部变量
```

尽可能使用局部变量

# 控制结构

if while repeat for

if while for 的终结符为 end
repeat 的终结符为 until

## if then else

```lua
if op == "+" then
    r = a + b
elseif op == "-" then
    r = a - b
elseif op == "*" then
    r = a * b
elseif op == "/" then
    r = a / b
else
    error("invalid operation")
end
```

## while

```lua
local i = 1
while a[i] do
    print(a[i])
    i = i + 1
end
```

## repeat

直到型循环

```lua
local line
repeat
    line = io.read()
until line ~= ""
print(line)
```

注意：在lua中循环体内声明的局部变量在循环的条件测试处仍然有效

## for

存在两种for：1. 数值型 2. 泛型

### 数值型

```lua
for var = exp1, exp2, exp3 do
    something
end
--[[
    var 的值从 exp1 变化到 exp2, exp3 为变化的步长，若无 exp3 默认步长为1。
    循环开始前 exp1、exp2和exp3都会运行一遍
]]
```

### 泛型

泛型for会遍历迭代函数返回的所有值。

## return 

return 只能是代码块的最后一句

# 闭包

词法定界：当一个函数内嵌套另一个函数的时候，内函数可以访问外部函数的局部变量，这种特征叫做词法定界。

第一类值：在Lua中，函数是一个值，它可以存在于变量中、可以作为函数参数，也可以作为返回值return。

upvalue（非局部变量）：内嵌函数可以访问外部函数已经创建的局部变量，而这些局部变量则称为该内嵌函数的外部局部变量（即upvalue）。

函数是第一类值，这意味着函数可以保存在变量中。

所有函数都是匿名的，这是说函数名实际是保存函数的变量名。

闭包就是一个函数外加能够使该函数正确访问非局部变量所需的其他机制。

