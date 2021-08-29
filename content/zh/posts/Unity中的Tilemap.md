---
title: "Unity中的Tilemap"
date: 2021-05-18T15:06:16+08:00
draft: false
tags: ["unity", "tilemap", "2d"]
categories: ["unity",]
---

## 用法

1. 导入图片资源，并将其 Texture Type 设置为 (2D Sprite and UI)
2. 创建 Tilemap 
3. 打开 TilePalette 窗口，创建 Palette
4. 

## Grid

Grid 对象用于对齐瓦片等对象。

## TileMap

## Tile

### RuleTile

RuleTile的Inspection界面存在一个九宫格用于配置Tile的绘制规则。中心表示当前的Tile，其它八个方向表示相邻的其它Tile。

|选项|图标|描述|
|-|-|-|
|Don't Care|空白|Rule Tile 会忽略这个位置的内容|
|This|方向箭头|Rule Tile 会检查这个位置的内容，如果是该 Rule Tile 的实例，则通过检查，反之，未通过检查。|
|Not This|红叉|与 This 相反，也就是这个位置的内容不能够是 Rule Tile 的实例。|

Rule Tile 是按 Rule 的排列顺序依次检查规则。


