---
title: Hexo博客使用
tags: 博客日志
abbrlink: 59612
date: 2018-09-30 16:34:20
---

## Git管理博客源码

### 博客源码初始备份：
> 由于在本地`hexo g -d`操作上传到git上的只是生成的`public`静态博客页面文件夹，为了实现多台电脑管理操作，可以在Git上建立分支dev-blog保存备份hexo源码.

``` git
$ git init                                     //初始化一个git仓库
$ git remote add origin [仓库地址]             //与远程仓库建立连接
$ git add .                                   //在本地把源代码提交到本地版本库中
$ git commit -m '提交说明'                     //添加到暂存区，并备注提交说明
$ git push origin master:dev-blog             //推送到远程仓库的源代码分支上面
```

*tips: 这样就把源代码备份完毕，可以在远程仓库里面看见dev-blog分支，里面保存着博客源代码*


***

### 获取Git远程分支源码：

> 首先，电脑需安装好git、node、npm，然后使用克隆clone的命令下载源代码：

```git
$ git clone -b dev-blog git@github.com:xxx/xxx.github.io.git // 克隆分支源码
```

> 然后，使用npm包管理工具安装相应的配置

```bash
npm install hexo      // 安装hexo「不需要初始化hexo，否者hexo 配置参数会重置」
npm install                               // 安装依赖库
npm  install hexo-deployer-git  --save    // 安装部署相关配置
```

> 接着同步获取的源代码到github分支

```git
$ git add .
$ git commit -m 'add new blog again'
$ git pull origin dev-blog  //先拉取仓库的源代码到本地进行合并，解决不同版本的冲突
$ git push origin dev-blog  //再将现有的源代码文件push到dev-blog分支
```

***

### 本地源码修改提交：


```git
<!-- 将本地博客源码提交Git仓库中的dev-blog分支 -->

$ git add .
$ git commit -m 'add new blog'
$ git push origin master:dev-blog   //源码的本地仓库分支为master,远程仓库分支为dev-blog
```
*tips: 如果源码的本地仓库分支也是dev-blog，推送可简写为：`$ git push origin dev-blog `*
 
<br>
<br>

## Hexo常用命令

### 新建博客

``` bash
$ hexo new "My New Post"
```

More info: [Writing](https://hexo.io/docs/writing.html)

### 运行本地服务器

``` bash
$ hexo server    // hexo s -p [端口号] （可修改默认的本地服务端口）
```

More info: [Server](https://hexo.io/docs/server.html)

### 生成静态博客文件

``` bash
$ hexo generate   //在public中生成打包好的页面静态资源文件
```

More info: [Generating](https://hexo.io/docs/generating.html)

### 部署到线上

``` bash
$ hexo deploy   // 将生成的public文件夹内容提交到远程仓库master主分支上
```

### 清除静态文件缓存

``` bash
$ hexo clean
```

更多操作: [Deployment](https://hexo.io/docs/deployment.html)

***

## 自定义域名
1. 在布置Hexo博客的平台上开启Pages服务
2. 在购买域名提供商处为域名进行解析（一般需要等待10分钟左右生效），关联Pages服务的地址，我配置在Coding平台的的参数如下：

|主机记录|记录类型|记录值|
|---|---|---|
|@|CNAME|jiangwen1994.coding.me|
|www|CNAME|jiangwen1994.coding.me|

>tips: 每个托管平台的记录值有差异，Github和Coding的记录值为该平台提供的Pages服务页面访问地址，而Gitee码云的记录值为固定的gitee.gitee.io
