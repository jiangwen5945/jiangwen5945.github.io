---
title: vue项目优化经验总结
tags: [优化， gzip]
categories: 开发
date: 2022-04-16 18:50:24
thumbnail: https://jiangwen-blog.oss-cn-fuzhou.aliyuncs.com/images/20ab6cb5cef84475ad912705b3d5da51.webp
---



> 前言：首先可以通过安装 `npm i webpack-bundle-analyzer`插件，在webpack中引入并配置后，执行`npm run build`打包构建后，浏览器会自动打开分析结果	



```js
// vue.config.js
const { defineConfig } = require('@vue/cli-service')
const { BundleAnalyzerPlugin } = require('webpack-bundle-analyzer')

module.exports = defineConfig({
   chainWebpack: config => {
    if (process.env.NODE_ENV === 'production') {
      config.plugin('webpack-report').use(BundleAnalyzerPlugin, [{ 
       analyzerMode: 'static' }])
    }
  },
})
```

![_Users_jiangwen_GitHub_admin_dist_report.html (1)](/Users/jiangwen/Downloads/_Users_jiangwen_GitHub_admin_dist_report.html%20(1).png)

可以根据得到的分析结果，对项目进行有针对性的优化...


## 懒加载

1. 路由懒加载

   ```js
   const router = new VueRouter({
   	routes: [
   		{path: '/foo', component: () => import('./Foo.vue')	}
   	]
   })
   ```

   

2. 图片懒加载：可以使用 `vue-lazyload` 插件



## 缓存优化

1. 使用`keep-alive`缓存组件





## 打包优化

1. 按需引入UI组件库
2. 开启`gzip`打包压缩
3. 关闭源码映射，不生成`sourcemap`文件





## 首屏优化

1. 数据加载使用loding骨架屏

2. 采用**SSR**服务端渲染

## 代码层面的优化

1. 避免`v-if` 和 `v-for` 同级使用，在vue2 中 v-for 的 优先级比 v-if 高，容易导致渲染错误

2. 使用`v-for`时设置key的值，并且使用数据中的唯一标识，尽量不使用index作为标识，有利于dom定位和diff算法

3. 根据场景合理选择使用`v-if` 或 `v-show`

4. 将无状态的**组件标记为函数式组件**（静态组件）

   ```vue
   <!-- 只接收父组件传过来的值，自己不做处理，无状态，不创建实例 -->
   <template>
     <div class="page">
      	<span v-if='prop.show'>1<span>
       <span v-else>2<span>
     </div>
   </template>
   
   <script>
   	export default{
     	props: ['show']
     }
   </script>
   ```

5. **事件销毁**：Vue组件在销毁时，会自动解绑它的全部指令事件及监听器，但仅限于组件本身。
   比如定时器等最好在销毁阶段手动销毁，避免内存泄漏

6. **子组件分割**：将子组件中耗时的任务交给组件自己管理，不影响整体页面的加载

7. **变量本地化**：减少使用 `this.xxx` 的形式获取数据，可以用一个变量先获取数据，然后在这个变量上处理数据。

8. **模块化、组件化**

9. 长列表优化（插件：vue-virtual-scroller ）

   1. 纯粹做数据展示，不需要热更新的场景🎬：处于data中的数据会被监视，发生变化时数据就发生变化
      所以采用object.freeze（数据）方法冻结数据。
   2. 采用虚拟滚动，只渲染视口部分的数据，也就是说渲染固定的DOM节点个数

