---
title: 实现移动端Retina屏幕1px边框
excerpt: false
thumbnail: 'https://jiangwen-blog.oss-cn-fuzhou.aliyuncs.com/images/dd3e880811ebb6e017c2d2eca2.webp'
tags: [移动端适配, H5]
categories: [CSS]
date: 2021-11-18 11:25:00
---

> 前言：在Reina（视网膜）屏幕的手机上，使用CSS设置的1px的边框实际会比视觉稿粗很多，因为不同的手机有不同的像素密度，因此在css中的1px并不等于移动设备的1px。在window对象中有一个`devicePixelRatio`属性（物理像素 / 设备独立像素），他可以反应css中的像素与设备的像素比。

## 使用background-image/border-image实现

你要先准备一张符合你要求的图片。然后将边框模拟在背景上。

```css
/* background-image方案 */
.border-bottom-1px {
  border-width: 0 0 1px 0;
  -webkit-border-image: url(linenew.png) 0 0 2 0 stretch;
  border-image: url(linenew.png) 0 0 2 0 stretch;
}

/* border-image方案 */
.background-image-1px {
  background: url(../img/line.png) repeat-x left bottom;
  -webkit-background-size: 100% 1px;
  background-size: 100% 1px;
}
```

**优点：**

- 可以设置单条,多条边框

- 没有性能瓶颈的问题

**缺点：**

- 修改颜色麻烦, 需要替换图片

- 圆角需要特殊处理，并且边缘会模糊

## 多背景渐变实现

与background-image方案类似，只是将图片替换为css3渐变。设置1px的渐变背景，50%有颜色，50%透明。

```css
.background-gradient-1px {
  background:
    linear-gradient(#000, #000 100%, transparent 100%) left / 1px 100% no-repeat,
    linear-gradient(#000, #000 100%, transparent 100%) right / 1px 100% no-repeat,
    linear-gradient(#000,#000 100%, transparent 100%) top / 100% 1px no-repeat,
    linear-gradient(#000,#000 100%, transparent 100%) bottom / 100% 1px no-repeat
}
```

**优点：**

- 可以实现单条、多条边框

- 边框的颜色随意设置

**缺点：**

- 代码量不少

- 圆角没法实现

- 多背景图片有兼容性问题

## 使用box-shadow模拟边框

利用css 对阴影处理的方式实现0.5px的效果

```css
.box-shadow-1px {
  box-shadow: inset 0px -1px 1px -1px #c8c7cc;
}
```

**优点：**

- 代码量少

- 所有场景都能满足

**缺点：**

- 边框有阴影，颜色变浅

## viewport + rem 实现

同时通过设置对应viewport的rem基准值，这种方式就可以像以前一样轻松愉快的写1px了。

```html
 <!-- 在devicePixelRatio = 2 时，输出viewport：-->
<meta name="viewport" content="initial-scale=0.5, maximum-scale=0.5, minimum-scale=0.5, user-scalable=no">

 <!-- 在devicePixelRatio = 3 时，输出viewport：-->
<meta name="viewport" content="initial-scale=0.33, maximum-scale=0.33, minimum-scale=0.33, user-scalable=no">
```

这种兼容方案相对比较完美，**适合新的项目**，老的项目修改成本过大。可以看看《使用Flexible实现手淘H5页面的终端适配》

**优点：**

- 所有场景都能满足

- 一套代码，可以兼容基本所有布局

**缺点：**

- 老项目修改代价过大，只适用于新项目

## 伪类 + transform 实现（推荐）

对于老项目，有没有什么办法能兼容1px的尴尬问题了，个人认为伪类+transform是比较完美的方法了。
原理是把原先元素的 border 去掉，然后利用 :before 或者 :after 重做 border ，并 transform 的 scale 缩小一半，原先的元素相对定位，新做的 border 绝对定位。

**单条border样式设置：**

```css
.scale-1px{

  position: relative;

  border:none;

}

.scale-1px:after{

  content: '';

  position: absolute;

  bottom: 0;

  background: #000;

  width: 100%;

  height: 1px;

  -webkit-transform: scaleY(0.5);

  transform: scaleY(0.5);

  -webkit-transform-origin: 0 0;

  transform-origin: 0 0;

}
```

**四条boder样式设置:**

```css
.scale-1px{

  position: relative;

  margin-bottom: 20px;

  border:none;

}

.scale-1px:after{

  content: '';

  position: absolute;

  top: 0;

  left: 0;

  border: 1px solid #000;

  -webkit-box-sizing: border-box;

  box-sizing: border-box;

  width: 200%;

  height: 200%;

  -webkit-transform: scale(0.5);

  transform: scale(0.5);

  -webkit-transform-origin: left top;

  transform-origin: left top;

}
```

最好在使用前也判断一下，结合 JS 代码，判断是否 Retina 屏：

```js
if(window.devicePixelRatio && devicePixelRatio >= 2){
 document.querySelector('ul').className = 'scale-1px';
}
```

**优点：**

- 所有场景都能满足

- 支持圆角(伪类和本体类都需要加border-radius)

**缺点：**

- 对于已经使用伪类的元素(例如clearfix)，可能需要多层嵌套



🔗: [参考链接](https://mp.weixin.qq.com/s?__biz=MjM5MDA2MTI1MA==&amp;mid=2649088476&amp;idx=3&amp;sn=44893ca9980310c02a8b1b63f2145fd5&amp;chksm=be5bc671892c4f6791cd6a60dbcd72918c682cf5e5cf36baf421e9829d7f48014f92cc50fc2a&amp;scene=27 )

