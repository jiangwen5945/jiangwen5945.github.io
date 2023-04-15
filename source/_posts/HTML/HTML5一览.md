---
title: HTML5一览
tags: [h5]
categories: HTML
thumbnail: https://jiangwen-blog.oss-cn-fuzhou.aliyuncs.com/images/20ab6cb5cef84475ad912705b3d5da51.webp
date: 2020-01-01 18:00:00
---

## HTML

- [x] 语义化

  - 页面结构更加清晰和可读性，有利于团队开发和维护
  - 方便其他设备解析
  - 有利于SEO，搜索引擎根据标签来确定上下文和各个关键字的权重
  - 易于用户阅读，样式丢失的时候能让页面呈现清晰的结构

  

- [x] H5新增标签特性

  |    标签名    |                             说明                             |
  | :----------: | :----------------------------------------------------------: |
  |  <article>   |                定义文章，规定独立的自包含内容                |
  |   <aside>    |                    定义页面内容之外的内容                    |
  |  <section>   |                    定义文档中的小节、区段                    |
  |   <header>   |                   定义section或page的页眉                    |
  |   <footer>   |                   定义section或page的页脚                    |
  |    <main>    |                      定义文档的主要内容                      |
  |    <nav>     |                         定义导航链接                         |
  |    <bdi>     |       定义文本的文本方向，使其脱离其周围文本的方向设置       |
  |   <canvas>   |                         定义图形画布                         |
  |  <command>   |              定义命令按钮  (仅ie9支持这个标签)               |
  |  <datalist>  |                定义下拉列表（配合input使用）                 |
  |  <details>   | 定义元素的细节:页面只显示summary里的内容，点击之后才显示详细内容 |
  |  <summary>   |                为<details>元素定义可见的标题                 |
  |   <embed>    |                    定义外部交互内容或插件                    |
  | <figcaption> |                     定义figure元素的标题                     |
  |   <figure>   |                           定义媒介                           |
  |   <keygen>   |                         定义生成密钥                         |
  |    <mark>    |                       定义有记号的文本                       |
  |   <meter>    |                    定义预定义范围内的度量                    |
  |   <output>   |                      定义输出的一些类型                      |
  |  <progress>  |                   定义任何类型的任务的进度                   |
  |   <audio>    |              定义声音内容，比如音乐或其他音频流              |
  |   <source>   |                          定义媒介源                          |
  |   <dialog>   |                       定义对话框或窗口                       |
  |    <time>    |                        定义日期/时间                         |
  |   <track>    |                定义用在媒体播放器中的文本轨道                |
  |   <video>    |                           定义视频                           |
  |    <wbr>     |                  定义英文字符自动进行软换行                  |

  > tip : 对于一段主题性的内容，则就适用`section`，而假如这段内容可以脱离上下文，作为完整的独立存在的一段内容，则就适用 `article`  

  

- [x] input和textarea的区别

  - 有无value属性
  - 是否闭合标签

  

- [x] 用div模拟textarea的实现

  ```css
  .editBox {
      width: 400px;
      min-height: 100px;
      max-height: 300px;
      _height: 100px; /* IE6 */
      padding: 3px;
      outline: 0;
      border: 1px solid #a0b3d6;
      overflow-y: auto; /* 超过最大高度就出现滚动条 */
      _overflow-y: visible;
  }
  
  <div class="editBox" contenteditable></div>
  ```

  

- [x] 移动设备忽略将页面的数字识别为电话号码

  <meta name = "format-detection" content = "telephone=no">