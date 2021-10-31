---
title: "Lua学习记录（三）"
date: 2021-08-29T14:44:11+08:00
draft: false
tags: ["lua",]
categories: ["编程语言",]
series:
description:
toc: true
authors:
  - zemelling
lastmod: 2021-08-29T14:44:11+08:00
featuredImage:
---

# 位和字节

## 位运算

|运算符|操作|
|-|-|
|&|按位与|
|\||按位或|
|~|按位异或|
|<<|逻辑左移|
|>>|逻辑右移|
|~|按位取反（一元运算）|

Lua 中没有提供算术右移和左移，只提供了逻辑的。

## 无符号整数

Lua 中使用 math.ult 函数比较两个无符号整数的大小。

## 打包和解包二进制数据

string.pack 函数会把值打包成二进制数据
string.unpack 则反之

string.pack 和 string.unpack 的第一个参数是格式化字符串，函数将根据这个字符串打包或解包。

string.unpack 还会返回最后一个读取的元素在字符串中的位置。其第三个参数用于指定开始读取的位置。

### 整型数相关
|格式字符串|含义|
|-|-|
|b|char|
|h|short|
|i|int|
|l|long|
|j|Lua中整型数的大小|
|in|n为1~16的数，表示会产生n字节的整型数|
以上有关整数的部分，都有对应的大写表示，用于表示无符号整型数

```lua
s = "\xFF"
string.unpack("b", s) --> -1 2
string.unpack("B", s) --> 255 2
```

### 字符串相关
|格式字符串|含义|
|-|-|
|z|以\0结尾的字符串|
|cn|定长字符串，n表示打包字符串的长度|
|sn|显式长度的字符串(会在字符串前加上字符串的长度)，n是用于保存字符串长度的无符号整数的大小|

```lua
s = string.pack("s1", "hello")
for i = 1, #s do print((string.unpack("B", s, i))) end
--> 5 (length)
--> 104 ('h')
--> 101 ('e')
--> 108 ('l')
--> 108 ('l')
--> 111 ('o')
```

### 浮点型数

|格式字符串|含义|
|-|-|
|f|单精度浮点数|
|d|双精度浮点数|
|n|Lua语言浮点数|


## 二进制文件

io.open("file path","rb") 使用字母b表示操作的是二进制文件

# 数据结构

## 数组

```lua
local a = {} -- 数组
for i = 1, 1000 do
  a[i] = 0
end

squares = {1, 4, 9, 16, 25} -- 使用表构造器创建数组
```

## 矩阵及多维数组

### 表示矩阵的方式1

```lua
-- N x M 矩阵 元素都为0
local mt = {}
for i = 1, N do
  local row = {}
  mt[i] = row
  for j = 1, M do
    row[j] = 0
  end
end
```

### 表示矩阵的方式2

```lua
-- 使用一维表示二维
local mt = {}
for i = 1, N do
  local aux = (i - 1) * M
  for j = 1, M do
    mt = [aux + j] = 0
  end
end
```

## 链表

### 单链表

```lua
list = nil -- 根节点
list = {next = list, value = v} -- 在表头处插入节点

-- 遍历
local l = list
while l do
  visit l.value
  l = l.next
end
```

## 队列

### 双端队列

```lua
function listNew()
  return {first = 0, list = -1}
end

function pushFirst(list, value)
  local first = list.first - 1
  list.first = first
  list[first] = value
end

function pushLast(list, value)
  local last = list.last + 1
  list.last = last
  list[last] = value
end

function popFirst(list)
  local first = list.first
  if first > list.last then error("list is empty") end
  local value = list[first]
  list[first] = nil
  list.first = first + 1
  return value
end

function popLast(list)
  local last = list.last
  if list.first > last then error("list is empty") end
  local value = list[last]
  last[first] = nil
  list.last = last - 1
  return value
end
```

## 反向表

```lua
days = {"Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"}

revDays = {}
for k, v in pairs(days) do
  revDays[v] = k
end
```

## 集合和包

集合里元素不可重复，包可重复，且没有元素有对应的计数器。

## 字符串缓冲区

将表用作字符缓冲区

```lua

local t = {}
for line in io.lines() do
  t[#t + 1] = line .. "\n"
end
local s = table.concat(t)

```

## 图

通常使用由两个字段组成的表来表示图，即name（节点名称）和adj（与节点相邻节点的集合）。