---
title: Koa2快速上手
excerpt: false
thumbnail: 'https://jiangwen-blog.oss-cn-fuzhou.aliyuncs.com/images/cover/4.webp'
tags: [Koa2]
categories: [Node]
date: 2020-02-28 12:15:00
---


#### 检查Node版本

```bash
node -v  // => 大于v7.6
```

#### 安装Koa

```bash
npm init 
npm install koa
```

#### 创建编写app.js文件

```javascript
// 1. 创建koa对象
const Koa = require('koa')
const app = new Koa()
// 2. 编写响应函数（中间件 ）
app.use((ctx, next) => {
  ctx.response.body = 'hi'
})
// 3. 绑定端口号
app.listen(3000)
```

##### 启动服务器

```
node app.js
```



## Koa项目

```
Koa                                 源码
├── node_modules                     - npm依赖包
├── data                             - json模拟数据
├── middleware                       - 中间件
│   |── koa_response_data.js            - 业务逻辑中间件
│   |── koa_response_duration.js        - 计算耗时中间件
│   └── koa_response_header.js          - 响应头中间件
├── utils                           - 工具模块
│   └── fils_utils.js               	- 读取文件工具方法
└── app.js                          - 入口文件
```

