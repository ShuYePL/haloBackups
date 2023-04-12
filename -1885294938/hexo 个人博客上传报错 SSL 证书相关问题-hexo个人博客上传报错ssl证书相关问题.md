---
title: hexo 个人博客上传报错 SSL 证书相关问题
date: 2022-06-03 14:34:00.0
updated: 2022-07-05 21:02:12.616
url: /archives/hexo个人博客上传报错ssl证书相关问题
categories: 
- 博客建设
tags: 
---



#### 内容简介：博客文章推送时 SSL 证书验证不通过的解决方法

<!--more-->

我因为国内 Github 的访问经常会断开，所以就使用了 steam++ 来为 Github 访问加速，然而，我发现在加速器开起来之后我就没有办法推送我的个人博客文章，就导致我现在使用加速器看起来好像很鸡肋，只要我想将自己写好的博客文章推送到 Github 上面就会出错：fatal: unable to access 'https://github.com/ShuYePL/ShuYePL.github.io/': SSL certificate problem: unable to get local issuer certificate，报错情况如下

![001](http://img.shuyepl.com/202207052100221.png)

我猜测可能是因为加速器的原因导致 SSL 证书验证没有通过

解决方法是解除 SSL 验证，在命令行中输入下面的代码

~~~cmd
git config --global http.sslverify "false"
~~~

然后重新推送文章就可以推送成功了