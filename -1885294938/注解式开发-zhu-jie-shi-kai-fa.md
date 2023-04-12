---
title: 注解式开发
date: 2022-06-28 23:23:47.259
updated: 2022-09-01 08:22:43.401
url: /archives/zhu-jie-shi-kai-fa
categories: 
- JavaWeb
tags: 
---

# 注解式开发

每次向 WEB 程序中添加 Servelet 时，都要在 web.xml 文件中添加相应的 servlet 属性，这样一来的话，整个 web.xml 文件很臃肿，而且每次都要添加，对于我们开发来讲也很不方便，所以，解决方法就是注解式开发，直接在类上面使用注解进行标注就可以了

# 注解式开发的优点

- 开发效率高，不需要编写大量的配置信息，直接在类上面使用注解进行标注就可以了，一些不需要经常变化的配置信息可以使用注解
- web.xml 文件的体积变小了

注意：这个开发方式有优点，但并不是说就不需要 web.xml 的配置文件了，一些需要发生变化的信息，还是要配置在 web.xml 文件中的，在开发的过程中可能会使用 web.xml 和注解式开发一起使用的联合开发模式

#  相关的注解及其作用

注解的完整包名是在注解名的前面加上 jakarta.servlet.annotation.

## @WebServlet

### 作用

表明这是一个 Servlet 类

### 允许标注位置

类

### 注解的属性

- name：Servlet 类的名字，相当于 web.xml 文件内 \<servlet-name\> 标签配置的信息，是一个字符串，非必须

  可以通过 HttpServlet 的 getServletName() 方法获取到

- urlPatterns：指定 Servlet 的映射路径，相当于 web.xml 文件中 \<url-pattern\> 标签配置的信息，是一个字符串数组

  可以通过 HttpServlet 的 getServletPath() 方法获取，这个是你通过哪一个路径的地址去访问它，就会获得哪个路径

- loadOnStartup：设置服务器启动时创建 Servlet 类对象，相当于 web.xml 文件中 \<load-on-startup\> 配置的信息，是一个 int 类型数据

- iniParams：是一个 WebInitParam 数组

  WebInitParam 是一个注解，相当于 \<init-param\> 标签，这个注解的属性有

  - name：字符串类型，相当于 \<param-name\> 标签
  - value：字符串类型，相当于 \<param-value\> 标签
  - description：字符串类型，非必须

  可以通过 HttpServlet 的 getInitParameterNames() 方法获取初始化参数

  - ~~~java
    response.setContentType("text/html");
    PrintWriter out = response.getWriter();
    Enumeration<String> names = getInitParameterNames();
    while(names.hasMoreElements()){
        String name = names.nextElement();
        String value = getInitParameter(name);
        out.print(name + value + "<br>");
    }
    ~~~

- value：这个和 urlPatterns 一模一样，唯一的区别就是，如果注解的属性名是 value ，是可以省略的，直接写上对应的值就可以了
  
注意：如果注解的属性是一个数组，并且数组中只有一个元素时，大括号可以省略
  
---
参考资料：https://www.bilibili.com/video/BV1Z3411C7NZ?p=42&share_source=copy_web