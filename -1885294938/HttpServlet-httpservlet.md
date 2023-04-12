---
title: HttpServlet
date: 2022-06-07 15:25:00.0
updated: 2022-07-04 20:16:44.356
url: /archives/httpservlet
categories: 
- JavaWeb
tags: 
---



#### 内容简介：HTTP 协议和 HttpServlet 类的简单了解

<!--more-->

## HTTP 协议

W3C 制定的一种超文本传输协议，B （浏览器）和 S（服务器）之间相互传输数据都要遵守， 这样 B（浏览器） 和 S（服务器） 可以实现解耦合，超文本的意思是不是普通的文本，而是声音、图像、视频等流媒体。

HTTP 协议包括：

- 请求协议：浏览器向服务器发送数据时要遵守的一套标准，协议中规定了数据的具体格式，请求协议包含四部分
  - 请求行
    - 请求方式（七种）
      - get（*）
      - post（*）
      - delete
      - put
      - head
      - options
      - trace
    - URI ：统一资源标识符，代表网络中某个资源的名字，无法定位资源（ URL 是统一资源定位符，代表网络中的某个资源，能定位到资源，URL 包括 URI ）
    - 协议版本号
  - 请求头
    - 请求的主机
    - 主机的端口
    - 浏览器信息
    - 平台信息
    - cookie等信息
    - ...
  - 空白行：区分请求头和请求体
  - 请求体：向服务器发送的具体数据
- 响应协议：服务器向浏览器发送数据时要遵守的一套标准，协议中规定了数据的具体格式，响应协议包含四部分
  - 状态行
    - 协议版本号（例子：HTTP/1.1）
    - 状态码（不同的响应结果对应不同的号码，例子：200 -- > 响应成功，正常结束    404 -- > 访问的资源不存在，是前端错误    405 -- > 前端发送的请求与后端请求的处理方式不一致时发生    500 -- > 服务器端的程序出现了异常，是服务器端的错误    总结：以 4 开头的一般是浏览器端的错误，以 5 开头的一般是服务器端的错误） 
    - 状态的描述信息（ok -- > 表示正常成功结束    not found -- > 表示资源找不到）
  - 响应头
    - 响应的内容类型
    - 响应的内容长度
    - 相应的时间
    - ...
  - 空白行：用来分割响应头和响应体的
  - 响应体：响应的正文，是一个长的字符串，会被浏览器渲染，解释和执行，变成显示的效果

~~请求协议中包含的内容我想了一下， **行头行体** 好像背起来还可以~~

请求：浏览器向服务器发送数据 -- > request

响应：服务器向浏览器发送数据 -- > response

## GET 请求和 POST 请求

到目前为止 ，对于 POST 请求，只有使用表单，且在表单中指明使用 POST 时，才会发送 POST 请求，而对于 GET 请求的情况就比较多了，像下面的情况都是 GET 的请求

- 浏览器地址栏中输入 URL ，回车
- 浏览器中点击超链接
- 使用 form 表单提交数据时，如果没有写 method 数值的值为 post ， 是 get

两个请求的区别

- get 请求会在 URI 的后面加上一个 ？ ，然后将数据放在后面，发送的数据会回显在浏览器的地址栏上，而 post 请求发送数据的时候会在请求体中发送数据，不会回显到浏览器地址栏上，他们发送的数据格式都是一样的，都是 name=value&name=value&name=value... ，只是放的地方不一样而已
  - name 是 form 表单中 input 标签的 name 属性值
  - vlue 是 form 表单中 input 标签的 value 属性值

- get 请求只能发送普通字符串，字符串的长度有限制，无法发送大数据量，post 请求可以发送任何类型的数据，可以发送大量的数据，没有长度的限制
- 在 W3C 中说，get 请求比较适合从服务器端获取数据，post 请求比较适合向服务器端传送数据
- get 请求是安全的，因为只是向服务器请求数据，而 post 请求是危险的，因为它要向服务器发送数据，如果数据通过后门的方式进入服务器中，服务器是危险的    ~~？？？这个是相对于服务器而言的？~~
- get 请求支持缓存，post 请求不支持缓存（缓存是指的浏览器的缓存，get 请求的结果都会被浏览器缓存下来，post 则不会，利用时间戳可以让每个 get 请求都从服务器获取数据，而不使用浏览器的缓存）
- 大部分的 form 表单提交，都是 post 方式；如果表单中有敏感信息， 也一般是用 post 方式；文件上传，也是使用 post

## 模板方法设计模式

