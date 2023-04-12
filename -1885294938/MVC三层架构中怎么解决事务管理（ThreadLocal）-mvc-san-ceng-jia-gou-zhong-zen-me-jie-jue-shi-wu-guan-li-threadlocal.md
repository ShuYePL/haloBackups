---
title: MVC三层架构中怎么解决事务管理（ThreadLocal）
date: 2023-03-24 22:45:31.145
updated: 2023-03-24 22:45:31.145
url: /archives/mvc-san-ceng-jia-gou-zhong-zen-me-jie-jue-shi-wu-guan-li-threadlocal
categories: 
- mybatis
tags: 
---

# MVC三层架构的事务管理

在编写MVC三层架构的Demo时发现，SqlSession的获取是在Dao层的，每一个sql语句的执行都对应着一个SqlSession，而我们在Service层调用Dao层的多个接口时，由于SqlSession不同就会导致一些事务方面的问题，比如在Service层有几Dao个接口的调用需要开启事务的，但是以为使用了不同的SqlSession导致每个接口调用都是单独的事务，这样就会发生错误，这个时候就需要ThreadLocal来帮忙了，ThreadLocal对象是服务器级别的，一个服务器就一个，我们将当前线程的SqlSession绑定到ThreadLocal中，要使用的时候再从里面取出来，这样就能保证几个Dao接口调用的都是同一个SqlSession对象了。

# ThreadLocal是什么

---

参考资料：

- [057-在WEB应用中使用MyBatis之事务的控制_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1JP4y1Z73S?p=57&vd_source=3b9bc9314c9590a1d18a84ef490fb982)