---
title: HttpServletRequest
date: 2022-06-08 22:34:00.0
updated: 2022-07-04 20:16:38.82
url: /archives/httpservletrequest
categories: 
- JavaWeb
tags: 
---



#### 内容简介：HttpServletRequest 相关知识点

<!--more-->

## 这个对接口实现类的对象是什么样子的

我们先在 java 程序中获取这个接口实现类的一个对象，看下这个对象是什么

java 程序代码

~~~java
package com.shuyepl.javaweb.servlet.httpservlet;

import jakarta.servlet.ServletException;
import jakarta.servlet.http.HttpServlet;
import jakarta.servlet.http.HttpServletRequest;
import jakarta.servlet.http.HttpServletResponse;
import java.io.IOException;
import java.io.PrintWriter;

public class RequestTest extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) 
        	throws ServletException, IOException {
        response.setContentType("text/html");
        PrintWriter out = response.getWriter();
        out.print(request);
    }
}
~~~

输出结果

~~~txt
org.apache.catalina.connector.RequestFacade@bc5e0e6
~~~

结论：WEB 服务器实现了 HttpServletRequest 的接口

HttpServletRequest 对象由 WEB 服务器负责创建，这个对象中封装了 HTTP 的请求协议（用户发送的请求被封装进了这个对象中），这个对象的声明周期很短，只在当前的请求中有效（ response 也是如此）

## 相应的方法

这些方法可以用来获取前端浏览器用户提交的数据

- String getParameterMap(String name)
  - 获取 Map
- Map<String, String[]> getParameterMap()
  - 获取 Map 集合中所有的 key
- Enumeration<String> getParameterNames()    **
  -  根据 key 获取 Map 集合的 value
- String[] getParameterValues(java.lang.String name)    **
  - 获取 value 这个一维数组当中的第一个元素
- String getRemoteAddr();
  - 获取客户端的 IP 地址
- setCharacterEncoding("UTF-8");
  - 设置请求体的字符集，用来解决 post 请求的乱码问题，不能解决 get 请求的乱码问题（Tomcat 10 及之后的版本默认字符集为 UTF-8 不会出现中文乱码问题，但之前的版本会，而且还有可能会出现输出到页面中的内容也乱码的情况，这个时候，要这样设置：response.setContentType("text/html;charset=utf-8") ）
- String getContextPath()
  - 获取应用的根路径
- String getmethod
  - 获取请求方式
- String getRequestURI()
  - 获取请求的 URI
- String getServletPath()
  - 获取 Servlet 路径

## 请求域对象

HttpServletRequest 的对象是请求域的对象，一次请求对应一个请求对象，一个请求对象对应一个请求域对象，请求域的对象要比应用域的对象范围小，生命周期短很多，请求域中同样也有应用域中有的方法，所以，在两者都能完成需求的情况下，选择对象范围更小的，节省资源

- void setAttribute(String name, Object obj);
- Object getAttribute(String name);
- void removeAttribute(String name);

虽然和应用域的方法一样，但是 ，如果在一个请求域中绑定数据，想在另外一个请求域中获取数据这是不可能的， 因为每一次访问都对应着不同的请求域，不同域中的信息不相通

要是想将不同的 Servlet 对象放到一次请求中，可以使用 Servlet 中的请求转发机制（在一个 Servlet 对象中执行到一半的时候跳转到另外的一个 Servlet 对象中）：使用 request 对象的 getRequestDispatcher() 方法，方法中填写的参数是想要转到的 Servlet 对象的路径（就是 web.xml 中配置的 url-pattern 标签中的内容），得到一个转发器对象，然后使用转发器的 forword 方法，将 request 对象和 response 对象发送出去，下面是一个例子

AServlet 类中的内容

~~~java
+package com.shuyepl.javaweb.servlet.RequestTest;

import jakarta.servlet.ServletException;
import jakarta.servlet.http.HttpServlet;
import jakarta.servlet.http.HttpServletRequest;
import jakarta.servlet.http.HttpServletResponse;
import java.io.IOException;
import java.util.Date;

public class AServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) 
        	throws ServletException, IOException {
        Date newTime = new Date();  // 这个是要添加的参数值
        request.setAttribute("systime", newTime);   // 添加参数
        request.getRequestDispatcher("/b").forward(request,response);   // 将 request 对象转发到 BServlet 对象中
    }
}
~~~

BServlet 类中的内容

~~~java
package com.shuyepl.javaweb.servlet.RequestTest;

import jakarta.servlet.ServletException;
import jakarta.servlet.http.HttpServlet;
import jakarta.servlet.http.HttpServletRequest;
import jakarta.servlet.http.HttpServletResponse;
import java.io.IOException;
import java.io.PrintWriter;

public class BServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) 
        	throws ServletException, IOException {
        response.setContentType("text/html");
        PrintWriter out = response.getWriter();
        out.print("<h1>hello, this is BServlet<h1>");
        // 在 BServlet 对象中调用转发过来的 Request 对象获取在 AServlet 对象中添加的参数
        Object systime = request.getAttribute("systime");
        out.print(systime); // 打印获取到的对象
    }
}
~~~

web.xml

~~~xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="https://jakarta.ee/xml/ns/jakartaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="https://jakarta.ee/xml/ns/jakartaee https://jakarta.ee/xml/ns/jakartaee/web-app_5_0.xsd"
         version="5.0">
    <servlet>
        <servlet-name>a</servlet-name>
        <servlet-class>com.shuyepl.javaweb.servlet.RequestTest.AServlet</servlet-class>
    </servlet>
    <servlet-mapping>
        <servlet-name>a</servlet-name>
        <url-pattern>/a</url-pattern>
    </servlet-mapping>
    <servlet>
        <servlet-name>b</servlet-name>
        <servlet-class>com.shuyepl.javaweb.servlet.RequestTest.BServlet</servlet-class>
    </servlet>
    <servlet-mapping>
        <servlet-name>b</servlet-name>
        <url-pattern>/b</url-pattern>
    </servlet-mapping>
</web-app>
~~~

网页中访问 AServlet 的结果

![001](http://img.shuyepl.com/202207042008908.png)

可以看到，这里执行了 BServlet 对象的程序，输出了调用 BServlet 类时才会输出的内容

注意：转发的对象不一定要是 Servlet 对象，只要是 WEB 容器中的合法资源就可以，只是在转发的时候要注意路径不要写错了

---

参考资料：https://www.bilibili.com/video/BV1Z3411C7NZ?p=27&share_source=copy_web