---
title: mysql 的 ResultSet 使用注意事项
date: 2022-06-28 20:29:42.908
updated: 2022-09-01 08:23:03.485
url: /archives/mysql的resultset使用注意事项
categories: 
- JavaWeb
tags: 
---

异常如下
>java.sql.SQLException: Before start of result set

发生的问题是，我在使用 ResultSet 的对象前没有进行使用 rs.next() ，就直接使用 rs.getString() 了，所以一定要记住，在使用 rs.getString() 之前一定要先使用 rs.next()