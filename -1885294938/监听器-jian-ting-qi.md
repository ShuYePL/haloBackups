---
title: 监听器
date: 2022-07-18 21:14:07.945
updated: 2022-07-18 21:14:07.945
url: /archives/jian-ting-qi
categories: 
- JavaWeb
tags: 
---

# 什么是监听器

监听器是 Servlet 规范中的一员，在 Servlet 中，所有监听器接口都是以 Listener 结尾的。

# 监听器的作用

监听器是 Servlet 规范给我们准备的一个特殊时机，如果我们想在一个特定的时间点执行某一段代码的话，就使用相应的监听器。

# 有哪些监听器

## Jakarta.servlet 包下

ServeletContextListener

ServletContextAttributeListener

ServletRequestListener

ServletRequestAttributeListener

## jakarta.servlet.http 包下

HttpSessionListener

HttpSessionAttributeListener

HttpSessionBindingListener

HttpSessionldListener

HttpSessionActivationListener

注意：包含三个域名称但是里面没有 Attribute 的接口，里面实现的方法是这个三个域对象创建和销毁时会执行的方法，而包含这三个域的名字并且里面有 Attribute 的接口，里面的方法执行的时间则是对应于向域中存储数据的时候、存储的数据被删除的时候和存储的数据被替换的时候。

后面三个接口，首先是 HttpSessionBindingListener 接口，只有将实现这个接口的类，往 session 域中绑定或者解绑的时候才能被监听到（不需要写注解），HttpSessionldListener 监听 sessionid 的变化，只要 sessionid 改变了，就能被监听到，HttpSessionActivationListener 是用来监听 session 对象的钝化和活化的。

PS：钝化和活化是什么？

- 钝化：session 对象从内存存储到硬盘文件
- 活化：session 对象从硬盘文件恢复到内存

# 如何实现监听器

## 编写一个类实现监听器接口，实现里面的方法

```java
package com.bjpowernode.javaweb.listener;

import jakarta.servlet.ServletContextEvent;
import jakarta.servlet.ServletContextListener;

public class MyServletContextListener implements ServletContextListener {
    @Override
    public void contextInitialized(ServletContextEvent sce) {
    }

    @Override
    public void contextDestroyed(ServletContextEvent sce) {
    }
}
```

在里面实现的方法不需要我们手动去调用，当某个特定的事件发生之后，服务器会自动调用。前一个方法是在 ServletContext 对象被创建的时候调用，后面的方法是在 ServeletContext 对象被销毁时调用。

## 在 web.xml 文件中对这个文件进行配置或者使用注解

### xml 文件配置的方式

```xml
<listener>
    <listener-class>com.bjpowernode.javaweb.listener.MyServletContextListener</listener-class>
</listener>
```

### 注解的方式

在类的上面加上以下的这个注解

```java
@WebListener
```

---

参考资料：https://www.bilibili.com/video/BV1Z3411C7NZ?share_source=copy_web&vd_source=ab2c5095571010032601fa9e036d004a