---
title: JS位置距离
date: 2018-08-28 11:48:37
tags: JS
---

## 名词解释

* screen：屏幕。这一类取到的是关于屏幕的宽度和距离，与浏览器无关，应该是获取window对象的属性。
* client：使用区、客户区。指的是客户区，当然是指浏览器区域。
* offset：偏移。指的是目标甲相对目标乙的距离。
* scroll：卷轴、卷动。指的是包含滚动条的的属性。
* inner：内部。指的是内部部分，不含滚动条。
* avail：可用的。可用区域，不含滚动条，易与inner混淆。

#### 特别注意:
* body：DOM对象里的body子节点，即 \<body\> 标签。（未声明`<!DOCTYPE html`>）
* documentElement：整个节点树的根节点root，即\<html\> 标签。（未声明`<!DOCTYPE html`>）

> 使用body或者documentElement在于是否声明了`<!DOCTYPE html>`

> 即：在标准w3c下（声明了`<!DOCTYPEhtml>`），document.body.scrollTop恒为0，需要用document.documentElement.scrollTop来代替,ie5.5之后已经不支持`document.body.scrollX`对象了。

所以在编程的时候，请加上这样的判断

```
if (document.body && document.body.scrollTop && document.body.scrollLeft)
{
　　top=document.body.scrollTop;
　　left=document.body.scrollleft;    
}
if (document.documentElement && document.documentElement.scrollTop && document.documentElement.scrollLeft)
{
　　top=document.documentElement.scrollTop;
　　left=document.documentElement.scrollLeft;
}
```


### Window对象

>获取设备屏幕的高度和宽度（屏幕分辨率）：

    window.screen.height
    window.screen.width
    

>获取屏幕工作区域的高度和宽度（去掉状态栏）：

    window.screen.availHeight
    window.screen.availWidth

>浏览器可见区域的内宽度、高度（不含浏览器的边框，但包含滚动条）:

    window.innerHeight
    window.innerWidth
    
 
   
>浏览器的位移（浏览器与屏幕左上角的距离）

    window.screenLeft/screenTop (兼容：ie6/7/8/9/10、chrome)
    window.screenY/screenX  (兼容：ie9/10、chrome、firefox)
    * 注意：ie浏览器的内边缘距离屏幕边缘的距离，chrome浏览器的外边缘距离屏幕边缘的距离。 



>滚动条偏移距离(滚动条滚动距离)

    window.pageXOffset/pageYOffset(兼容：ie9/10、chrome、firefox。)
    window.scrollX/scrollY(兼容：chrome、firefox)
    

### document对象

>滚动条偏移距离(滚动条滚动距离)

    document.body.scrollTop
    document.body.scrollLeft
    document.documentElement.scrollTop
    document.documentElement.scrollLeft
    
>网页可见区域的高度和宽度（不加边线）：

    document.body.clientHeight
    document.body.clientWidth
    document.documentElement.clientHeight
    document.documentElement.clientWidth

>网页全文的高度和宽度：滚动条总高度（含边线）

    document.body.scrollHeight
    document.body.scrollWidth
    document.documentElement.scrollHeight
    document.documentElement.scrollWidth
 
 >网页全文的高度和宽度：滚动条总高度（不含边线）：

    document.body.offsetHeight
    document.body.offsetWidth  
    document.documentElement.offsetHeight
    document.documentElement.offsetWidth  


### element对象
>有滚动条时：clientWidth=元素左内边距宽度+元素宽度+元素右内边距宽度-元素垂直滚动条宽度

>无滚动条时：clientWidth=元素左内边距宽度+元素宽度+元素右内边距宽度

    elment.clientWidth/clientHeight
    兼容：chrome、firefox、ie6/7/8/9/10

>clientLeft为左边框宽度，clientTop为上边框宽度。

    element.clientLeft/clientTop
    兼容：chrome、firefox、ie6/7/8/9/10



>偏移量，包含元素在屏幕上所用的所有可见空间(包括所有的内边距滚动条和边框大小，不包括外边距)

    element.offsetWidth/offsetHeight
    兼容：chrome、firefox、ie6/7/8/9/10
    

>表示该元素相对于最近的定位祖先元素的距离

    element.offsetLeft/offsetTop
    兼容：chrome、firefox、ie6/7/8/9/10
    差异：
        chrome：offsetLeft=定位祖先左边框宽度+定位祖先元素左内边距宽度+左位移+左外边距宽度
        ie6/7/8/9/10、firefox：offsetLeft=定位祖先元素左内边距宽度+左位移+左外边距宽度
        即：chrome比其他浏览器多计算了定位祖先元素的边框


>获得水平、垂直滚动条的距离。

    element.scrollLeft/scrollTop
    兼容：chrome、firefox、ie6/7/8/9/10


>包含滚动内容的元素大小

    element.scrollWidth/scrollHeight
    兼容：chrome、firefox、ie8/9/10、ie6/7（半兼容）
    有滚动条时：
        chrome、firefox、ie8/9/10：左内边距宽度+内容宽度
        ie6/7：左内边距宽度+内容宽度+右内边距宽度（是由CSS的BUG引起）
    无滚动条时：
        左内边距宽度+宽度+右内边距宽度

## 常用总结

屏幕宽度(分辨率)：`window.screen.width`

元素内容宽度：`element.clientWidth`

元素占位宽度：`element.offsetWidth`

滚动条偏移距离：  

    window.pageXOffset/pageYOffset(兼容：ie9/10、chrome、firefox)
    window.scrollX/scrollY(兼容：chrome、firefox)
    document.body.scrollTop（没有声明<!DOCTYPE html>）
    document.documentElement.scrollTop（声明<!DOCTYPE html>）

网页可视区域宽/高度：

    window.innerWidth/innerHeight
    document.body.clientWidth/clientHeight








