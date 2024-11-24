---
title: vue3实现遮罩层外部禁止页面滚动，内部可滚动
excerpt: false
thumbnail: 'https://jiangwen-blog.oss-cn-fuzhou.aliyuncs.com/images/cover/4.webp'
tags: [vue3]
categories: Vue
date: 2020-02-28 12:15:00
---


> 方法一：直接在遮罩层添加 `@touchmove.prevent` ，就可以实现禁止页面滚动，但是同时遮罩层内部也无法滚动了，适用于内部不需滚动的场景。

> 方法二：(此方法可以实现需求，但是关闭遮罩层后主页面会滑动到顶部，无法回到原来的浏览位置)

```js
function mo = function(e){
    e.preventDefault();
};
 
//禁止滚动-在显示遮罩层的时候调用
function stopScroll(){
   document.body.style.overflow = 'hidden';
   document.addEventListener("touchmove", mo, false);
},
// 取消滑动限制-在关闭遮罩层的时候调用
function resetScroll(){
   document.body.style.overflow = '';//出现滚动条
   document.removeEventListener("touchmove", mo, false);
}
```
> 方法三：（推荐：此方法不仅可以实现需求，还会记录打开遮罩层时的位置，使关闭后仍然停在原来的位置）

```js
// 记录页面滚动位置
const pageLocation = ref('');

// 禁止滚动-在显示遮罩层的时候调用
function stopScroll(e) {
    let scrollTop = window.scrollY;//滚动的高度；
    pageLocation.value = scrollTop;
    document.body.style.position = 'fixed';
    document.body.style.top = '-' + scrollTop + 'px';
};

// 取消滑动限制-在关闭遮罩层的时候调用
function resetScroll() {
    document.body.style.position = 'static';
    window.scrollTo(0, pageLocation.value);
}
```

