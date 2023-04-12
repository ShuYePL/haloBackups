---
title: ServletConfig 是什么
date: 2022-06-04 21:52:00.0
updated: 2022-07-04 20:16:49.372
url: /archives/servletconfig是什么
categories: 
- JavaWeb
tags: 
---



#### 内容简介：ServletConfig 的简单介绍，以及相关方法的介绍

<!--more-->

ServletConfig 是一个接口，完整接口名：jakarta.servlet.ServletConfig，所以这是 Servlet 规范中的一员；这个接口由 WEB 服务器来实现； 不同的 Servlet 对象会对应不同的 ServletConfig 对象（这里说的 ServletConfig 对象指的是实现了ServletConfig 接口的对象，不要以为接口可以创建对象）；Servlet 对象是又 WEB 服务器创建的，创建的时间是在创建 Servlet 对象的时候，同时创建了 ServletConfig 对象；ServletConfig 对象里面包装了 Servlet 对象的配置信息，这里面的信息是 web.xml 文件中 \<servlet>\</servlet> 标签中的内容，所以我们可以在这个标签内配置 Servlet 对象的初始信息，配置的方式是在 \<servlet>\</servlet> 中添加一个 \<ini-param>\<param-param> 标签，具体的代码示例如下：

~~~xml
<servlet>
    <servlet-name>config1</servlet-name>
    <servlet-class>com.bjpowernode.javaweb.ConfigTestServlet</servlet-class>
    <init-param>
        <param-name>driver</param-name>
        <param-value>com.mysql.cj.jdbc.Driver</param-value>
    </init-param>
    <init-param>
        <param-name>url</param-name>
        <param-value>jdbc:mysql://localhost:3306/bjpowernode</param-value>
    </init-param>
    <init-param>
        <param-name>user</param-name>
        <param-value>root</param-value>
    </init-param>
    <init-param>
        <param-name>password</param-name>
        <param-value>123456</param-value>
    </init-param>
</servlet>
~~~

在 param-name 标签中书写的是参数的名称，在 param-value 标签中书写的是参数的值，这些信息在 WEB 服务器启动的时候都会被封装进与对应 Servlet 对象相匹配的 ServletConfig 对象中，这样，在程序中我们就可以通过 ServletConfig 对象来获取这些配置信息了，获取的程序例程如下

~~~java
package com.bjpowernode.javaweb;

import jakarta.servlet.*;
import java.io.IOException;
import java.io.PrintWriter;
import java.util.Enumeration;

public class ConfigTestServlet extends GenericServlet {
    @Override
    public void service(ServletRequest request, ServletResponse response) throws ServletException, IOException {
        response.setContentType("text/html");
        PrintWriter out = response.getWriter();
        // 获取 ServletConfig 对象
        ServletConfig config = this.getServletConfig();     // 这个是因为继承了 GenericServlet 类，所以会有一个 ServletConfig 对象的属性
        // 获取 ServletConfig 对象的参数集合
        Enumeration<String> initParameterNames = config.getInitParameterNames();
        // 遍历集合输出参数的名称和它的值
        while(initParameterNames.hasMoreElements()){
            String parameterName = initParameterNames.nextElement();        // 得到参数的名字
            out.print(parameterName);
            out.print(" = ");
            out.print(config.getInitParameter(parameterName));              // 通过参数的名字获取参数的值并打印
            out.print("<br>");
        }
    }
}
~~~

这里面涉及到 ServletConfig 的两个方法，第一个是 getInitParameterNames() ，它可以返回一个遍历对象，遍历的结果是输出我们在 xml 文件中给这个 Servlet 对象配置的参数名称，第二个方法是 getInitParameter(String parameterName) ，这个可以通过传入参数的名称来得到参数的值

在浏览器中的输出结果如下

~~~txt
password = 123456
driver = com.mysql.cj.jdbc.Driver
user = root
url = jdbc:mysql://localhost:3306/bjpowernode
~~~

