---
title: 51单片机-_nop_报错
date: 2022-11-05 12:35:19.373
updated: 2022-11-05 12:35:19.373
url: /archives/51-dan-pian-ji--nop-bao-cuo
categories: 
- 51单片机
tags: 
---

# 报错信息

` '_nop_':missing function-prototype `

# 报错原因

在使用定时函数的时候使用到了 ` _nop ` 函数，但是没有引入相应的包。

# 解决方法

文件头部加入 ` #include<intrins.h> `



---

参考资料：

- [_nop_()函数_边道的博客-CSDN博客__nop_](https://blog.csdn.net/bdhk6688/article/details/8612860)