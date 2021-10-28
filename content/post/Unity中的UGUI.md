---
title: "Unity中的UGUI"
date: 2021-10-27T19:26:14+08:00
draft: false
tags: ["unity", "ugui"]
categories: ["游戏开发",]
---

## 基本组件

### Text

需要使用TTF字体，RaycastTarget选项决定组件是否响应点击事件。可使用 Outline 和 Shadow 组件为文本描边和添加阴影。描边的效率低于阴影。

### Image

图片类型（Image Type）分为四种：
1. Simple 直接显示图片
2. Sliced 通过九宫格方式显示图片，可用 SpriteEditor 来编辑九宫格的区域
3. Tiled 平铺图片
4. Filled 像技能CD一样，可以旋转图片

### Raw Image

mage组件只能显示Sprite，Raw Image组件既可以显示任意Texture，也可以使用Sprite，不过它还是以Texture方式显示的。Image组件使用Sprite时，可以使用Atlas来合并批次，但是Raw Image组件却不能，每个Raw Image就占一次DrawCall。和Render Texture 配合可将相机内容渲染到Raw Image上。

### Button

Button 存在普通、点击、抬起和悬浮这几种状态，切换各状态可以改变按钮颜色，也可以更改Sprite图片，再或者使用动画控制。