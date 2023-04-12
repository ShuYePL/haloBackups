---
title: MyBatis引入logback框架
date: 2023-03-24 14:20:51.628
updated: 2023-03-24 14:20:51.628
url: /archives/mybatis-yin-ru-logback-kuang-jia
categories: 
- mybatis
tags: 
---

引入框架依赖

```xml
<dependency>
    <groupId>ch.qos.logback</groupId>
    <artifactId>logback-classic</artifactId>
    <version>1.2.11</version>
</dependency>
```

配置MyBatis的配置文件

```xml
<settings>
    <setting name="logImpl" value="SLF4J"/>
</settings>
```

加入logback.xml文件

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration debug="false">
    <!-- 控制台输出 -->
    <appender name="STDOUT" class="ch.qos.logback.core.ConsoleAppender">
        <encoder class="ch.qos.logback.classic.encoder.PatternLayoutEncoder">
            <!--格式化输出：%d表示日期，%thread表示线程名，%-5level：级别从左显示5个字符宽度%msg：日志消息，%n是换行符-->
            <pattern>[%thread] %-5level %logger{50} - %msg%n</pattern>
        </encoder>
    </appender>
    <!--mybatis log configure-->
    <logger name="com.apache.ibatis" level="TRACE"/>
    <logger name="java.sql.Connection" level="DEBUG"/>
    <logger name="java.sql.Statement" level="DEBUG"/>
    <logger name="java.sql.PreparedStatement" level="DEBUG"/>
    <!-- 日志输出级别,logback日志级别包括五个：TRACE < DEBUG < INFO < WARN < ERROR -->
    <root level="DEBUG">
        <appender-ref ref="STDOUT"/>
        <appender-ref ref="FILE"/>
    </root>
</configuration>
```

然后随便测试一下就可以了，但是，记得，xxxMapper的namespace千万别乱写，有可能会出错的。

---

参考资料：

- 【【动力节点】SSM框架之MyBatis上线即经典，跟老杜从零学mybatis 入门到架构思维】 https://www.bilibili.com/video/BV1JP4y1Z73S/?p=74&share_source=copy_web&vd_source=ab2c5095571010032601fa9e036d004a