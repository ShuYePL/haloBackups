---
title: sql- 报错：Public Key Retrieval is not allowed
date: 2022-11-25 18:50:35.53
updated: 2022-11-25 18:50:35.53
url: /archives/sql--bao-cuo-publickeyretrievalisnotallowed
categories: 
- sql
tags: 
---

# 前言

今天不知道怎么回事的，启动一个 crm 项目的时候就发生了 Public Key Retrieval is not allowed 的这个报错，之前明明都运行得好好的。

# 解决方法

在配置数据库的 xml 文件里面，数据库地址配置的后面加上 allowPublicKeyRetrieval=true 就好了。

---

参考资料：

- [(4条消息) 连接mysql时报错Public Key Retrieval is not allowed的解决方法_往事不堪回首..的博客-CSDN博客](https://blog.csdn.net/qq3892997/article/details/122570788)