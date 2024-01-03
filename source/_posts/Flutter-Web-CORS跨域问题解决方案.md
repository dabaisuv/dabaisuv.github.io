---
title: Flutter Web CORS跨域问题解决方案
date: 2024-01-03 15:57:07
tags:
---
**作者：dabaisuv，原文链接：https://dabaisuv.github.io/2024/1/03/Flutter-Web-CORS跨域问题解决方案**

**问题描述：**
在使用Flutter web访问`localhost:8080/api/xxx` 接口时，http库一直报错不知名原因，后台也服务器也显示收到了请求并返回，postman也能成功打开，进入到浏览器的控制台发现是跨域的问题-_-。

**解决方案：**
这是浏览器的限制，不让你的网站程序随意访问别人网站资源。
如果这是你的服务器资源，在你服务器每次请求返回的`header`中加入：`'Access-Control-Allow-Origin': '*'`。
其中`*`可以改为特定的允许主机名。