虽然上面讲了两个方法的使用，但在平时开发的时候我们基本上可能不会使用上面的方法去获取迭代对象，因为我们写的类是继承 GrnericServlet 类的，而 GenericServlet 类中已经为我们编写好了获取参数信息的方法，这两个方法就是使用和我们上面的代码一样的方式去获取配置信息的，所以我们可以直接通过调用这两个方法来获取我们想要的信息就可以了，使用方式如下：

~~~java
package com.bjpowernode.javaweb;
import jakarta.servlet.*;
import java.io.IOException;
import java.io.PrintWriter;
import java.util.Enumeration;

public class ConfigTestServlet extends GenericServlet {

    @Override
    public void service(ServletRequest request, ServletResponse response) throws ServletException, IOException {
        response.setContentType("text/html");
        PrintWriter out = response.getWriter();
        // 获取迭代对象
        Enumeration<String> names = this.getInitParameterNames();
        // 对迭代对象进行迭代输出
        while(names.hasMoreElements()){
            String name = names.nextElement();
            String value = this.getInitParameter(name);
            out.print(name + " = " + value);
            out.print("<br>");
        }
    }
}
~~~

输出的结果为：

~~~txt
password = 123456
driver = com.mysql.cj.jdbc.Driver
user = root
url = jdbc:mysql://localhost:3306/bjpowernode
~~~

~~经典白学了属于是 O(∩_∩)O哈哈~~~

应该能听得懂上面是开玩笑的意思吧

---

使用 ServletConfig 对象的 getServletName 方法可以获取 Servlet 的名字

---

接下来讲的是 ServletContext 对象

首先是通过 ServletConfig 对象获取 ServletContext 对象

~~~java
package com.bjpowernode.javaweb;
import jakarta.servlet.*;
import java.io.IOException;
import java.io.PrintWriter;
import java.util.Enumeration;

public class ConfigTestServlet extends GenericServlet {

    @Override
    public void service(ServletRequest request, ServletResponse response) throws ServletException, IOException {
        response.setContentType("text/html");
        PrintWriter out = response.getWriter();

        // 获取 ServletConfig 对象
        ServletConfig config = this.getServletConfig();     // 这个是因为继承了 GenericServlet 类，所以会有一个 ServletConfig 对象的属性
        // 通过 ServletConfig 对象获取 ServletContext 对象
        ServletContext application = config.getServletContext();
        // 将得到的对象输出，看下结果是什么
        out.print(application);
    }
}
~~~

输出结果如下：

~~~txt
org.apache.catalina.core.ApplicationContextFacade@6fd1dd80
~~~

同样的，和上面的获取 Servlet 对象参数的方式一样，GenericServlet 类中已经封装好了，直接使用就可以获取这个 ServletContext 对象了

~~~java
package com.bjpowernode.javaweb;
import jakarta.servlet.*;
import java.io.IOException;
import java.io.PrintWriter;
import java.util.Enumeration;

public class ConfigTestServlet extends GenericServlet {

    @Override
    public void service(ServletRequest request, ServletResponse response) throws ServletException, IOException {
        response.setContentType("text/html");
        PrintWriter out = response.getWriter();

        // 获取 ServletConfig 对象
        ServletConfig config = this.getServletConfig();     // 这个是因为继承了 GenericServlet 类，所以会有一个 ServletConfig 对象的属性

        ServletContext application = this.getServletContext();
        out.print(application);
    }
}
~~~

输出结果如下：

~~~txt
org.apache.catalina.core.ApplicationContextFacade@6b23e3c1
~~~

---

总结：ServletConfig 接口一共有四个方法

- public String getInitParameter(String name);
- public Enumeration<String> getInitParameterNames();
- public ServletContext getServletContext();
- public String getServletName();

因为我们编写的 Servlet 类一般是继承 GenericServlet 类的，这几个方法在 GenericServlet 类中都有同名的方法，所以我们可以直接在自己编写的类中使用 this. 加上面的方法名的方式进行调用，获取的结果都是一样的

---

参考资料：https://www.bilibili.com/video/BV1Z3411C7NZ?p=12&share_source=copy_web