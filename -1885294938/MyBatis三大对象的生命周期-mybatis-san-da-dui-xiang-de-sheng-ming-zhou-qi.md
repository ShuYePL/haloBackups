---
title: MyBatis三大对象的生命周期
date: 2023-03-26 12:01:10.589
updated: 2023-03-26 12:01:10.589
url: /archives/mybatis-san-da-dui-xiang-de-sheng-ming-zhou-qi
categories: 
- mybatis
tags: 
---

SqlSessionFactoryBuilder的作用在创建了SqlSessionFactory之后就可以丢弃了，控制它的生命周期为方法作用域就可以了。

SqlSessionFactory在程序运行期间一直存在，一个数据库就对应一个SqlSessionFactory对象。

SqlSession不是线程安全的，一个线程一个就可以了。

---

参考资料：

- 【【动力节点】SSM框架之MyBatis上线即经典，跟老杜从零学mybatis 入门到架构思维】 https://www.bilibili.com/video/BV1JP4y1Z73S/?p=58&share_source=copy_web&vd_source=ab2c5095571010032601fa9e036d004a

