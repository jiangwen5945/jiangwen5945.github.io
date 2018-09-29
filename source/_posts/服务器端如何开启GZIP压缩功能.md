---
title: 服务器端如何开启GZIP压缩功能
date: 2018-08-22 18:16:42
tags: 
- GZIP
categories: 
- web前端
---

> 我们知道做好负载均衡对网站的正常运行，用户体验相当重要。在负载均衡中有一个必须要做的事情就是给服务器开启GZIP压缩功能，对用户请求的页面进行压缩处理，以达到节省网络带宽，提高网站速度的作用。

GZIP是若干文件压缩程序的简称，通常指GNU计划的实现，此处的GZIP代表的就是GUN ZIP，这也是HTTP1.1协议定义的两种压缩方法中最常用的一种压缩方法，客户端浏览器大都支持这种压缩格式。接下来，DNSLA将介绍apache、IIS、nginx 这些现在流行的web服务器如何开启GZIP压缩的方法。

 

## Apache如何开启GZIP功能

Apache开启GZIP要看查看是否已经开启mod_deflate模块，如果没有则需要先加载，在配置文件httpd.conf中将


```
LoadModule deflate_module modules/mod_deflate.so

LoadModule headers_module modules/mod_headers.so
```

前面的#号去掉。DNSLA建议，如果对apache的配置文件不太懂的客户在修改配置文件之前对配置文件进行备份。

开启模块后，在httpd.conf配置文件的最下面空白处添加一下内容：


```
<IfModule mod_deflate.c>
# 告诉 apache 对传输到浏览器的内容进行压缩
SetOutputFilter DEFLATE
# 压缩等级 9
DeflateCompressionLevel 9
</IfModule>
```
这样就能对所有文件进行 gzip 压缩了。压缩等级是个 1-9 之间的整数，取值范围在 1(最低) 到 9(最高)之间，不建议设置太高，虽然有很高的压缩率，但是占用更多的CPU资源。

实际开发中我们并不需要对所有文件进行压缩，比如我们无需对图片文件进行gzip压缩，因为图片文件（一般为jpg、png等格式）本身已经压缩过了，再进行gzip压缩可能会适得其反（详见图片要启用gzip压缩吗？绝对不要！，背景图片千万不要gzip压缩，尤其是PNG)，类似的还有 PDF 以及音乐文件。所以我们可以设置过滤指定文件或者对指定文件进行压缩。

比如我们要对图片等特殊文件不进行 gzip 压缩处理：

```
<IfModule mod_deflate.c>
# 告诉 apache 对传输到浏览器的内容进行压缩
SetOutputFilter DEFLATE
# 压缩等级 9
DeflateCompressionLevel 9
#设置不对后缀gif，jpg，jpeg，png的图片文件进行压缩
SetEnvIfNoCase Request_URI .(?:gif|jpe?g|png)$ no-gzip dont-vary
</IfModule>
```
或者指定文件格式进行压缩：

```
<IfModule mod_deflate.c>
# 压缩等级 9
DeflateCompressionLevel 9
# 压缩类型 html、xml、php、css、js
AddOutputFilterByType DEFLATE text/html text/plain text/xml application/x-javascript application/x-httpd-php
AddOutputFilter DEFLATE js css
</IfModule>
```

其中`DeflateCompressionLevel` 的意思是压缩等级，共分为1-9，9级为最高，不建议使用太高的压缩比，这样会对CPU产生太大的负担。

 

## IIS如何开启GZIP功能

打开IIS管理工具，在右键网站打开网站属性，在服务选项卡中开启HTTP压缩，不建议选中压缩应用程序文件，但一定要选上压缩静态文件，不然就等于没有压缩，达不到负载均衡了。然后选中我那个站下面那个服务器扩展，新建一个服务器扩展，名字为GZIP，下面的添加文件路径为：c:\windows\system32\inetsrv\gzip.dll，然后启用这个扩展。DNSLA提醒大家，还没结束，第三步是，我们要修改配置文件，在配置文件之前要停止IIS服务，（DNSLA提醒大家一定要先关闭IIS服务）打开C:\Windows\System32\inetsrv\MetaBase.xml，这个文件很大，找到下面一段信息：


```
<IIsCompressionScheme  Location ="/LM/W3SVC/Filters/Compression/gzip"

HcCompressionDll="%windir%\system32\inetsrv\gzip.dll"

HcCreateFlags="1"

HcDoDynamicCompression="TRUE"

HcDoOnDemandCompression="TRUE"

HcDoStaticCompression="TRUE"

HcDynamicCompressionLevel="0"

HcFileExtensions="htm

html

txt"

HcOnDemandCompLevel="10"

HcPriority="1"

HcScriptFileExtensions="asp

dll

exe"

>

</IIsCompressionScheme>
```

修改这个文件是要增加一些要进行压缩的文件后缀，其中 HcFileExtensions 是静态文件的扩展名，增加 js 和 css 等；HcScriptFileExtensions 为动态文件的扩展名，增加 aspx，HcDynamicCompressionLevel改成9，（0－10，6是性价比最高的一个）。

然后需要重启一下IIS服务即可。

 

## Nginx如何开启GZIP功能

相对apache 和 IIS nginx开启GZIP简单很多，只需要打开配置文件 nginx.conf找到gzip on 把前面的注释符号#去掉即可开启GZIP服务。然后配置GZIP即可。

下面是一个相对优化不错的配置，DNSLA建议使用。


```
Gzip on;

gzip_min_length 1024;

gzip_buffers   4  8k;

gzip_types   text/plain application/x-javascript text/css  application/xml;
```