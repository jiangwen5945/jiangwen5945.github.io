---
title: 块级格式化上下文(BFC)
excerpt: false
thumbnail: 'https://jiangwen-blog.oss-cn-fuzhou.aliyuncs.com/images/cover/6.webp'
tags: [css属性继承]
categories: [CSS]
date: 2020-02-28 12:15:00
---

## BFC是什么？

> 块级格式化上下文（Block Formatting Context，BFC）是一个独立的渲染区域，只有Block-level box参与。



## BFC 的特征：

1. 内部的box会在垂直方向顺序排列

   > 个人备注:display:black不会触发新的bfc,属于同一个bfc环境中,所以满足该特征

2. box间垂直方向的距离由margin决定，属于同一个BFC的box的margin会发生重叠

   > 个人备注:要想不发生margin重叠,可以使该元素脱离同一个bfc环境,即:可以通过包裹一层使其脱离

3. 每个元素的margin box的左边， 与包含块border box的左边相接触。即使存在浮动也是如此

   > 即:从左往右排列,可以想象成'回'字,里面的口margin box,与外面的口border box 左侧相邻

4. BFC的区域内所有元素不会与浮动 (float) 元素重叠

   > 使一个元素生成bfc就可以避免该元素与浮动元素的重叠

5. BFC就是页面上的一个隔离的独立容器，容器里面的元素与外面的元素互不影响

6. 计算BFC的高度时，浮动元素也参与计算

   > 解决父元素塌陷的情况



## 如何触发 BFC？

**以下任意一条即可**：

1. 根元素
2. float 的值不为none。
3. position 的值不为static或者relative。
4. display的值为 table-cell, table-caption, inline-block, flex, 或者 inline-flex中的其中一个。
5. overflow的值不为visible。



## BFC 的应用场景：

1. ###   解决 `margin` 叠加问题

   **产生叠加的条件**：

   - 处于常规文档流（非float和绝对定位）的块级盒子,并且处于同一个 BFC 

   - 没有inline盒子，没有空隙，没有 padding 和 border 将他们分隔开

   

   **解决方案:**

   - 将其中一个元素形成BFC区域，使其与其他元素互不影响
   - 通过给元素加边框或者边距来解决

   

2. ### 用于布局

   - 三列布局（圣杯布局、双飞燕布局）

3. ### 用于清除浮动，计算BFC高度

   **解决方案：**

   - 利用 `clear` 属性清除浮动。
   - overflow:hidden使父容器形成BFC



> [参考链接：细说CSS中的BFC](https://juejin.im/post/583bb606a22b9d006c141286)

