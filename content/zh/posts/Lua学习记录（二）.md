---
title: "Lua学习记录（二）"
date: 2021-08-28T10:53:56+08:00
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

# 模式匹配

## 函数

string.find 返回匹配的开始和结束索引
string.match 返回匹配的字符串
string.gsub  返回替换后的字符串和发生替换的次数
string.gmatch 返回函数，此函数可遍历一个字符串中所匹配的所有字串

## 模式

Lua 中模式使用百分号做转义字符。

|字符分类|含义|
|-|-|
|.|任意字符|
|%a|任意字母|
|%c|控制字符|
|%d|数字|
|%g|除空格外的可打印字符|
|%l|小写字母|
|%p|标点符号|
|%s|空白字符|
|%u|大写字母|
|%w|字母和数字|
|%x|十六进制数字|

字符分类的大写形式表示对应的补集

使用方括号可创建字符集，例如：`[%w_]` 表示所有字母数字以及下划线。

|修饰符|含义|
|-|-|
|+|重复一次或多次|
|*|重复零次或多次|
|-|重复零次或多次（最小匹配）|
|?|可选（零次或一次）|

`%b` 用于匹配成对的字符串，写法是 `'%bxy'` x和y是任意不同字符
`%f[char-set]` 前置模式，表示这个模式的前一个字符不在 char-set（字符集）内且，后一个字符在 char-set 内时，返回空字符串。

## 捕获

在进行匹配时可以在模式中将想要捕获的内容使用圆括号包围。

函数 string.match 在匹配具有捕获的模式时，会将匹配的捕获返回。

```lua
string.match("name = Anna", "(%a+)%s*=%s*(%a+)") --> name    Anna  具有捕获
string.match("name = Anna", "%a+%s*=%s*%a+")  --> name = Anna 没有指定捕获
```

`%n` 其中 n 为数字，表示匹配第n个捕获的副本。其中 `%0` 表示整个匹配。

```lua
s = [[then he said: "it's all right"!]]
q1, quotedPart, q2 = string.match(s, "([\"'])(.-)(%1)")
print(quotedPart) --> it's all right
print(q1) --> "
print(q2) --> "
```

捕获也可用用于替换

```lua
print((string.gsub("hello Lua!", "%a", "%0-%0")))
--> h-he-el-ll-lo-o L-Lu-ua-a!

print((string.gsub("hello Lua!", "(.)(.)", "%2%1")))
--> ehll ouL!a
```

# 时间和日期

## 表示方式

1. 数值

自 Jan 01, 1970, 0:00 UTC 起的秒数

2. 表

包含字段：year, month, day, hour, min, sec, wday, yday, isdst
其中 wday 表示一周中的第几天，yday表示一年中的第几天，isdist表示是否使用夏时令。


## 函数

### os.time

```lua
os.time() --> 1630150137
os.time({year=2021, month=8, day=28, hour=19, min=32, sec=23}) --> 1630150343
 os.time({year=1970, month=1, day=1})  --> 14400
```

### os.date

os.date 像是 os.time 的反函数

os.date的指示符
|指示符|含义|
|-|-|
|%a|星期几的缩写|
|%A|星期几的全名|
|%b|月份的缩写|
|%B|月份的全名|
|%c|时间和日期|
|%d|一个月中的第几天|
|%H|24小时制中的小时数|
|%I|12小时制中的小时数|
|%j|一年中的第几天|
|%m|月份|
|%M|分钟|
|%p|am or pm|
|%S|秒|
|%w|星期|
|%W|一年中的第几周|
|%x|日期|
|%X|时间|
|%y|两位数的年份|
|%Y|完整的年份|
|%z|时区|
|%%|百分号|
|*t|返回时间的Table|
