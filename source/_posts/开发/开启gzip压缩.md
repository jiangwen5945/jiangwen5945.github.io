---
title: 开启gzip压缩
tags: [优化， gzip]
categories: 开发
date: 2022-03-06 18:50:24
thumbnail: https://jiangwen-blog.oss-cn-fuzhou.aliyuncs.com/images/20ab6cb5cef84475ad912705b3d5da51.webp
---

1. 首先安装插件 `npm i compression-webpack-plugin@10.0.0 `

2. 添加 vue.config.js 项目配置

   ```js
   const { defineConfig } = require('@vue/cli-service')
   const CompressionPlugin = require("compression-webpack-plugin");
   
   module.exports = defineConfig({
     configureWebpack: config => {
       // 开发环境不配置
       if (process.env.NODE_ENV !== 'production') return
       // 生产环境才去配置
       return {
         plugins: [
           new CompressionPlugin({
             // filename: "[path][base].gz", // 这种方式是默认的，多个文件压缩就有多个.gz文件，建议使用下方的写法
             filename: '[path][base].gz[query]', //  使得多个.gz文件合并成一个文件，这种方式压缩后的文件少，建议使用
             algorithm: 'gzip', // 官方默认压缩算法也是gzip
             test: /\.js$|\.css$|\.html$|\.ttf$|\.eot$|\.woff$/, // 使用正则给匹配到的文件做压缩
             threshold: 10240, //以字节为单位压缩超过此大小的文件，使用默认值10240吧
             minRatio: 0.8, // 最小压缩比率，官方默认0.8
             //是否删除原有静态资源文件，即只保留压缩后的.gz文件，建议这个置为false，还保留源文件。以防：
             // 假如出现访问.gz文件访问不到的时候，还可以访问源文件双重保障
             deleteOriginalAssets: false
           })
         ]
       }
     }
   })
   ```

3. 项目执行 `npm run build` 打包构建,目录dist/js 文件就会生成 xxx.js.gz 文件。说明压缩成功

4. 配置 nginx 服务开启 gzip

   ```
   server {
       listen 80;
       listen 443 ssl http2;
       server_name xxx.com;
       
       location ~* \.(css|js)$ {
          gzip_static on;
       }
       
       gzip on;
       gzip_min_length 1k;
       gzip_comp_level 9;
       gzip_types text/plain application/javascript application/x-javascript text/css application/xml text/javascript application/x-httpd-php image/jpeg image/gif image/png;
       gzip_vary on;
       gzip_disable "MSIE [1-6]\.";
    
       #....其他配置
   }
   ```
   
5. 在浏览器中查看配置成功后的状态，项目体积减少，页面加载速度得到明显提升

![image-20230504211812296](/Users/jiangwen/Library/Application%20Support/typora-user-images/image-20230504211812296.png)

