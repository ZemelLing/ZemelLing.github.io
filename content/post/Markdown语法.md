---
title: "Markdown语法"
date: 2021-03-26T10:16:51+08:00
draft: false
tags: ["markdown", ]
categories: ["其他方面",]
series:
description:
toc: true
authors:
  - zemelling
lastmod: 2021-08-29T13:09:56+08:00
featuredImage:
---

## 标题（Headings）

要创建标题，请在单词或短语前面添加井号 (#) 。井号的数量代表了标题的级别。例如，添加三个井号即创建一个三级标题
```
### My Header
```
### My Header
------------
## 段落

在每行的的末尾空格两下并回车或是直接在两行间间隔一行空白行，这样就可以形成换行效果。

## 图片

### 本地图片

```
![图片描述](/iii.png)
```

### 网络图片

```
![图片描述](http://baidu.com/pic/doge.png)
```

### 以Base64编码插入图片

1. 将图片转为Base64字符串
2. 插入图片
```
![avatar][base64str]
[base64str]:data:image/png;base64,iVBORw0......
```

## 列表

### 有序列表

```
1. 第一项
2. 第二项
```

1. 第一项
2. 第二项

### 无序列表

```
* 第一项
    * 第一项第一项
* 第二项
```

* 第一项
    * 第一项第一项
* 第二项

子项前空 4 格

## 加粗与斜体

```
*斜体*
**加粗**
***斜体与加粗***

*与内容间无空白字符
```

*斜体*  
**加粗**  
***斜体与加粗***  