---
title: CSS中浮动和定位概念区分 
excerpt: false
thumbnail: 'https://jiangwen-blog.oss-cn-fuzhou.aliyuncs.com/images/cover/4.webp'
tags: [css]
categories: [CSS]
date: 2020-02-28 12:15:00
---
## 首先明确元素进行布局时的两个状态：

​	脱离文档流：该元素不进行占位，其他元素在其下方排列。

​	脱离文本流：文字是否会在其周围进行环绕布局

|          | 脱离文档流 | 脱离文本流 |
| -------- | ---------- | ---------- |
| float    | 是√        | 否×        |
| absolute | 是√        | 是√        |
| relative | 是√        | 是√        |
| fixed    | 否×        | 是√        |



对父元素overflow: hidden进行清除浮动，在脱离文档流的方式中只对float有效，其他两种`absolute`、`fixed`无效，即无法撑开父级高度

