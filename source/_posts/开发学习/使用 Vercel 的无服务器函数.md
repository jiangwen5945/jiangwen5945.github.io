---
title: 使用 Vercel 的无服务器函数
excerpt: false
thumbnail: 'https://jiangwen-blog.oss-cn-fuzhou.aliyuncs.com/images/dd3e880811ebb6e017c2d2eca2.webp'
tags: [Vercel]
categories: 开发
date: 2020-02-28 12:15:00
---



前言：使用 Vercel，您可以部署无服务器函数，这是使用 NodeJS 等后端语言编写的代码片段，它们接受 HTTP 请求并提供响应。

您可以使用无服务器函数来处理用户身份验证、表单提交、数据库查询、自定义 Slack 命令等。

在本文中，我们将使用 NodeJS 创建一个简单的无服务器函数，然后将其部署在 Vercel 中。

## 使用 API 端点创建项目

初始化项目`npm`

```
$ npm init -y
```



现在我们需要创建一个名为 API 端点文件所在的文件夹。`/api`

在此示例中，我们将创建一个名为 的文件，其中包含以下内容：`hello.js`

```
module.exports = (req, res) => {
    res.json({
        hola: 'mundo'    
    })
}
```



您的项目现在如下所示

[![VSCode](https://res.cloudinary.com/practicaldev/image/fetch/s--Uk9-PbbN--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://dev-to-uploads.s3.amazonaws.com/uploads/articles/vm5or9mb9q1qf5hcyw6w.png)](https://res.cloudinary.com/practicaldev/image/fetch/s--Uk9-PbbN--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://dev-to-uploads.s3.amazonaws.com/uploads/articles/vm5or9mb9q1qf5hcyw6w.png)

在此示例中，我们的终端节点服务将使用具有以下结构的 JSON 进行响应：

```
{
    hola: 'mundo'
}
```



## 部署到韦塞尔

以前，您需要安装和配置 [Vercel CLI](https://vercel.com/download)。

```
$ npm i -g vercel
```



在终端中，在项目的根目录下写入：

```
$ vercel
```



[![韦塞尔 CLI](https://res.cloudinary.com/practicaldev/image/fetch/s--pTqvgmPE--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://dev-to-uploads.s3.amazonaws.com/uploads/articles/voibizmdx5e1vrwukrnm.png)](https://res.cloudinary.com/practicaldev/image/fetch/s--pTqvgmPE--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://dev-to-uploads.s3.amazonaws.com/uploads/articles/voibizmdx5e1vrwukrnm.png)

现在，在Vercel网络仪表板中，您将看到您的项目和项目URL

[![Vercel Dashboard](https://res.cloudinary.com/practicaldev/image/fetch/s--BtRzLERJ--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://dev-to-uploads.s3.amazonaws.com/uploads/articles/wugczr28rjyd15cx6bv1.png)](https://res.cloudinary.com/practicaldev/image/fetch/s--BtRzLERJ--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://dev-to-uploads.s3.amazonaws.com/uploads/articles/wugczr28rjyd15cx6bv1.png)

现在，让我们在浏览器中测试我们的服务，转到项目 URL，并记得添加 API 路径，在本例中为`/api/hello`

[![在浏览器中测试](https://res.cloudinary.com/practicaldev/image/fetch/s--wQS2Ikyi--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://dev-to-uploads.s3.amazonaws.com/uploads/articles/lu0u30x6iu2ojl5v8alq.png)](https://res.cloudinary.com/practicaldev/image/fetch/s--wQS2Ikyi--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://dev-to-uploads.s3.amazonaws.com/uploads/articles/lu0u30x6iu2ojl5v8alq.png)

就这样。。。现在轮到你了，在 API 中创建你需要的所有端点，只要记住每个端点都是一个文件。