---
title: IDEA创建空工程会自动携带同名模块的解决方法
date: 2022-08-31 11:42:55.653
updated: 2022-08-31 14:35:57.3
url: /archives/idea-chuang-jian-kong-gong-cheng-hui-zi-dong-xie-dai-tong-ming-mo-kuai-de-jie-jue-fang-fa
categories: 
- IDEA配置
tags: 
---

# 前言

IDEA 版本是 2022.1.1 ，在创建空共工程的时候老是会在工程里面自动携带一个和工程同名的模块。

![202208001](http://img.shuyepl.com/202208311141955.png)

# 解决方法

将同名模块的 .iml 文件直接删除就可以了，然后需要注意的是在创建新模块的时候，模块的本地地址要写好。

![202208002](http://img.shuyepl.com/202208311141761.png)