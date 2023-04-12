---
title: Tomcat 环境配置
date: 2022-05-03 22:37:00.0
updated: 2022-07-04 20:18:07.47
url: /archives/tomcat环境配置
categories: 
- JavaWeb
tags: 
---



#### 内容简介：Tomcat 要真正使用起来之前要配置的环境变量还挺多的，记录一下，以后忘了，可以回来看看。

<!--more-->

# 环境变量配置位置的打开方式

WIN10 的情况是 在 “计算机” 图标上右键点击属性

![202205001](http://img.shuyepl.com/202207042011034.jpg)

滚动鼠标滚轮向下找到 “高级系统设置” 选项，鼠标左键点击

![202205002](http://img.shuyepl.com/202207042011267.jpg)

找到属性面板右下角的 “环境变量” 选项，再次左键点击，然后我们就到的环境变量的配置页面了

![202205003](http://img.shuyepl.com/202207042011114.jpg)

首先需要配置的是 Tomcat 的根目录和 Java JDK 根目录

配置的名字分别是 CATALINA_HOME 和 JAVA_HOME

![202205004](http://img.shuyepl.com/202207042011105.jpg)

之后在 path 变量中配置 Java 和 Tomcat 的 bin 目录

![202205005](http://img.shuyepl.com/202207042012299.jpg)

最后，在 classpath 变量中配置 Tomcat 的 jar 包位置

![202205006](http://img.shuyepl.com/202207042012717.jpg)

---

参考教程：[动力节点最新JavaWeb视频教程,javaweb零基础入门到精通IDEA版-【已完结】_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1Z3411C7NZ?p=10&spm_id_from=333.999.header_right.history_list.click)