---
title: session.md
date: 2022-07-10 09:12:24.368
updated: 2022-07-15 12:07:12.922
url: /archives/sessionmd
categories: 
- JavaWeb
tags: 
---

# 为什么要用 session ？

对 OA 系统设置登录界面之后发现，在知道请求路径的情况下可以直接在浏览器中输入路径直接访问相应的操作界面，登录功能形同虚设。

# session （会话）是什么

会话：简单点理解的话就是从你打开浏览器到关闭浏览器的这一整个过程。然后这个会话在服务器端会有一个对应的对象，这个对象就是 session 。（老师是这样讲的，但我理解的就是，一次会话是你访问一个服务器，到访问这个服务器结束的这一整个过程，之后找点书再看清楚一些。）

在 B / S 结构的系统中都有 session 机制，它是一个规范，用于保存会话的状态。

一次会话也可以说是从 session 对象被创建到 session 对象被销毁的这一整个时间段。

# 为什么需要保存会话的状态

因为 HTTP 协议是一种无状态协议，在请求的时候，B 和 S 是连接的，但是在请求结束之后，连接就断开了，你关闭浏览器的时候他是不知道的。

# 保存数据的方式

这个和 HttpServletRequest 很相似，就是调用 setAttribute() 方法和调用 getAttribute() 方法。

# 浏览器识别 session 的方式

为什么浏览器在多次的请求中都能找到内个唯一对应的 session 对象：在服务器中有一个 session 列表（一个 Map 集合）存储了 sessionid 和 session 对象之间的对应关系。

session 记录用户登录信息的方式：在用户第一次访问服务器的时候，服务器会执行 request.getSession() 这个方法，得到一个 session 对象，然后将这个对象和一个 sessionid 对应起来存储在 session 列表中，同时，将这个 sessionid 发给浏览器，浏览器将其以 cookie 的方式保存在缓存中，内容是 JSESSIONID=XXXX（32位），等下一次发送请求的时候顺便把这个 sessionid 发送给服务器，服务器根据这个 sessionid 查找对应的 session 对象。

为什么关闭浏览器后，会话结束：因为关闭浏览器之后，浏览器的缓存就被清理了（cookie 没了），也就是说现在浏览器这边没有 sessionid 这个东西了，等下一次重新打开浏览器发送请求的时候，浏览器没有对应的 sessionid 可以发送，服务器接收不到 sessionid ，没有办法找到对应的session 对象（相当于认为会话结束了，注意这里的 session 对象可能没有被销毁），就会自动重新创建一个 session 对象。

# 获取 session 对象的方式

从服务器中获取对应的 session 对象，如果没有获取到，则新建 session 对象。

~~~java
HttpSession session = request.getSession();
~~~

从服务器中获取对应的 session 对象，如果没有则返回 null 。

~~~java
HttpSession session = request.getSession(false);
~~~

# session 对象的销毁

因为浏览器和服务器之间的通信是基于 http 协议的，它们之间的连接在请求结束之后就断开了，服务器无法知道浏览器是否关闭，所以，session 对象的销毁依靠于超时机制，超时没有请求发送就销毁相应的对象，或者手动销毁，也就是向服务器发送一个请求，请求销毁 session 对象。  

## 超时时长的配置

在 web.xml 文件中配置如下

~~~xml
<session-config>
    <!--   这里的意思是 session 对象 30 分钟未被访问就自动销毁 session 对象     -->
    <session-timeout>30</session-timeout>
</session-config>
~~~

超时时长在没配置的情况下就是 30 分钟。

## 手动销毁

调用 session 对象的 invalidate() 方法

~~~java
session.invalidate();
~~~

# 禁用 cookie

禁用 cookie 的影响：禁用 cookie 表示服务器正常发送的 cookie ，浏览器不接收，那么 sessiionid 也就没有办法保存在浏览器的缓存中，在下次向浏览器发送请求的时候，不会带有 sessionid 的信息，所以，每一次请求都会被服务器当做是第一次发送请求，服务器每一次都会重新创建一个 session 对象。

解决方法：利用 url 重写机制，将 sessionid 的信息通过 url 发送给服务器。这种方法的开发成本较高，所以，禁用 cookie 的这个东西就可以不用去考虑它了。

# JSP 内置 session 对象

程序里面没有创建 session 对象，但是访问 JSP 页面时获取 session 对象却能获取到 session 对象，这是因为 JSP 内置对象中有 session 对象，在访问 JSP 页面的时候 session 对象就已经创建好了，如果在访问 JSP 的时候不想要自动生成这个 session 对象，可以在 JSP 的顶部写上如下的代码

~~~jsp
<%@page session="false" %>
~~~

---

参考资料：https://www.bilibili.com/video/BV1Z3411C7NZ?p=47&share_source=copy_web