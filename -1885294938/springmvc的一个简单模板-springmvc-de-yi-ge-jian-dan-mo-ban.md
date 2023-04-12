---
title: springmvc的一个简单模板
date: 2023-03-24 22:33:49.502
updated: 2023-03-24 22:33:49.502
url: /archives/springmvc-de-yi-ge-jian-dan-mo-ban
categories: 
- SpringMVC
tags: 
---

1. 首先，IDEA 里面创建一个 maven 项目，选择 maven-archetype-webapp 模板。

2. 补充模板缺失的文件，补充之后的文件结构如下

    ![202209003](https://img.shuyepl.com/202209302119466.png)

    其中，web.xml 文件和 index.jsp 文件在原本的模板中就有，但是，有点老了，删掉重建。

    spirngmvc.xml 文件的创建使用已有的模板，创建方式如下：

    ![202209004](https://img.shuyepl.com/202209302120936.png)

3. 接下来配置三个主要文件的内容，这三个文件分别是 springmvc.xml、pom.xml、web.xml
    这三个文件主要配置的内容如下：
    pom.xml ：配置依赖、配置资源文件的路径。
    主要配置两个依赖：

    ~~~xml
    <!-- 添加springmvc框架的依赖 -->
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-webmvc</artifactId>
        <version>5.2.5.RELEASE</version>
    </dependency>
    <!-- 添加tomcat servlet的依赖 -->
    <dependency>
        <groupId>javax.servlet</groupId>
        <artifactId>javax.servlet-api</artifactId>
        <version>3.1.0</version>
    </dependency>
    ~~~

    资源文件的配置：

    ~~~xml
    <build>
        <resources>
            <resource>
                <directory>src/main/java</directory>
                <includes>
                    <include>**/*.xml</include>
                    <include>**/*.xml</include>
                </includes>
            </resource>
            <resource>
                <directory>src/main/resources</directory>
                <includes>
                    <include>**/*.xml</include>
                    <include>**/*.xml</include>
                </includes>
            </resource>
        </resources>
    </build>
    ~~~

    web.xml ：解决中文乱码问题、注册 springmvc 。

    解决中文乱码：

    ~~~xml
    <!-- 添加中文编码 -->
    <filter>
        <filter-name>encode</filter-name>
        <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
        <!-- 设置编码格式 -->
        <init-param>
            <param-name>encoding</param-name>
            <param-value>UTF-8</param-value>
        </init-param>
        <!-- 设置强制请求编码 -->
        <init-param>
            <param-name>forceRequestEncoding</param-name>
            <param-value>true</param-value>
        </init-param>
        <!-- 设置强制响应编码 -->
        <init-param>
            <param-name>forceResponseEncoding</param-name>
            <param-value>true</param-value>
        </init-param>
    </filter>
    <!-- 设置拦截的请求 -->
    <filter-mapping>
        <filter-name>encode</filter-name>
        <!-- 拦截所有的请求 -->
        <url-pattern>/*</url-pattern>
    </filter-mapping>
    ~~~

    注册 springmvc ：

    ~~~xml
    <servlet>
        <servlet-name>springmvc</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
        <init-param>
            <param-name>contextConfigLocation</param-name>
            <param-value>classpath:springmvc.xml</param-value>
        </init-param>
    </servlet>
    <servlet-mapping>
        <servlet-name>springmvc</servlet-name>
        <url-pattern>*.action</url-pattern>
    </servlet-mapping>
    ~~~

    spring.xml ：添加包扫描、添加视图解析器。

    包扫描：

    ~~~xml
    <!--  添加包扫描  -->
    <context:component-scan base-package="com.shuyepl.controller"></context:component-scan>
    ~~~

    这个注意要在 beans 标签中添加如下的包 ` xmlns:context="http://www.springframework.org/schema/context" `

    添加视图解析器：主要就是添加文件的前缀和后缀，方便视图解析器后期自动帮我们填充这一部分的内容。

    ~~~xml
    <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
        <property name="prefix" value="/admin/"></property>
        <property name="suffix" value=".jsp"></property>
    </bean>
    ~~~

4. 在相应的包下编写 Java 程序，这些程序很多都需要使用到注解。
    在 Controller 包下的程序，类上要使用 @Controller 注解，类中的方法上要使用 @RequestMapping 注解，后者的属性可以指定请求的路径和请求的方式：` @RequestMapping(value = "/req", method = RequestMethod.POST) `

一个简单的 springmvc 工程结构就是这样的了。

---

接下来，如果想使用 springmvc 处理 ajax 请求，设置更改如下

1. pom.xml 文件中添加 jackson 的依赖

    ~~~xml
    <!--  添加jackson依赖  -->
    <dependency>
        <groupId>com.fasterxml.jackson.core</groupId>
        <artifactId>jackson-databind</artifactId>
        <version>2.9.8</version>
    </dependency>
    ~~~

2. web.xml 文件中不需要添加视图解析器，添加注解驱动

    ~~~xml
    <!-- 添加注解驱动，专门用来处理ajax请求的 -->
    <mvc:annotation-driven></mvc:annotation-driven>
    ~~~

    注意：这个注解驱动的命名空间末尾是 mvc 的，注意别导错了。

3. 添加 jq 的依赖库：在 webapp 文件下添加一个 jq 文件夹，将 jQuery 的 js 文件添加到里面去

4. 编写服务器端的 Controller 程序：添加 @RequestMapping 注释、添加 @ResponseBody 注释

    后面的那个注释是用来解析 ajax 请求的，添加了这个注释之后，springmvc 会将服务器端要返回的集合自动转换为 json 数组。

5. 接下来的就和前面的一样了，编写 Java 程序，类上写上 @Controller 注释，方法上面写上 @RequestMapping  ，这些与前面一样，然后是不一样的地方，方法上面要写上 **@ResponseBody** 这个注解。

---

参考资料：

- 【动力节点SpringMVC框架教程2022最新版_四天快速搞定SpringMVC框架】 https://www.bilibili.com/video/BV1oP4y1K7QT?p=27&share_source=copy_web&vd_source=ab2c5095571010032601fa9e036d004a