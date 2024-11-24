---
title: setup中如何使用mapState
excerpt: false
thumbnail: 'https://jiangwen-blog.oss-cn-fuzhou.aliyuncs.com/images/cover/4.webp'
tags: [vue-cli]
categories: Vue
date: 2020-02-28 12:15:00
---

## 开发环境、测试环境、生产环境

1.开发环境，创建.env.development文件

```json
VUE_APP_BASEURL="http://192.168.0.136:8088"
```

2.测试环境，创建.env.test文件

```json
VUE_APP_BASEURL="http://test.baidu.com"
```

3.生产环境，创建.env.production文件

```json
VUE_APP_BASEURL="http://www.baidu.com"
```



## package.json文件的配置

```json
"scripts": {
    "serve": "vue-cli-service serve",
    "build": "vue-cli-service build",
    "test": "vue-cli-service build --mode test"
  },
```



## 打包命令

1.打生产包

```bash
npm run build
```

2.打测试包

```bash
npm run test
```



## 打包后的dist目录如何启动？

安装web 服务

```bash
npm i -g serve
```

启动

```bash
serve dist
```

