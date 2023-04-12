---
title: EL表达式.md
date: 2022-07-12 21:17:09.119
updated: 2022-07-12 21:17:43.133
url: /archives/el表达式md
categories: 
- JavaWeb
tags: 
---

# EL 表达式的作用

Excepssion Language（表达式语言）。EL 表达式可以代替 JSP 中的 java 代码，让 JSP 文件中的程序看起来更加整洁，美观。它最主要的作用是从某个作用域中取出数据，将其转成字符串，最后输出给浏览器。

- 取数据的作用域是四个域： pageContext、request、session、application

- 装换成字符串的方法是调用对象的 toString 方法

- 输出给浏览器的方式和 <%= %> 差不多

# 基本语法格式

${表达式}

## 普通数据的取法

假如你在 request 中绑定了一个数据

~~~jsp
request.setAttribute("username", "root");
~~~

使用 EL 表达式取出这个数据值的方式是

~~~jsp
${username}
<%-- 不可以添加双引号，添加了双引号之后会被当做普通字符串处理，直接输出到浏览器 --%>
~~~

在 EL 表达式中，调用对象的 get 方法，不需要在对象的后面写上 get 方法，当成属性调用就可以了

~~~jsp
${user.username}	
<%-- 这里的 user 是有一个对应的对象，这个对象里面有一个 username 属性 --%>
<%-- 也可以使用下面的方法取出这个东西 --%>
${user["username"]}
~~~

底层会去调用对象对应属性的 get 方法，也就是说，只要在 Java 程序中有一个 getXXX 方法，你就能通过上面的表达式去调用这个 get 方法，而不用管这个 get 方法是否有与之对应的属性。（这个 get 方法如果没有遵循驼峰命名方式的话是没有问题的，但，规范，还是遵守比较好）

## 取出在 map 中的数据

```jsp
<%@ page import="java.util.Map" %>
<%@ page import="java.util.HashMap" %>
<%@ page contentType="text/html;charset=UTF-8" %>
<%
    // 创建 map 集合
    Map<String, String> map = new HashMap<>();
    // 往 map 中存入数据
    map.put("username", "zhangsan");
    map.put("password", "123");
    // 将 map 绑定到 request 中
    request.setAttribute("userMap", map);
%>

<%-- 将数据取出并输出到浏览器中 --%>
${userMap.username}
${userMap.password}
${userMap["username"]}
${userMap["password"]}
```

## 取出在数组中的数据

```jsp
<%@ page contentType="text/html;charset=UTF-8" %>

<%
    // 创建数组
    String[] names = {"zhangsan", "lisi", "wangwu"};
    // 往 request 中绑定数组
    request.setAttribute("names", names);
%>

<%-- 取出数据 --%>
${names[0]}
${names[1]}
${names[2]}
```

如果数组的下标越界了，什么也不会在浏览器上显示。

## 取 List 集合中的数据

格式和上面取数组中的数据一样。

# 取数据的优先级

在没有指定范围的情况下，如果不同的域里面绑定了相同 name 值的对象，那么在取对象的时候，优先从较小的域中取数据。

## 如何指定读取数据的范围

在 EL 表达式中有四个隐式的对象：pageScope、requestScope、sessionScope、applicationScope 。

在 EL 表达式里面显示带上这些隐式的对象，后面加上一个点 . ，就可以指定从那个域中取出相应的数据了，下面是一个例子

```jsp
<%@page contentType="text/html;charset=UTF-8" %>

<%
    // 在四个域中绑定相同的数据
    session.setAttribute("data", "session");
    request.setAttribute("data", "request");
    pageContext.setAttribute("data", "pageContext");
    application.setAttribute("data", "application");
%>

${pageScope.data}
${requestScope.data}
${sessionScope.data}
${applicationScope.data}
```

# 取数据的时候写错了

使用 <%=%> 调用 request 的 getAttribute() 方法去数据的话会得到 null 。

如果使用 EL 表达式去取的话什么也取不到，在浏览器上什么也没有。

所以，EL 表达式相当于下面的代码

