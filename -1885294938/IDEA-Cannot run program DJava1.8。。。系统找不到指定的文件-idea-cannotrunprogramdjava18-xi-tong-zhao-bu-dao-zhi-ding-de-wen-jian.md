---
title: IDEA-Cannot run program DJava1.8。。。系统找不到指定的文件
date: 2022-11-19 18:33:31.896
updated: 2022-11-19 18:33:31.896
url: /archives/idea-cannotrunprogramdjava18-xi-tong-zhao-bu-dao-zhi-ding-de-wen-jian
categories: 
- IDEA配置
tags: 
---

# 前言

前两天把 D 盘清空和 C 盘合并了，而之前的 Java 安装在 D 盘中，今天运行 Mava 逆向工程时出现了一点小错误。

![202211005](http://img.shuyepl.com/202211191830502.png)

# 解决方法

打开 Project Structure --> SDKS 将之前失效的 SDK 删除，修改成新的 SDK 位置，然后出现了这个错误，可以从描述里面看到是 Maven 的 Runner 配置有问题

![202211006](http://img.shuyepl.com/202211191830636.png)

File --> Settings --> Build, Execution, Deployment --> Build Tools --> Maven --> Runner ， 点开进去 JRE 那里爆红了，修改一下就好了。

