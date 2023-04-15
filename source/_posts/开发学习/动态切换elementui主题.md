---
title: 动态切换elementui主题
tags: [elementui]
categories: 开发
excerpt: false
date: 2021-03-06 18:50:24
thumbnail: 'https://jiangwen-blog.oss-cn-fuzhou.aliyuncs.com/images/2387ae258a104923ad649dbb740b4f8b.webp'
---


#### 准备自定义的主题

> 在[element ui](https://element.eleme.cn/#/zh-CN/theme)官网主题页下载自定义后的主题，多套主题存放本地，两者有不同的命名空间，如写两套主题，一套叫 `theme-dark`，一套叫 `theme-light` ，主题都在它的 `.theme-dark`或`.theme-light` 的命名空间下
 

#### 批量为css文件扩展命名空间

> 使用gulp-css-wrap将这个主题的每个元素外面包裹一个class 来做命名空间。

1. 创建一个新的项目`css-wrap`，并初始化搭建gulp环境

   ```
   // 1.安装gulp：
   npm install gulp
   // 2.安装gulp-clean-css
   npm install gulp-clean-css
   // 3.安装gulp-css-wrap
   npm install gulp-css-wrap
   ```


2. 在项目根目录下创建一个名为 gulpfile.js 的文件

   ```js
   var path = require('path')
   var gulp = require('gulp')
   var cleanCSS = require('gulp-clean-css')
   var cssWrap = require('gulp-css-wrap')
   gulp.task('css-wrap', function () {
     return gulp.src(path.resolve('./theme/index.css'))
     /* 找需要添加命名空间的css文件，支持正则表达式 */
     .pipe(cssWrap({
         selector: '.custom-dark' /* 添加的命名空间 */
     }))
     .pipe(cleanCSS())
     .pipe(gulp.dest('src/theme')) /* 存放的目录 */
   })
   ```

 3. 将生成在`scr/theme`下的css和字体文件复制到所需项目中

    


#### 项目中动态切换主题

1. 在`main.js`中引入全部所需的css主题样式

```js
import 'element-ui/lib/theme-chalk/index.css'; // 基础css不能少，否则一些组件会丢失样式
import "../src/styles/theme/dark.css";
import "../src/styles/theme/light.css";
```

2. 设置`App.vue`入口页面中`#app`的元素的类名为默认主题的名称

```vue
<template>
  <div id="app" class="custom-dark">
    <router-view></router-view>
  </div>
</template>
```

3.  修改`#app`元素的类目切换主题

```js
document.getElementById("app").setAttribute("class", 'custom-' + this.theme);
```

