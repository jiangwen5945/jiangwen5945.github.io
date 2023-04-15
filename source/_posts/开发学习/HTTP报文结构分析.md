---
title: HTTP报文结构分析
excerpt: false
tags: [http]
categories: 浏览器
date: 2021-01-01 18:00:00
thumbnail: 'https://jiangwen-blog.oss-cn-fuzhou.aliyuncs.com/images/dd3e880811ebb6e017c2d2eca2.webp'
---

## 请求报文

HTTP请求报文由请求行、请求头、空行和请求内容4个部分构成

![1556086946814](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1556086946814.png)

### 请求行：

请求行由请求方法字段、URL字段、协议版本字段三部分构成，字段之间由空格隔开

常用的请求方法有：GET、POST、HEAD、PUT、DELETE、OPTIONS、TRACE、CONNECT

### 请求头：

请求头由key/value对组成，每行为一对，key和value之间通过冒号(:)分割。

请求头的作用主要是用于通知服务端有关于客户端的请求信息

常见的请求头(Request Headers)有:

- **User-Agent：**生成请求的浏览器类型
- **Accept：**客户端可识别的响应内容类型列表；星号*用于按范围将类型分组
- **Accept-Language:**  客户端可接受的自然语言
- **Accept-Encoding:**  客户端可接受的编码压缩格式
- **Accept-Charset：** 可接受的字符集
- **Host:** 请求的主机名，允许多个域名绑定同一IP地址
- **connection：**连接方式（close或keeplive）
- **Cookie:**  存储在客户端的扩展字段

### 空行:

最后一个请求头之后就是空行，用于告诉服务端以下内容不再是请求头的内容了。

### 请求内容:

请求内容主要用于POST请求，与POST请求方法配套的请求头一般有Content-Type（标识请求内容的类型）和Content-Length（标识请求内容的长度）



## 响应报文

HTTP响应报文由状态行、响应头、空行和响应内容4个部分构成

![1556087134966](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1556087134966.png)



### 状态行：

由HTTP协议版本、状态码、状态码描述三部分构成，它们之间由空格隔开

状态码由3位数字组成，第一位标识响应的类型，常用的5大类状态码如下：

- 1xx：表示服务器已接收了客户端的请求，客户端可以继续发送请求	
- 2xx：表示服务器已成功接收到请求并进行处理
- 3xx：表示服务器要求客户端重定向
- 4xx：表示客户端的请求有非法内容
- 5xx：标识服务器未能正常处理客户端的请求而出现意外错误

### 响应头:

一般情况下，响应头会包含以下，甚至更多的信息。

Location：服务器返回给客户端，用于重定向到新的位置

Server： 包含服务器用来处理请求的软件信息及版本信息

Vary：标识不可缓存的请求头列表

Connection: 连接方式。

对于请求端来讲：close是告诉服务端，断开连接，不用等待后续的求请了。keep-live则是告诉服务端，在完成本次请求的响应后，保持连接，等待本次连接后的后续请求。

对于响应端来讲：close表示连接已经关闭。keeplive则表示连接保持中，可以继续处理后续请求。Keep-Alive表示如果请求端保持连接，则该请求头部信息表明期望服务端保持连接多长时间（秒），例如300秒，应该这样写Keep-Alive: 300

### 空行

最后一个响应头之后就是空行，用于告诉请求端以下内容不再是响应头的内容了。

### 响应内容

服务端返回给请求端的文本信息。