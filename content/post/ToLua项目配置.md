---
title: "ToLua项目配置"
date: 2021-10-31T09:39:02+08:00
draft: false
tags: ["lua", "tolua"]
categories: ["游戏开发",]
---

## 

1. 将 ToLua 下的 Assets 和 Luac53 文件夹复制到自己的工程
2. 前往 https://github.com/topameng/tolua_runtime
3. 将Plugins53文件夹里面的所有tolua相关的runtime底层库，都拷贝覆盖到unity工程的Plugins目录下。
4. 打开unity编辑器，添加“**LUAC_5_3**”宏，回车等待编辑器编译完毕既是Lua5.3的虚拟机环境。
***注意要用lua5.3宏定义“LUAC_5_3”必须一直有效！！如果用luajit或lua5.1版本的，宏定义一定要删除！！！！***
5. 如果更改了文件夹结构，则需要将 CustomSettings.cs 中的相关路径也做调整，还有 LuaConst 的路径也要更改