模板方法定义核心的算法骨架，具体的实现可以延迟到子类中去实现（定义为抽象方法）。模板类一般是抽象类，模板类中的模板方法一般是要加 final 修饰的，防止子类修改。

## HttpServlet

jakarta.servlet.http 包下的类：

- jakarta.servlet.http.HttpServlet -- > HTTP 协议专用的 Servlet 类，抽象类
- jakarta.servlet.http.HttpServletRequest -- > HTTP 协议专用的请求对象
  - 封装了请求协议的全部内容（ WEB 服务器将请求协议中的数据解析出来之后封装到这个对象当中）
- jakarta.servlet.http.HttpServletResponse -- > HTTP 协议专用的响应对象
  - 响应 HTTP 协议到浏览器

如果在 HttpServlet 的子类中没有重写父类的 doGet 等方法，那么，在子类调用 doGet 方法的时候就会报错，因为 HttpServlet 中不允许使用它内部的 doGet 等方法（这些方法就只会报错），这些方法必须要在子类中重写

doPost 和 doGet 方法不要在 HttpServlet 的子类中一起重写，这样没有意义（下面说到子类的，没有明确说明都是指的 HttpServlet 的子类）

HttpSevlet 类是用模板方法构造起来的类，所以，我们在编写继承了 HttpServlet 类的子类时，只需要重写相应的 doPost 方法，或者 doGet 方法等方法就可以了

## Servlet 类的开发步骤

- 编写 Servlet 类继承 HttpServlet 类
- 重写 doGet 方法或者 doPost 方法
- 配置 Servlet 类到 web.xml 文件中
- 准备前端页面（from 表单），指定请求路径

## web 应用的欢迎页面

在访问路径中没有指定具体访问资源的时候（直接访问站点，也就是相应的 webapp ），会自动访问我们设置的访问页面

如何设置：在 web.xml 文件中将欢迎页面的文件配置到 welcome-file-list 标签中就行了，这里我直接在 web 文件夹下创建一个 index.html 文件，内容如下

~~~html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>这是站点的欢迎页面</title>
</head>
<body>
    <h1>欢迎访问我的页面</h1>
</body>
</html>
~~~

然后是 web.xml 中的内容

~~~xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="https://jakarta.ee/xml/ns/jakartaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="https://jakarta.ee/xml/ns/jakartaee https://jakarta.ee/xml/ns/jakartaee/web-app_5_0.xsd"
         version="5.0">
    
    <!--这里下面的内容就是在配置欢迎页面了-->
    <welcome-file-list>
        <welcome-file>index.html</welcome-file>
    </welcome-file-list>
</web-app>
~~~

然后在浏览器中直接访问该 web 应用的站点就可以直接访问这个页面了

![001](http://img.shuyepl.com/202207042007710.png)

从上面的内容中我们可以看到， welcome-file 标签中要填写的是欢迎页面的文件，而且该文件是从 webapp 的根目录进行查找的

欢迎页面可以配置多个：在 welcome-file-list 标签中可以配置多个 welcome-file 标签，这些标签的优先级越靠上的越高，上面的页面没有找到的，就向下查找

~~~xml
<welcome-file-list>
	<welcome-file>page1/page2/page.html</welcome-file>		<!--这个优先级比下面的标签高-->
    <welcome-file>index.html</welcome-file>					<!--如果上面的页面没有找到，就会找这个页面-->
</welcome-file-list>
~~~

注意：如果我们配置的欢迎页面在 webapp 的根目录下，而且名字为 index.html 时可以不用配置欢迎页面，当我们访问站点的时候会自动访问这个页面（因为 Tomcat 服务器已经为我们配置好了）

配置欢迎页面的地方有两个：

- webapp 里面的 web.xml 文件中，是局部的配置
- CATALINA_HOME/conf/web.xml 文件中，是全局配置

当两个配置同时存在是，局部优先，CATALINA_HOME/conf/web.xml 文件中的配置信息为

~~~xml
<welcome-file-list>
	<welcome-file>index.html</welcome-file>
    <welcome-file>index.htm</welcome-file>
    <welcome-file>index.jsp</welcome-file>
</welcome-file-list>
~~~

欢迎页面就是一个资源，可以是静态资源（ html ），也可以是动态资源（ servlet ）

## WEB_INF 目录

这个目录下的文件是受保护的，在浏览器中不能通过路径直接访问

---

参考链接：https://www.bilibili.com/video/BV1Z3411C7NZ?p=22&share_source=copy_web