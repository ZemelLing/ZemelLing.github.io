---
title: "Lua编码规范"
date: 2021-12-05T18:09:04+08:00
draft: false
tags: ["lua",]
categories: ["游戏开发",]
---

> 转载自 [lua代码规范-大于](https://zhuanlan.zhihu.com/p/26119366)

## 程序的版式

    Code is read much more often than it is written.
    Programming style is an art.

### 空行

#### 需加空行

* 函数之间都要加空行；
* 函数内部代码概念与逻辑之间，逻辑段落小节之间，都应该加空行；
* 注释行之前；

#### 不加空行

* 在一个函数体内，逻揖上密切相关的语句之间不加空行；
* 多行注释解释参数的时候，注释之间不加空行；

### 空格

#### 需加空格

* ”and“，”or“等关键字前后留一个空格，便于辨析；
* 逗号”,“后面要留一个空格；
* 赋值操作符、比较操作符、算术操作符如”=“、 “==“、”~=“、”>=“、”<=“、”>“、”<“、”+“、”-“、”*“、”/“、”%“、”“,等二元操作符的前后应当加空格；
* if、for、while等关键字之后如果要加左括号”(“，关键字与左括号之间应留一个空格，以突出关键字；

#### 不加空格

* 函数名之后不要留空格，紧跟左括号”(“；
* 左括号”(” 向后紧跟，紧跟处不留空格；
* 右括号”)“、逗号”,“、分号”;“，向前紧跟，紧跟处不留空格；
* 字符串连接符”..“前后不加空格；
* “:“，”.“，”[“，”]“这类操作符前后不加空格；

```lua
a > b and a or b           -- 良好的风格
a > b  and a or  b         -- 不良的风格

local a, b, c, max         -- 良好的风格
local a,b,c,max            -- 不良的风格

if a > b then              -- 良好的风格
    max = a 
end 
if a>b then max=a end      -- 不良的风格

data = dataTable[index]    -- 良好的风格
data = dataTable [ index ] -- 不良的风格

function(posX, posY)       -- 良好的风格
function (posX,posY)       -- 不良的风格
```

### 长行拆分

* 代码行最大长度宜控制在70至80个字符以内。代码行不要过长，否则眼睛看不过来，也不便于打印；
* 长表达式要在低优先级操作符处拆分成新行，操作符放在新行之首（以便突出操作符）；
```lua
-- 良好的风格
local newBuindingBtn = UI.newButton({             
    text = btnName,
    x = self.x,
    y = self.y,
    parent = self, 
    style = {
        normal = ResConfig.png.commonBtnBlue
    }
})
-- 不良的风格
local newBuindingBtn = UI.newButton({text = btnName,x = self.x,y = self.y,parent = self, style = {normal = ResConfig.png.commonBtnBlue}})

-- 良好的风格
if veryLongerVariable1 >= veryLongerVariable2
    and veryLongerVariable3 <= veryLongerVariable5
    and veryLongerVariable4 <= veryLongerVariable6 then
    doo()
end
-- 不良的风格
if veryLongerVariable1 >= veryLongerVariable2 and veryLongerVariable3 <= veryLongerVariable5 and veryLongerVariable4 <= veryLongerVariable6 then
        doo()
end
```

### 使用缩进

* 类中的成分；
* 方法体或语句块中的成分；
* 换行时的非起始行；

### 其它

* table的数据较多时考虑分行增加可读性

```lua
local a = {
    [1] = 8000,
    [2] = 8001,
    [3] = 8888
}
```

## 命名规则

    三思而命名。

### 共性规则

* 命名应当直观且可拼读，可望文知意； *标识符的长度应当符合“min-length && max-information”原则；
* 采用英文单词或单词组合，英文单词不要复杂，但用词需准确，切忌使用汉语拼音命名；
* 切勿为了避免命名过长而随意截取单词，以丢失可读性；
* 所有命名都不要与－x已有的命名风格冲突，例如不要以CC，UI开头；

### 文件命名

* 所有Lua文件的命名时使用大驼峰法；
* 根据文件的特性，一般以文件里的模块名或者类名作为同名文件名；
* 确定命名前，请检查下，不要跟其他文件同名；

```lua
    CCArmature.lua    -- 不良的风格
    UILayout.lua      -- 不良的风格
```

### 类的命名

* 所有类命名时使用大驼峰法，即首字母大写；

```
如触摸管理器类，命名为TouchManager
```

* 类名一般由”名词”或”多名词”组成，不要简写；

* 根据类的特性，加上相关的后缀或者前缀；

```
后缀：  
管理类  Manager
缓存类  Cache
控制类  Controller
模块    Module
网络类  Proxy
```

### 变量命名

> 小驼峰式命名法：第一个单字以小写字母开始；第二个单字的首字母大写，例如：firstName、lastName。

> 大驼峰式命名法：每一个单字的首字母都采用大写字母，例如：FirstName、LastName、CamelCase，也被称为Pascal命名法。

#### 通用规则

* 使用 “名词” 或是 “形容词+名词” 命名；
* 使用小驼峰法命名；
* 为了可读性，尽量避免变量名中出现标号，如value1， value2；
* 不要出现仅靠部分字母大小写区分的相似的变量；
* 除非是局部变量功能等价全局变量，不然局部变量不要与已有的全局变量同名；
* 尽量不要使用已有的类名作为变量名；
* 布尔值型变量，通常前缀加上is可以方便理解，比如isRemoved比Removed更加能表示这是一个布尔值变量；

```lua
        local data          -- 良好的风格  
        local oldData       -- 良好的风格 
        local newData       -- 良好的风格     
        local pairs = pairs -- 良好的风格

        local posx,posX     -- 不良的风格 
        local btn1,btn2     -- 不良的风格
        local TABLE = {}    -- 不良的风格
        local uILabel       -- 不良的风格
```

#### 类的成员变量

* 类的成员变量以”self.”开头，以区分于局部变量；

```lua
        function init()
            self.mainPanel = false -- 常用格式
            topPanel = false       -- 这样是全局变量，占用全局资源，而且难以区分于局部变量 
            ...
        end
```

#### 全局变量

* 全局变量使用双下划线(“_“)开头以及结尾，中间的命名以名词拼接，或”形容词＋名词”拼接，不同单词之间用（”“）隔开；

```lua
__VERSION_CODE__ = "1.0.0.0"
```

* 全局变量要有较详细的注释，包括对其功能、取值范围、存取时的注意事项等；

#### 局部变量

* M常用做模块里面表示模块本身；

```lua
module("MainGame.Module.IntegrationTest.MapModule", package.seeall)
local M = class(SceneView, "MapScene")

--数据的初始化
function M:init()
    ...
end

...
return M
```

* 引用进来的类或模块，用大驼峰法命名，引用路径统一带括号；

```lua
module(“MainGame.Module.IntegrationTest.MapModule”,package.seeall)
local M = class(SceneView,”MapScene”) 
local Surface = require(“xx.xx”) 
local TestButtonPanel = require(“xx”) 
```

#### 临时变量

* 常用下划线”_”作为可以忽略的变量，常使用在循环中；

```lua
for _,v in ipairs(t) do
    print(v)
end
```

* i,k,v,t常做临时变量，在表的循环中和函数参数列表中，i常表示ipairs下的数组下标，k常表示pairs下的键，v常表示对应的值，t则表示表；

```lua
for k,v in pairs(t)
    ...
end
for i,v in ipairs(t)
    ...
end
mt.__newindex = function(t, k, v)
    ...
end
```

#### 常量，事件名的命名

* 常量，事件名所用单词均大写，单词用下划线(‘_‘)分割；

```lua
-- 常量 默认宽度
LIST_DEFAULT_WIDTH = 100

-- 事件 添加到场景
ADDED_TO_STAGE = getId() 
```

#### 枚举的命名

* 枚举名命名，与类名命名一致；
* 枚举值命名，与常量，事件名的命名一致；

```lua
ControllerViewType = {
    SCENE = "SCENE",
    PANEL = "PANEL",
    POP =   "POP",
}
```

## 文件组织

### 文件描述

* 文件开头加上此文件的简要功能作用描述；

```lua
    -- MapModule.lua
    -- Author:xx
    -- 20xx年x月x日 xx:xx
    -- Using:创建地图 
```

### 文件中变量的定义

* 如果在文件中需要多次使用的某些导入文件，可以在文件开头用局部变量存储导入信息，而不是在每次使用的时候都重新导入一次；

```lua
    local Surface = require("xx.xx")
    local TestButtonPanel = require("xx")

    function M:xx()
        local testBtn = TestButtonPanel.newCC()
        ...
    end

    function M:yy()
        local panel = TestButtonPanel.newCC()
        local surface = Surface.newCC()
        ...
    end
```

## 类变量的定义

* 类中的成员变量需要在init中先声明，并赋予初始值，不允许不声明直接使用；

## 函数的定义规则

* 函数的行数过长(大于 100 行)时,尽量拆分为多个子函数；
* 函数中一些晦涩的部分,一定要加上注释；

## 注释的使用

* 短小的注释用--；
* 注释的位置应与被描述的代码相邻，可以在代码上方或右方，不可放在下方；
* 注释原则是有助于对程序的阅读理解，应该是说明为什么做而不仅仅是做什么；
* 注释也不宜太多，一般情况下源程序有效注释量在20%以上
* 建议注释符号后加空格

```lua
    return nil -- not found 良好的风格
    return nil --not found  不良的风格
```

* 在复杂的end结束的语句中，建议加对应注释
    
```lua
    for i, v in ipairs(t) do
        if type(v) == "string" then
            ...lots of code here...
        end -- if string
    end -- for each t
```

## 编码技巧

###  **在一切能使用local变量时使用local修饰，而非global变量（重要）；**

* 全局变量实际是放入全局表中，每次调用是用传入变量名作为key去获取，而local变量是直接通过lua的堆栈访问的；
* 推荐写法

    * 在能用局部变量解决的地方，不要使用全局变量，这点很容易被忽略；
    * 多次重复使用的全局接口，可以用局部变量保存下再使用；

```lua
        -- 比如需要多重遍历操作一个大表：
        -- 写法1:
        for k1,v1 in pairs(tbl) do
            for k2,v2 in pairs(v1) do
                ... 
            end
        end

        -- 写法2:
        do
            local pairs = pairs
            for k1,v1 in pairs(tbl) do
                for k2,v2 in pairs(v1) do
                    ... 
                end
            end
        end

        -- 由于pairs是一个全局变量应用的函数，所以写法2在这里有稍微效率上的提升，但要是单层遍历的没有这个效果了。
```

* 当作常量来多次使用的全局变量应该存为局部变量使用；

```lua
    local playerName = Cache.playerCache.username 
    ...
    local function = itemTemplate(data)            
        nameLabel:setText(playerName)

    local list = NewList.newCC(,{
        itemTemplate = itemTemplate,
        ... 
    })
```
### 临时变量的处理

* 字符串的连接 .. 由于字符串的管理机制，字符串在使用..连接时，会产生新的对象。由于lua在VM内对相同的string永远只保留一份唯一copy；

```lua
    local description ＝ ""
    for i = 1,20 do
        description = description.."xxx"
    end
    -- 这样会生成21份string的copy，但实际上我们只需要最后那一份 

    --如果是轻量级的简单连接还是可以使用的，因为影响不大，但要是大量的类似拼接，推荐使用string.format
```

* 类似于字符串的管理机制，表也存在类似的临时变量copy：

```lua
    -- 函数传参数
    function func({x,y})
        ...
    end
    -- 这种传参方式，每次都会生成一份copy，所以推荐以下的用法：
    function func(x,y)
        ...
    end

    function func({posX = x, posY = y})
        ...
    end
```

### 利用逻辑运算的短路效应

* and or 的返回值是表达式中的左值或者右值，可用来简化代码

```lua
    function foo(arg)
        arg = arg or "default"
        ...
    end

    -- 但要注意当赋值为bool值时候，容易出bug

    a = a or true  -- 错误的写法，当 a 明确写为 false 的时候，也会被改变成 true 。
    a = a ~= false -- 正确的写法，当 a 为 nil 的时候，被赋值为 true ；而 false 则不变。
```

* 另外，巧妙使用 and or 还可以实现类似 C 语言中的 ?: 三元操作：

```lua
    function max(a,b)
        return a > b and a or b
    end

    -- 这里相当于 return (a > b) ? a : b;
```

* 设法减少从C向lua传递字符串

> 字符串常量在 Lua VM 内部工作的非常快，但是一个从 C 向 lua vm 通过 lua_pushstring 之类的 api 传递进 VM 时，至少包含一个再 hash 和匹配的过程。

* 用#t == 0并不能判断表是否为空，因为#运算符会忽略所有不连续的数字下标和非数字下标

```lua
    if next(t) == nil then 
        -- 表为空
        -- ...
    end
    -- 因为表的键可能为false，所以必须与nil比较，而不直接使用~next(t)来判断表是否空。
```

* 更快的插入代码

```lua
    -- 更慢，不推荐
    table.insert(t, value) 

    -- 更快，推荐
    t[#t+1] = value
```

* 用do ... end可以来明确限定局部变量的作用域

```lua
    local v
    do
      local x = u2*v3-u3*v2
      local y = u3*v1-u1*v3
      local z = u1*v2-u2*v1
      v = {x,y,z}
    end -- x,y,z的作用域结束，被系统清理

    local count
    do
      local x = 0
      count = function() x = x + 1; return x end
    end -- x的作用域结束，被系统清理
```