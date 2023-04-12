---
title: cookie.md
date: 2022-07-10 10:36:12.763
updated: 2022-07-15 12:07:48.792
url: /archives/cookiemd
categories: 
- JavaWeb
tags: 
---

# cookie 是什么

在服务器向浏览器，或者浏览器向服务器发送的数据中，那个 sessionid 对应的信息就是 cookie 。

# cookie 保存在哪里

浏览器最终保存在浏览器客户端，可能保存在运行内存中，也可能保存在硬盘文件上，后者可以永久保存，而前者只要浏览器关闭，相关信息就消失了。

# cookie 的作用

将会话状态保存在浏览器客户端上。

# cookie 的组成

HTTP 协议规定，任何一个 cookie 都是由字符串型的 name 和 value 组成的。

# Java 的 Servlet 对 cookie 的支持

有一个专门的类表示 cookie 对象，这个类是 jakarta.servlet.http.Cookie ，这个类只有一个有参数的构造方法，里面填写的参数分别是 cookie 的 name 和 value 。

# cookie 怎么发送到浏览器中

通过 response 的 void addCookie(Cookie cookie) 方法。

注意：从浏览器向服务器发送 cookie 这个过程是浏览器自动完成的，HTTP 协议规定了在浏览器发送请求的时候自动将该 path 下的 cookie 发送给服务器。

# cookie 相关方法

## public void setmaxAge()

设置 cookie 的有效时间，参数是一个数字，以秒为单位。设置这个东西之后，在服务器向浏览器发送的响应中，cookie 信息的后面会跟上 cookie 的有效时长信息和过期的时间（按国际标准时间计算），并且只要设置的时间大于 0 ，cookie 就会保存到硬盘文件中。如果没有设置有效时间的话，cookie 保存在浏览器的运行内存中，浏览器关闭，cookie 就消失。如果设置的时间为 0 ，则这样设置的功能主要是删除浏览器上的同名 cookie 。如果设置的参数为负值，则表示该 cookie 不会被存储硬盘文件中，被存储在浏览器运行内存当中。

## public void setPath()

手动设置 cookie 的 path 。

cookie 在没有设置路径的情况下，默认的路径是获取 cookie 时发送请求链接的父路径及该父路径的所有子路径。如果设置了路径的话，那么这个路径下所有的子路径链接，在发送请求的时候都能携带这个 cookie 。

# 怎么接收 cookie

使用 request 的 getCookies() 方法，返回一个 Cookie 数组，如果请求中没有 cookie 的话，这个方法会返回一个 null 。

# 获取 cookie 的 name 和 value

分别调用以下的两个方法

~~~java
cookie.getName();
cookie.getValue();
~~~

---

参考资料：https://www.bilibili.com/video/BV1Z3411C7NZ?p=50&share_source=copy_web