~~~jsp
<%=request.getAttribute("name") == null ?  "" : request.getAttribute("name")%>
~~~

# 使用指令设置是否忽略 EL 表达式

~~~jsp
<%@page isELIgnored="true" %>
~~~

true 表示忽略 EL 表达式，忽略后 EL 表达式就变成普通的字符串了。这个方式是将所有的 EL 表达式都忽略的，但有时候只想要忽略一两个 EL 表达式，这个时候可以直接在想忽略的 EL 表达式前面加上 \ 就行了，不用上面的这个指令。

# EL 表达式中常用的隐式对象

## pageContext

这个 pageContext 对应于 JSP 中的内置对象 pageContext ，通过 JSP 中 pageContext 的一系列 get 方法可以获取到很多对象。

例如在 JSP 中有使用下面这段代码可以获取 request 对象

~~~jsp
<%pageContext.getRequest() %>
~~~

而在 EL 表达式中对应的获取 request 对象的方式为

~~~jsp
${pageContext.request}
~~~

那么依次类推的，在 JSP 中获取项目根目录的方式为 

~~~jsp
<%=((HttpServletRequest)pageContext.getRequest()).getContextPath() %>
~~~

在 EL 表达式中获取这个根路径的方式为

~~~jsp
${pageContext.request.contextPath}
~~~

## param

```jsp
<%=request.getParameter("name")%>
```

等价于

```jsp
${param.name}
```

如果一个 name 对应于多个 value 的话，这个方式只能获取到第一个 value ，如果想要获取多个 value 的话，使用下面的这个隐式对象。

## paramValues

```jsp
${paramValues.name}
```

使用上面这条语句获取到的是一个字符串的数组，等同于

```jsp
<%=request.getParameterValues("name")%>
```

所以，如果想要获取这个数组里面的元素，需要使用下标

```jsp
${paramValues.name[0]}<br>
${paramValues.name[1]}<br>
${paramValues.name[2]}<br>
```

在测试上面这些代码时浏览器中填写的路径是

~~~
localhost:9000/cookie/example4.jsp?name=zhangsan&name=wangwu&name=lisi
~~~

## initParam

在 xml 文件中配置如下

```jsp
<context-param>
    <param-name>pageSize</param-name>
    <param-value>20</param-value>
</context-param>
<context-param>
    <param-name>pageNum</param-name>
    <param-value>5</param-value>
</context-param>
```

注意：一个 context-param 标签里面只能配置一个 param-name 标签和一个 param-value 标签，如果配置多个的话，我这边试了一下，好像只能读取到最后一个 pram-name 和 最后一个 param-value 中的信息。

在 JSP 中，内置对象 application 对应于 ServletContext ，application 对象就可以获取到 xml 中的配置信息

```jsp
<%=application.getInitParameter("pageSize")%>
<%=application.getInitParameter("pageNum")%>
```

而在 EL 表达式中可以通过 initParam 来获取这些配置信息

```jsp
${initParam.pageNum}
${initParam.pageSize}
```

# EL 表达式中的运算符

## 算术运算符

~~~
+ - * / %
~~~

示例

```jsp
${10 + 20}       <%-- 30 --%>
${10 + "20"}    <%-- 30 --%>
```

在 EL 表达式中 + 运算符只会做求和运算，不会做字符串拼接。

```jsp
${10 + "abc"}  
<%-- 500 报错 --%>
<%-- java.lang.NumberFormatException: For input string: "abc" --%>
```

## 关系运算符

~~~
== != > >= < <= eq gt lt
~~~

示例

```jsp
${"abc" == "abc"}	 <%-- true --%>
```

EL 表达式中的 == 实际上是调用了 equals 方法。eq 和 == 是等价的。

!= 运算符运行的时候也会去调用 equals 方法，就是将 equals 方法的结果非了一下而已。

## 逻辑运算符

~~~
! && || not and or
~~~

! 和 not 的作用是一样的。

## 条件运算符

~~~
? :
~~~

## 取值运算符

~~~
[] .
~~~

## empty 运算符

判断是否为空，空就是 true

```jsp
${empty param.name}
```