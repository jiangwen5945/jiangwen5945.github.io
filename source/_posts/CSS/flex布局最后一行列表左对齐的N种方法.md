---
title: flex布局最后一行列表左对齐的N种方法
excerpt: false
thumbnail: 'https://jiangwen-blog.oss-cn-fuzhou.aliyuncs.com/images/cover/4.webp'
tags: [flex]
categories: [CSS]
date: 2023-01-18 14:25:00
---


> 在CSS flex布局中，`justify-content`属性可以控制列表的水平对齐方式，例如`space-between`值可以实现两端对齐。但是，如果最后一行的列表的个数不满，则就会出现最后一行没有完全垂直对齐的问题。

如下代码：

```css
.container {    
    display: flex;    
    justify-content: space-between;
    flex-wrap: wrap;
}
.list { 
    width: 24%; height: 100px;    
    background-color: skyblue;    
    margin-top: 15px;
 }
```

然后列表的个数不多不少正好7个：

```html
<div class="container">
    <div class="list"></div>    
    <div class="list"></div>    
    <div class="list"></div>    
    <div class="list"></div>    
    <div class="list"></div>    
    <div class="list"></div>   
    <div class="list"></div>
</div>
```

此时最后一行的小方块的排列就显得很尴尬了：

![图片](https://jiangwen-markdown-img.oss-cn-fuzhou.aliyuncs.com/CSS/flex%E5%B8%83%E5%B1%80%E6%9C%80%E5%90%8E%E4%B8%80%E8%A1%8C%E5%88%97%E8%A1%A8%E5%B7%A6%E5%AF%B9%E9%BD%90%E7%9A%84N%E7%A7%8D%E6%96%B9%E6%B3%95.assets/640-20230626005711005.png)

您可以狠狠地点击这里：最后一行flex列表没有对齐demo

此时，最后一行应该左对齐排列才是我们想要的效果，如何实现呢？

其实实现的思路和display:inline-block的两端对齐是一样的。

### **二、如果每一行列数是固定的**

如果每一行列数是固定的，则下面两种方法可以实现最后一行左对齐。

#### 方法一：模拟space-between和间隙

也就是我们不使用`justify-content:space-between`声明在模拟两端对齐效果。中间的gap间隙我们使用margin进行控制。

例如：

```css
.container {    
    display: flex;    
    flex-wrap: wrap;
}
.list {    
    width: 24%; height: 100px;    
    background-color: skyblue;    
    margin-top: 15px;
}
.list:not(:nth-child(4n)) {    
    margin-right: calc(4% / 3);
}
```

此时，布局效果是这样的：

![图片](https://jiangwen-markdown-img.oss-cn-fuzhou.aliyuncs.com/CSS/flex%E5%B8%83%E5%B1%80%E6%9C%80%E5%90%8E%E4%B8%80%E8%A1%8C%E5%88%97%E8%A1%A8%E5%B7%A6%E5%AF%B9%E9%BD%90%E7%9A%84N%E7%A7%8D%E6%96%B9%E6%B3%95.assets/640-20230626005710826.png)

#### **方法二：****根据个数最后一个元素动态margin**

由于每一列的数目都是固定的，因此，我们可以计算出不同个数列表应当多大的`margin`值才能保证完全左对齐。

例如，假设每行4个元素，结果最后一行只有3个元素，则最后一个元素的`margin-right`大小是“列表宽度+间隙大小”的话，那最后3个元素也是可以完美左对齐的。

然后，借助树结构伪类数量匹配技术（这篇文章“伪类匹配列表数目实现微信群头像CSS布局的技巧”中的布局技巧就是借助这种技术实现），我们可以知道最后一行有几个元素。

例如：

- `.list:last-child:nth-child(4n - 1)`说明最后一行，要么3个元素，要么7个元素……
- `.list:last-child:nth-child(4n - 2)`说明最后一行，要么2个元素，要么6个元素……

在本例中，一行就4个元素，因此，我们可以有如下CSS设置：

```css
.container {    
    display: flex;    /* 两端对齐 */    
    justify-content: space-between;    
    flex-wrap: wrap;
}
.list {    
    width: 24%; 
    height: 100px;    
    background-color: skyblue;    
    margin-top: 15px;
}
/* 如果最后一行是3个元素 */
.list:last-child:nth-child(4n - 1) {    
    margin-right: calc(24% + 4% / 3);}
/* 如果最后一行是2个元素 */.list:last-child:nth-child(4n - 2) {    
    margin-right: calc(48% + 8% / 3);
}

```

效果如下GIF示意，删除列表后，布局依然稳稳地左对齐。

![图片](https://jiangwen-markdown-img.oss-cn-fuzhou.aliyuncs.com/CSS/flex%E5%B8%83%E5%B1%80%E6%9C%80%E5%90%8E%E4%B8%80%E8%A1%8C%E5%88%97%E8%A1%A8%E5%B7%A6%E5%AF%B9%E9%BD%90%E7%9A%84N%E7%A7%8D%E6%96%B9%E6%B3%95.assets/640-20230626005709403.gif)

眼见为实，您可以狠狠地点击这里：动态匹配数量实现flex子项左对齐demo

### **三、如果每一子项宽度不固定**

有时候，每一个flex子项的宽度都是不固定的，这个时候希望最后一行左对齐该如何实现呢？

由于此时间隙的大小不固定，对齐不严格，因此，我们可以直接让最后一行左对齐即可。具体方法有两个：

#### **方法一：****最后一项margin-right:auto**

CSS代码如下：

```css
.container {    
    display: flex;    
    justify-content: space-between;    
    flex-wrap: wrap;
}
.list {   
    background-color: skyblue;    
    margin: 10px;
}
/* 最后一项margin-right:auto */
.list:last-child {    
    margin-right: auto;
}
```

最终效果如下GIF：

![图片](https://jiangwen-markdown-img.oss-cn-fuzhou.aliyuncs.com/CSS/flex%E5%B8%83%E5%B1%80%E6%9C%80%E5%90%8E%E4%B8%80%E8%A1%8C%E5%88%97%E8%A1%A8%E5%B7%A6%E5%AF%B9%E9%BD%90%E7%9A%84N%E7%A7%8D%E6%96%B9%E6%B3%95.assets/640-20230626005709618.gif)

#### **方法二：****创建伪元素并设置flex:auto或flex:1**

CSS代码如下：

```css
.container {    
    display: flex;    
    justify-content: space-between;    
    flex-wrap: wrap;
}
.list {    
    background-color: skyblue;  
    margin: 10px;
}
/* 使用伪元素辅助左对齐 */
.container::after {    
    content: '';
    flex: auto;    /* 或者flex: 1 */
}
```

最终效果如下GIF：

![图片](https://jiangwen-markdown-img.oss-cn-fuzhou.aliyuncs.com/CSS/flex%E5%B8%83%E5%B1%80%E6%9C%80%E5%90%8E%E4%B8%80%E8%A1%8C%E5%88%97%E8%A1%A8%E5%B7%A6%E5%AF%B9%E9%BD%90%E7%9A%84N%E7%A7%8D%E6%96%B9%E6%B3%95.assets/640-20230626005709618.gif)

------

这两个方法我合在一个demo页面了，您可以狠狠的点击这里：flex子元素宽度不固定最后一行左对齐demo

### **四、如果每一行列数不固定**

如果每一行的列数不固定，则上面的这些方法均不适用，需要使用其他技巧来实现最后一行左对齐。

这个方法其实很简单，也很好理解，就是使用足够的空白标签进行填充占位，具体的占位数量是由最多列数的个数决定的，例如这个布局最多7列，那我们可以使用7个空白标签进行填充占位，最多10列，那我们需要使用10个空白标签。

如下HTML示意：

```html
<div class="container">    
    <div class="list"></div>    
    <div class="list"></div>    
    <div class="list"></div>    
    <div class="list"></div>    
    <div class="list"></div>    
    <div class="list"></div>    
    <div class="list"></div>    
    <i></i><i></i><i></i><i></i><i></i>
</div>
```

相关CSS如下，实现的关键就是占位的`<i>`元素宽度和`margin`大小设置得和`.list`列表元素一样即可，其他样式都不需要写。

```CSS
.container {    
    display: flex;    
    justify-content: space-between;    
    flex-wrap: wrap;    
    margin-right: -10px;
}
.list {    
    width: 100px; 
    height:100px;    
    background-color: skyblue;   
    margin: 15px 10px 0 0;
}
/* 和列表一样的宽度和margin值 */
.container > i {    
    width: 100px;    
    margin-right: 10px;
}
```

由于`<i>`元素高度为0，因此，并不会影响垂直方向上的布局呈现。

最后的效果如下GIF图示：

![图片](https://jiangwen-markdown-img.oss-cn-fuzhou.aliyuncs.com/CSS/flex%E5%B8%83%E5%B1%80%E6%9C%80%E5%90%8E%E4%B8%80%E8%A1%8C%E5%88%97%E8%A1%A8%E5%B7%A6%E5%AF%B9%E9%BD%90%E7%9A%84N%E7%A7%8D%E6%96%B9%E6%B3%95.assets/640-20230626005709595.gif)

### **五、如果列数不固定HTML又不能调整**

然而有时候，由于客观原因，前端重构人员没有办法去调整html结构，同时布局的列表个数又不固定，这个时候该如何实现我们最后一行左对齐效果呢？

我们不妨可以试试使用Grid布局。

Grid布局天然有gap间隙，且天然格子对齐排布，因此，实现最后一行左对齐可以认为是天生的效果。

CSS代码如下：

```CSS
.container {    
    display: grid;    
    justify-content: space-between;    
    grid-template-columns: repeat(auto-fill, 100px);    
    grid-gap: 10px;
}
.list {    
    width: 100px; 
    height:100px;    
    background-color: skyblue;    
    margin-top: 5px;
}
```

可以看到CSS代码非常简洁。

HTML代码就是非常规整非常普通的代码片段：

```CSS
<div class="container">    
    <div class="list"></div>    
    <div class="list"></div>    
    <div class="list"></div>    
    <div class="list"></div>    
    <div class="list"></div>    
    <div class="list"></div>    
    <div class="list"></div>
</div>
```

最后的效果如下GIF：

![图片](https://jiangwen-markdown-img.oss-cn-fuzhou.aliyuncs.com/CSS/flex%E5%B8%83%E5%B1%80%E6%9C%80%E5%90%8E%E4%B8%80%E8%A1%8C%E5%88%97%E8%A1%A8%E5%B7%A6%E5%AF%B9%E9%BD%90%E7%9A%84N%E7%A7%8D%E6%96%B9%E6%B3%95.assets/640-20230626005709595.gif)

### **六、这几种实现方法点评**

首先最后一行需要左对齐的布局更适合使用CSS grid布局实现，但是，`repeat()`函数兼容性有些要求，IE浏览器并不支持。如果项目需要兼容IE，则此方法需要斟酌。

然后，适用范围最广的方法是使用空的元素进行占位，此方法不仅适用于列表个数不固定的场景，对于列表个数固定的场景也可以使用这个方法。但是有些人代码洁癖，看不惯这种空的占位的html标签，则可以试试一开始的两个方法，一是动态计算margin，模拟两端对齐，另外一个是根据列表的个数，动态控制最后一个列表元素的margin值实现左对齐。

累计6种方法，各有各的优缺点，大家根据自己项目的实际场景，选择合适的方法。
