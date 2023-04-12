---
title: JSTL标签库.md
date: 2022-07-12 22:28:02.459
updated: 2022-07-12 22:28:31.494
url: /archives/jstl标签库md
categories: 
- JavaWeb
tags: 
---

# 什么是 JSTL 标签库

Java Standard Tag Lib （Java 标准的标签库）。

# 作用

结合 EL 表达式一起使用，让 JSP 中的 Java 代码消失。

# 使用步骤

## 引入 JSTL 标签库对应的 jar 包

Tomcat10 对应的 jar 包是 

- jakarta.servlet.jsp.jstl-2.0.0.jar
- jakarta.servlet.jsp.jstl-api-2.0.0.jar

引入 jar 包的方式是将其放到 WEB-INF/lib 目录下，然后右键 Add as Library 。

为什么要加入这些 jar 包：因为这些 jar 包是 tomcat 服务器没有的，程序的正确运行需要有这些 jar 包中的程序源码。

## 在 JSP 中引入到使用的标签库

引入的格式如下

```jsp
<%--引入标签库的方式，下面引入的是 JSTL 的核心库，uri 后面的就是核心库，prefix 后面跟着的是标签库的标志--%>
<%@taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
```

prefix 后面的内容可以随便写，但核心库 prefix 后面跟着的一般是 c

```jsp
<%--下面引入的是格式化标签库--%>
<%@taglib prefix="fmt" uri="http://java.sun.com/jsp/jstl/fmt" %>
<%--SQL标签库--%>
<%@taglib prefix="sql" uri="http://java.sun.com/jsp/jstl/sql" %>
```

在需要使用标签的位置使用标签

# JSTL 标签原理

我们引入标签的代码是 `<%@taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>` ，这个标签里面的 uti 后面的路径对应了一个 XXX.tld 文件，这个文件实际上是一个 xml 文件，在这个文件中描述了标签和 java 类之间的关系，这个文件就在我们引入的 jar 包里面。

tld 配置文件的源码解析

```
  <tag>
    <description>
	对该标签的描述信息。
    </description>
    <name>标签名</name>
    <tag-class>标签对应的 java 类</tag-class>
    <tei-class>org.apache.taglibs.standard.tei.ForEachTEI</tei-class>
    <body-content>
    标签体中可以出现的内容，如果在这里写的是 JSP ，就表示标签体中可以出现符合 JSP 所有语法的代码，如 EL 表达式，标签体指的是开始标签和结束标签中间的那个地方写的内容。
    </body-content>
    <attribute>
    	在 attribute 中出现的东西就是这个标签的属性，一个 attribute 标签里面配置的是一个属性。
        <description>
			对这个属性的描述。
        </description>
	<name>属性名</name>
	<required>
		在这个标签里面的值要么是 true ，要么是 false ，true 表示该属性是必须的，false 表示不是必须的。
	</required>
	<rtexprvalue>
		在这个标签里面的也是 true 或 false ，true 表示该属性支持 EL 表达式，false 表示不支持。
	</rtexprvalue>
	<type>java.lang.Object</type>
        <deferred-value>
	    <type>java.lang.Object</type>
        </deferred-value>
    </attribute>
  </tag>
```

# JSTL 标签的实际应用

先使用在 JSP 中嵌入 Java 代码的方式进行数据的遍历，这里因为没有用浏览器向服务器发送请求，所以，就将数据直接在 JSP 文件中以 Java 代码的方式绑定到 request 中。

```JSP
<%@ page import="com.bjpowernode.javaweb.bean.Student" %>
<%@ page import="java.util.List" %>
<%@ page import="java.util.ArrayList" %>
<%@ page contentType="text/html;charset=UTF-8" %>
<%
    Student stu1 = new Student("123", "zhangsan");
    Student stu2 = new Student("234", "wangwu");
    Student stu3 = new Student("345", "liuer");

    List<Student> stuList = new ArrayList<>();

    stuList.add(stu1);
    stuList.add(stu2);
    stuList.add(stu3);

    request.setAttribute("students", stuList);
%>

<%
    List<Student> students = (List<Student>)request.getAttribute("students");
    for (Student student : students) {
%>
    id:<%=student.getId()%> name:<%=student.getName()%><br>
<%
    }
%>
```

然后使用 JSTL 标签来取数据

```JSP
<%@ page import="java.util.ArrayList" %>
<%@ page import="com.bjpowernode.javaweb.bean.Student" %>
<%@ page import="java.util.List" %>
<%@page contentType="text/html;charset=UTF-8" %>
<%--引入标签库的方式，下面引入的是 JSTL 的核心库--%>
<%@taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<%--下面引入的是格式化标签库--%>
<%@taglib prefix="fmt" uri="http://java.sun.com/jsp/jstl/fmt" %>
<%--SQL标签库--%>
<%@taglib prefix="sql" uri="http://java.sun.com/jsp/jstl/sql" %>
<%
    Student stu1 = new Student("123", "zhangsan");
    Student stu2 = new Student("234", "wangwu");
    Student stu3 = new Student("345", "liuer");

    List<Student> stuList = new ArrayList<>();

    stuList.add(stu1);
    stuList.add(stu2);
    stuList.add(stu3);

    request.setAttribute("students", stuList);
%>

<%--items后面跟着的是遍历的对象，在var后面跟着的是表示集合每一个遍历对象的变量名--%>
<c:forEach items="${students}" var="s">
    id:${s.id},name:${s.name}<br>
</c:forEach>
```

就感觉，很 amazing 好吧。

# 核心库中常用的一些标签

## if 标签

三个属性

### text

必须的，支持 EL 表达式，它的值必须是 boolean 类型的。

示例

```jsp
<%@page contentType="text/html;charset=UTF-8" %>
<%@taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core"%>

<c:if test="${empty param.name}">
    用户名不能为空。
</c:if>

<c:if test="${not empty param.name}">
    Hello!${param.name}
</c:if>
```

### var

非必须的

### scope

指定 var 的存储域，非必须的，值有四个选择，对应四个域，分别是

- page
- request
- session
- application

```jsp
<%--下面这段代码表示 var 中的 v 绑定到 request 中去--%>
<c:if test="${empty param.name}" var="v" scope="request">
    用户名不能为空。
</c:if>
```

上面 var 属性后面跟着的 v 可以当做是一个变量，这个变量保存了 test 对应的值，scope 属性后面指定的域表示将该变量绑定到哪个域中。

## forEach

### var

指定变量名，这个变量是存储在 pageContext 域中的。

### begin

开始。

### end

结束。

### step

遍历时候的步长。

```jsp
<%--从0遍历到10--%>
<c:forEach var="i" begin="0" end="10" step="1">
    ${i}
</c:forEach>
```

### varStatus

这个属性表示 var 的状态对象，是一个 java 对象， 这个 java 对象代表了 var 的状态，属性后面的的值是随意的，相当于是这个对象的变量名。这个对象有一个 count 属性，可以查看遍历的次数，它从 1 开始，自动递增。

```jsp
<%--使用 count 可以查看遍历的次数--%>
<c:forEach items="${students}" var="s" varStatus="stuStatus">
     编号：${stuStatus.count}id:${s.id},name:${s.name}<br>
</c:forEach>
```

## choose

这个标签里面需要嵌套一个 when 标签，相当于 if ... else ...

```jsp
<c:choose>
  <c:when test="${param.age < 18}">
    青少年
  </c:when>
  <c:when test="${param.age < 35}">
    青年
  </c:when>
  <c:when test="${param.age < 55}">
    中年
  </c:when>
  <c:otherwise>
    老年
  </c:otherwise>
</c:choose>
```