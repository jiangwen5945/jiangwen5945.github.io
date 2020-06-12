---
title: CSS居中总结
tags: CSS布局
categories: CSS
abbrlink: 14596
date: 2018-08-28 11:34:17
---
## 1. 水平居中

### 1.1 内联元素水平居中
利用`text-align: center`可以实现在块级元素内部的内联元素水平居中。此方法对内联元素`(inline)`, 内联块`(inline-block)`,内联表`(inline-table)`,`(inline-flex)`元素水平居中都有效。

**核心代码：**

```css
.center-text {
    text-align: center;
 }
```



### 1.2 块级元素水平居中
通过把固定宽度块级元素的`margin-left`和`margin-right`设成auto，就可以使块级元素水平居中。

**核心代码：**
```css
.center-block {
  margin: 0 auto;
}
```

### 1.3 多块级元素水平居中
#### 1.3.1 利用`inline-block `
如果一行中有两个或两个以上的块级元素，通过设置块级元素的显示类型`为inline-block`和父容器的`text-align`属性从而使多块级元素水平居中。

**核心代码：**

```css
.container {
    text-align: center;
}
.inline-block {
    display: inline-block;
}
```
#### 1.3.2 利用`display: flex`
利用弹性布局`(flex)`，实现水平居中，其中`justify-content`用于设置弹性盒子元素在主轴（横轴）方向上的对齐方式，本例中设置子元素水平居中显示。

**核心代码：**
```css
.flex-center {
    display: flex;
    justify-content: center;
}
```


## 2. 垂直居中
### 2.1 单行内联`(inline-)`元素垂直居中
通过设置内联元素的高度`(height)`和行高`(line-height)`相等，从而使元素垂直居中。

**核心代码：**
```css
#v-box {
    height: 120px;
    line-height: 120px;
}

```
### 2.2 多行内联元素垂直居中
#### 2.2.1 利用表布局（table）
利用表布局的`vertical-align: middle`可以实现子元素的垂直居中。

**核心代码：**


```css
.center-table {
    display: table;
}
.v-cell {
    display: table-cell;
    vertical-align: middle;
}
```
#### 2.2.2 利用flex布局（flex）
利用`flex`布局实现垂直居中，其中`flex-direction:column`定义主轴方向为纵向。因为flex布局是CSS3中定义，在较老的浏览器存在兼容性问题。

**核心代码：**

```css
.center-flex {
    display: flex;
    flex-direction: column;
    justify-content: center;
}
```
#### 2.2.3 利用“精灵元素”
利用“精灵元素”`(ghostelement)`技术实现垂直居中，即在父容器内放一个100%高度的伪元素，让**文本和伪元素垂直对齐**，从而达到垂直居中的目的。

**核心代码：**

```css
.ghost-center {
    position: relative;
}
.ghost-center::before {
    content: " ";
    display: inline-block;
    height: 100%;
    width: 1%;
    vertical-align: middle;
}
.ghost-center p {
    display: inline-block;
    vertical-align: middle;
    width: 20rem;
}
```
### 2.3 块级元素垂直居中
#### 2.3.1 固定高度的块级元素
我们知道居中元素的高度和宽度，垂直居中问题就很简单。通过绝对定位元素距离顶部50%，并设置`margin-top`向上偏移元素高度的一半，就可以实现垂直居中了。

**核心代码：**

```css
.parent {
  position: relative;
}
.child {
  position: absolute;
  top: 50%;
  height: 100px;
  margin-top: -50px; 
}
```

#### 2.3.2 未知高度的块级元素
当垂直居中的元素的高度和宽度未知时，我们可以借助CSS3中的`transform`属性向Y轴反向偏移50%的方法实现垂直居中。但是部分浏览器存在兼容性的问题。

**核心代码：**

```css
.parent {
    position: relative;
}
.child {
    position: absolute;
    top: 50%;
    transform: translateY(-50%);
}
```
## 3. 水平垂直居中
### 3.1 固定宽高元素水平垂直居中
通过`margin`平移元素整体宽度的一半，使元素水平垂直居中。

**核心代码：**
```css
.parent {
    position: relative;
}
.child {
    width: 300px;
    height: 100px;
    position: absolute;
    top: 50%;
    left: 50%;
    margin: -50px 0 0 -150px;
}
```
### 3.2 未知宽高元素水平垂直居中
#### 3.2.1 利用2D变换
在水平和垂直两个方向都向反向平移宽高的一半，从而使元素水平垂直居中。

**核心代码：**

```css
.parent {
    position: relative;
}
.child {
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
}
```
#### 3.2.2 利用定位属性

将子元素设置为脱离文档流的position定位属性`fixed`或`absolute`,并使其位置属性`(top、bottom、left、right)`全部设置为0，外边距`margin`设置为auto，即可实现水平垂直居中。

```css
.parent {
    position: relative;
}
.child{
    position: absolute;
    margin: auto;
    left: 0;
    right: 0;
    top: 0;
    bottom: 0;
}
```

**核心代码：**

### 3.3 flex布局实现水平垂直居中
利用flex布局，其中`justify-content`用于设置或检索弹性盒子元素在主轴（横轴）方向上的对齐方式；而`align-items`属性定义flex子项在flex容器的当前行的侧轴（纵轴）方向上的对齐方式。

**核心代码：**

```css
.parent {
    display: flex;
    justify-content: center;
    align-items: center;
}
```

父元素display设置成flex，子元素设置属性为margin:auto实现水平垂直居中

**核心代码：**

```css
.parent {
    display: flex;
}
.child{
    margin:auto;
}
```

### 3.4 grid布局实现水平垂直居中
利用grid实现水平垂直居中，兼容性较差，不推荐。

**核心代码：**

```css
.parent {
  height: 140px;
  display: grid;
}
.child { 
  margin: auto;
}
```
### 3.5 table布局实现水平垂直居中
屏幕上水平垂直居中十分常用，常规的登录及注册页面都需要用到。要保证较好的兼容性，还需要用到表布局。

**核心代码：**

```css
.outer {
    display: table;
    position: absolute;
    height: 100%;
    width: 100%;
}

.middle {
    display: table-cell;
    vertical-align: middle;
}

.inner {
    margin-left: auto;
    margin-right: auto; 
    width: 400px;
}
```

## 4. 引用参考
[https://www.zcfy.cc/article/centering-in-css-a-complete-guide-css-tricks]
