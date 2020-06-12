---
title: CSS布局方案
abbrlink: 58574
date: 2018-11-02 11:14:10
tags: CSS布局
categories: CSS
---

## 1.左右布局
### 1.1 float + margin
```css
.left{
    float: left;
    width: 100px; // 仅适用于左侧定宽的情况
}
.right{
    margin-left: 100px;
}
```

### 1.2 float + overflow

``` css
.left{
    float: left;
    width: 100px; 
}
.right{
    overflow: hidden;
}
```
> tips：该方案中的right就是利用BFC中的【3】【4】规则形成了BFC空间与浮动元素left形成左右布局

BFC(块级格式化上下文)定义了如下布局规则：
1. 内部的块元素会在垂直方向，一个接一个地放置。
2. 块元素垂直方向的距离由margin决定,两个相邻块元素的垂直方向的margin会发生重叠。
3. 每个元素的左外边距，与包含块的左边相接触(对于从左往右的格式化，否则相反)。即使存在浮动也是如此。
4. BFC的区域不会与float元素的区域重叠。
5. BFC就是页面上的一个隔离的独立容器，容器里面的子元素不会影响到外面的元素。反之也如此。
6. 计算BFC的高度时，浮动元素也参与计算


### 1.3 table

```css
.parent{
    display: table;
    width: 100%;
    table-layout: fixed; //列宽由表格宽度和列宽度设定
}
.left{
    width: 100px;
    display: table-cell;
}
.right{
    display: table-cell;
}

```
### 1.4 flex

```css
.parent {
    display: flex;
}
.left {
    width: 100px;
}
.right {
    flex: 1; // flex: 1 1 0 的缩写,表示不随内容拉伸
}

```
> tips: flex属性是flex-grow,flex-shrink和flex-basis的简写,默认值为 0 1 auto


## 2. 等宽等高布局
### 2.1 table

```css
.container{
    width:100%;
    display: table;
    table-layout: fixed;
}
.column{
    display: table-cell;
}
```

### 2.2 flex 
```css
.container{
    display: flex;
}
.column{
    flex: 1;
}
```

> 补充：多行多列并排等分
```css
.container {
    display: flex;
    flex-flow: row wrap; // flex-direction和flex-wrap的简写形式，默认值为row nowrap
    justify-content: space-between;
}
.item {
    width:100px
}
```

### 2.3 内外补丁负值法
```css
.container{
    overflow: hidden; //隐藏元素padding-bottom: 9999px的高度部分
}

.left,.right{
    padding-bottom: 9999px; // padding可以撑开外层标签，增大盒模型的高度
    margin-bottom: -9999px; // margin不用来撑开外层标签，抵消盒模型的高度（抵消padding-bottom的占位高度区域）
}
.left{
    float: left; 
}
.right{
     overflow: hidden; // 形成BFC空间避免right内部元素受left浮动元素的影响
}

```
> tips: 父容器`overflow: hidden`此处的作用为：
1.隐藏padding-bottom撑开的不占位但可见的高度。
2就是形成BFC的区域,使得BFC区域中的left元素不会与float元素的区域重叠，同时浮动元素也参与BFC区域高度计算。

> 注：`内外补丁负值法`撑开的区域9999px可显示但没有实际占位，作用仅仅是填充显示较短元素的空白区域


## 3.经典布局
### 3.1圣杯布局

``` html
<div class="container">
    <div class="header">header</div>
    <div class="wrapper clearfix">
        <!-- 主体内容先于左右两侧加载渲染 -->
        <div class="main col">main</div> 
        <div class="left col">left</div>
        <div class="right col">right</div>
    </div>
    <div class="footer">footer</div>
</div>
```

``` css
.container {width: 500px; margin: 50px auto;}
.wrapper {padding: 0 100px 0 100px;} //让左右两侧区域包含在里面
.col {position: relative; float: left;}//设置元素浮动，且相对自身进行位移
.header,.footer {height: 50px;}
.main {width: 100%;height: 200px;}
.left {width: 100px; height: 200px; margin-left: -100%;left: -100px;} // 向上移自身高度，相对自身左移100px
.right {width: 100px; height: 200px; margin-left: -100px; right: -100px;} //向左移100px，相对自身右移100px
.clearfix::after {content: ""; display: block; clear: both; visibility: hidden; height: 0; overflow: hidden;}

```

> tips: 使用了 relative 相对定位、float（需要请浮动，此处使用 overflow:hidden; 方法）和 负值 margin ，将 left 和 right 部分「安装」到 wrapper 的两侧，顾名「圣杯」。

### 3.2双飞燕布局
```html
<div class="container">
    <div class="header">header</div>
    <div class="wrapper clearfix">
        <div class="main col">
            <div class="main-wrap">main</div>
        </div>
        <div class="left col">left</div>
        <div class="right col">right</div>
    </div>
    <div class="footer">footer</div>
</div>
```

```css
.col {float: left;}
.header {height: 50px;}
.main {width: 100%;}
.main-wrap {margin: 0 100px 0 100px;height: 200px;}
.left {width: 100px; height: 200px; margin-left: -100%;}
.right {width: 100px; height: 200px; margin-left: -100px;}
.footer {height: 50px;}
.clearfix::after {content: ""; display: block; clear: both; visibility: hidden; height: 0; overflow: hidden;}
```
>tips: 同样使用了 float 和 负值 margin,不同的是，并没有使用 relative 相对定位 而是增加了 dom 结构，增加了一个层级。

### 3.3定位布局
```html
<div class="header">header</div>
<div class="wrapper">
    <div class="main col">
        main
    </div>
    <div class="left col">
        left
    </div>
    <div class="right col">
        right
    </div>
</div>
<div class="footer">footer</div>
```

```css
.wrapper { position: relative; }
.main { margin:0 100px;}
.left { position: absolute; left: 0; top: 0;}
.right { position: absolute; right: 0; top: 0;}
```

> 思考：简单的绝对定位即可解决问题，为啥还要搞什么圣杯和双飞翼？[原因](https://link.juejin.im/?target=https%3A%2F%2Fgithub.com%2Fzwwill%2Fblog%2Fissues%2F11)

## 引用参考
> [「前端那些事儿」]('https://www.zcfy.cc/article/centering-in-css-a-complete-guide-css-tricks' style='text-decoration: none')

