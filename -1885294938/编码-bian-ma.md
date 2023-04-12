---
title: 编码
date: 2023-04-12 09:40:51.385
updated: 2023-04-12 09:40:51.385
url: /archives/bian-ma
categories: 
tags: 
---

# ASCII

美国信息交换标准码。使用一个字节存储字符，首部是0，可表示128个字符。

# GBK

汉子内码扩展规范，国标。汉字编码字符集，一个中文字符编码成两个字节的形式存储，GBK兼容ASCII字符集，规定字符编码的第一位必须是1，和ASCII区分开。

# Unicode

统一码，万国码。国际组织决定的，几乎可以容纳全世界的文字，符号。

## UTF-8

Unicode字符集的一种编码方案，可变长编码，分为四个长度区：一个字节、两个字节、三个字节、四个字节，英文字符、数字占1个字节（兼容ASCII），汉字占3个字节，编码规则如下，红色的部分是固定编码。

![QQ截图20230412092843](https://img.shuyepl.com/202304120929497.png)

果然直观的图片，比书里一堆的文字更容易理解。

---

参考资料：

- [一听就懂字符集、ASCII、GBK、UTF-8、Unicode、乱码、字符编码、解码问题的讲解_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1xD4y1y7yc/?spm_id_from=333.337.search-card.all.click&vd_source=3b9bc9314c9590a1d18a84ef490fb982)