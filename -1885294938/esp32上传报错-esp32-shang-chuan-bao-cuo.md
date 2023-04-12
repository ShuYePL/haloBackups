---
title: esp32上传报错
date: 2022-07-18 21:22:45.527
updated: 2022-07-18 21:22:45.527
url: /archives/esp32-shang-chuan-bao-cuo
categories: 
tags: 
---

最近因为要用到单片机来检测电压，打算使用 esp32 来进行这个检测，今天下了 esp32 的资料，看了 arduino 开发的视频教程，跟着试了一下例程，发现一直不能上传成功，报错内容

~~~
项目使用了 279907 字节，占用了 (21%) 程序存储空间。最大为 1310720 字节。
全局变量使用了26820字节，(9%)的动态内存，余留268092字节局部变量。最大为294912字节。
esptool.py v2.1-beta1
Connecting........_____....._____....._____....._____....._____....._____....._____....._____....._____.....____上传项目出错
_

A fatal error occurred: Failed to connect to ESP32: Timed out waiting for packet header
~~~

解决方法：当下方的信息栏出现 Connecting 的时候按住 boot 键，等待连接完成，进行下一步上传程序。

