---
title: ThreadLocal
date: 2023-03-26 11:58:45.3
updated: 2023-03-26 11:58:45.3
url: /archives/threadlocal
categories: 
- Javase
tags: 
---

# MVC三层架构的事务管理

在编写MVC三层架构的Demo时发现，SqlSession的获取是在Dao层的，每一个sql语句的执行都对应着一个SqlSession，而我们在Service层调用Dao层的多个接口时，由于SqlSession不同就会导致一些事务方面的问题，比如在Service层有几个Dao接口的调用需要开启事务的，但是因为使用了不同的SqlSession导致每个接口调用都是单独的事务，这样就会发生错误，这个时候就需要ThreadLocal来帮忙了，ThreadLocal对象是服务器级别的，一个服务器就一个，我们将当前线程的SqlSession绑定到ThreadLocal中，要使用的时候再从里面取出来，这样就能保证几个Dao接口调用的都是同一个SqlSession对象了。

# ThreadLocal是什么

在ThreadLocal中放置的变量可以认为是专属于当前线程的变量，其他线程没有访问到这个变量的可能。

ThreadLocal的作用

- 对象跨层传递时，打破层次间的约束
- 线程间数据隔离
- 进行事务操作，用于存储线程事务信息
- 数据库连接，Session管理
- 包含线程不安全的对象，实现线程安全

ThreadLocal在底层进行数据存储的时候使用的是ThreadLocalMap，一个静态内部类，这个静态内部类到时候创建出对象的话是Thread持有的，就是说在一个线程中持有ThreadLocalMap类的对象，而这个ThreadLcalMap底层在存储的时候使用了Entry数组，这个数组的key是ThreadLocal对象，而值则是我们设置的，所以这样我们就可以在一个线程中创建多个ThreadLocal对象，然后每个对象里面存储一个值，达到存储多个值的目的。

tips：在使用完ThreadLocal中的值，不需要再次使用的时候，记得调用它的remove()方法，清楚一下里面保存的内容，避免内存泄露。（如果是使用在由线程池的地方，不清除这个值也会发生业务方面的问题，如我上面提到的SqlSession，如果不清除的话，到时候从线程池里面取出线程来用，结果SqlSession是前面的事务用的，这不得出错了。）

# ThreadLocal内存泄露

现在整理一下思路，Thread对象会持有一个ThreadLocalMap对象，这个对象里面存储了一个Entry数组，这个数组的key是ThreadLocal对象，value是我们自己设置的值，所以，如果ThreadLocal被回收了，那么Entry数组的key就变成null，但是这个时候value并不会被清除，如果不对这个value进行处理的话，就会造成内存泄露。

对value进行清理的方式有两种，一种是上面提到的remove方法，手动清理无用的对象，比较推荐，另外一种是在调用get()、set()、remove()方法的时候会自动清理key为null的对象。

---

参考资料：

- [057-在WEB应用中使用MyBatis之事务的控制_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1JP4y1Z73S?p=57&vd_source=3b9bc9314c9590a1d18a84ef490fb982)
- [ThreadLocal，一篇文章就够了 - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/102744180)
- [ThreadLocal是什么？怎么用？为什么用它？有什么缺点？ - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/192997550)