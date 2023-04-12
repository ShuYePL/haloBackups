---
title: JSP
date: 2022-06-30 10:45:40.425
updated: 2022-09-01 08:22:35.738
url: /archives/jsp
categories: 
- JavaWeb
tags: 
---

# 为什么使用 JSP

使用纯粹的 Servlet 开发 WEB 程序，将前端代码嵌入到 Java 程序中有很多的痛点，调试难，每次更改都要重新部署上线，代码冗杂等问题挺突出的，使用 JSP 开发能解决这些痛点。

# JSP 是什么

JSP 是一套规范，所有的 WEB 服务器都要遵守这套规范。

# JSP 基本原理

JSP 的作用是，WEB 服务器会将编写有前端代码的 JSP 文件的编写成一个 Java 文件，然后再对 Java 文件进行编译，当我们在浏览器地址栏上面访问这个 JSP 文件的时候就是在访问对应的 Java 程序，WEB 服务器会自动调用 JVM 去执行它。并且，使用 JSP 编写前端代码有一个好处就是，当我们更改前端代码的时候，我们不需要去重新启动服务器的服务，当我们访问相应的 JSP 文件时，WEB 服务器会自动将 JSP 文件中的内容动态生成对应的 Java 文件。

执行一个带有 JSP 文件的 WEB 程序的时候，可以在 CATALINA_BASE 的地址下面找到一个 work 文件夹，在这里面放的文件就是 JSP 文件相关的内容，像是在这个文件夹下的 \Catalina\localhost\jsp01\org\apache\jsp 文件夹中可以找到相关 JSP 文件生成的 .java 文件和 .class 文件。

# JSP 生成的 Java 类

JSP 生成的 Java 类继承 org.apache.jasper.runtime.HttpJspBase 这个类，所以如果我们要研究 JSP 的话，要好好研究下这个类。

# HttpJspBase

在源码中可以看到：public abstract class HttpJspBase extends HttpServlet implements HttpJspPage

HttpJspBase 继承 HttpServlet ，而 JSP 又是继承的 HttpJspBase ，所以 JSP 就是一个 Servlet 对象，一圈下来，其实我们还是在玩 HttpServlet，O(∩_∩)O哈哈~

这是一个 Servlet 对象，那么很多我们之前的知识就能运用到这个地方上了，像是第一次访问 JSP 的时候会比较慢：因为生成 .java 文件和 .class 文件这个过程是在我们访问 JSP 文件的时候才发生的，同时，因为这是 Servlet 对象，在第一次访问它的时候还需要进行创建对象和初始化对象的操作，之后才调用 Servlet 对象的 service 方法，所以我们第一次访问 JSP 文件的时候加载是比较慢的，由于 Servlet 类是单例的，之后的访问就会比较快了。

# 解决 JSP 的中文乱码问题

在 JSP 的顶部加上以下的代码

~~~jsp
<% page contentType="text/html;charset=UTF-8" %>
~~~

# 在 JSP 里面编写 Java 代码

在要编写代码的地方写上 <%%> ，把代码写在里面，在这里面的代码会被翻译到 service 方法里。

另外还要一种方式是 <%! %>，也是把代码写里面，但是，这里面的代码会被翻译到 service 方法外面，被写到类体中了，所以，这里可以写成员变量和方法。并不建议使用这种方式，存在线程安全问题。

# JSP 里面的注释

<%-- --%> 是 JSP 里面的专业注释，这个注释信息不会被翻译到 Java 程序当中。

# 访问 JSP 文件出现 500 报错

这个可能是 Java 程序有问题，编译出错了，没有编译生成相应的 class 文件。

# JSP 输出 Java 变量

使用 JSP 内置对象 out ，使用方式是

~~~java
<%
	out.write("这里可以拼接要输出的字符串 ");    
%>
~~~

使用 <%= %> ，在 = 号的后面填写要输出的 Java 语句，例如

~~~java
<%= %>
~~~

在翻译的时候会被翻译成 out.print(); ，放在 service 方法中

# JSP 里面导包

~~~jsp
<%@page import="包1, 包2, 包3, 包4"%>
~~~

# JSP 指令

## 指令作用

指导 JSP 翻译引擎如何翻译 JSP 文件。

## 指令使用语法

~~~jsp
<%@指令名 属性名=属性值 属性名=属性值 ... %>
~~~

## JSP 三大指令

### include

包含指令，在 JSP 中完成静态包含，很少使用。

### taglib

引入标签库的指令，在 JSTL 标签库学习的时候学习这个。

### page

常用属性

- session ：属性值可以是 false 或者 true ，true 就启用 JSP 的内置 session 对象，false 就不启用。

  - ```jsp
    <%@page session="false" %>
    ```

- contentType ：设置响应的内容类型。

  - ```jsp
    <%@page contentType="text/html" %>
    ```

- pageEncoding ：设置响应使用的字符集，这个一般配置在 contentType 后面。

  - ```jsp
    <%@page contentType="text/html" pageEncoding="UTF-8" %>
    <%--  单独写一个标签也是可以的  --%>
    <%@page pageEncoding="UTF-8" %>
    ```

    同样配置响应使用的字符集的方式，还有另外一种方式是直接将字符集信息写在 contentType 属性中，样式如下

    ```jsp
    <%@page contentType="text/html;charset=UTF-8" %>
    ```

- inport ：导包

  - ```jsp
    <%page import="java.util.*" %>
    ```

- errorPage ：设置错误跳转页面，如果当前 JSP 页面出错了，那么就会跳转到这个指定的页面中。

  - ```jsp
    <%@page errorPage="/error.jsp" %>
    ```

    这个方法在跳转后无法查看到具体的出错信息，所以需要在出错页面中给出提示，提示的方式可以采用 JSP 内置对象 exception ，详细的见下一个属性。

- isErrorPage ：设置是否在给页面启用 exception 对象，属性值可以是 true 或者 false ，true 表示启用 exception 对象，false 表示不启用。

  - ```jsp
    <%@page isErrorPage="true" %>
    ```

    配置这个之后就可以在这个 JSP 程序中通过 exception 对象获取异常信息了（这里的异常对象就是刚刚发生的异常）。

    ```jsp
    <%
    	exception.printStackTrace();
    %>
    ```

- ifELIgnored ：设置是否忽略 EL 表达式

  - ```jsp
    <%@page isELIgnored="true" %>
    ```

    true 表示忽略 EL 表达式。

# JSP 九大内置对象

## jakarta.servlet.jsp.PageContext pageContext

页面作用域

## jakarta.servlet.http.HttpServletRequest request

请求作用域

## jakarte.servlet.http.HttpSessio session

会话作用域

## jakarta.servlet.ServletContext application

应用作用域

上面四个作用域的大小为：pageContext < request < session < application

四个作用域都有：setAttribute、getAttribute、removeAttribute 方法

上面四个作用域的使用原则是：尽可能使用小的域

## java.lang.Throwable exception

## jakarta.servlet.ServletConfig config

## jakarta.lang.Object page

其实是 this ，也就是当前的 servlet 对象。

## jakarta.servlet.jsp.jspWriter out

负责输出

## jakarta.servlet.http.HttpServletResponse response

负责响应

---

参考资料：https://www.bilibili.com/video/BV1Z3411C7NZ?p=33&share_source=copy_web