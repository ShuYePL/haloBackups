---
title: 使用source导入sql脚本路径错误（Windows平台）
date: 2022-07-06 21:30:11.48
updated: 2023-03-17 09:34:31.063
url: /archives/shi-yong-source-dao-ru-sql-jiao-ben-de-zhu-yi-shi-xiang
categories: 
- sql
tags: 
---

今天在使用source命令添加sql脚本的时候发现报错了
~~~
mysql> source D:\Users\jh\Desktop\t_student.sql;
ERROR:
Unknown command '\U'.
ERROR:
Unknown command '\j'.
ERROR:
Unknown command '\D'.
Outfile disabled.
ERROR:
Failed to open file 'D:\Users\jh\Desktop_student.sql', error: 2
~~~
这个错是Windows系统下的路径问题，将路径中的\改为/即可。