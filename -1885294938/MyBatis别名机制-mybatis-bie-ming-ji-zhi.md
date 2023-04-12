---
title: MyBatis别名机制
date: 2023-03-25 15:07:07.334
updated: 2023-03-25 15:07:07.334
url: /archives/mybatis-bie-ming-ji-zhi
categories: 
- mybatis
tags: 
---

# 别名TypeAlias

在Mapper里面写返回值类型要写全限定名太长了，给这些全限定名起一个简短的名字，别名在MyBatis的配置文件里面配置，有两种方式。

# 配置别名

第一种：单个配置

```xml
<typeAliases>
    <!--type是全限定名，alias是别名-->
    <typeAlias type="com.shuyepl.bank.pojo.Account" alias="account" />
</typeAliases>

或者

<typeAliases>
    <!--type是全限定名，省略了alias之后别名就是类的名字（不区分大小写）-->
    <typeAlias type="com.shuyepl.bank.pojo.Account" />
</typeAliases>
```

第二种：包扫描自动配置

```xml
<typeAliases>
    <package name="com.shuyepl.bank.pojo"/>
</typeAliases>
```

指定包名之后包下的类自动起别名。

tips：

- 别名在用的时候不区分大小写。
- namespace在配置的时候必须写全限定名，不能使用别名。

---

参考资料：

- [072-MyBatis小技巧之别名机制_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1JP4y1Z73S?p=72&spm_id_from=pageDriver&vd_source=3b9bc9314c9590a1d18a84ef490fb982)