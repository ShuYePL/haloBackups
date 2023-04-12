---
title: Ubuntu-重置密码
date: 2022-11-06 16:00:22.53
updated: 2022-11-06 16:00:22.53
url: /archives/ubuntu--zhong-zhi-mi-ma
categories: 
- Linux
tags: 
---

# 前言

之前在 VMware 上安装了一个 Ubantu Server 16.07 的系统，但是不知道什么原因那段时间使用不了，密码错误（我现在猜测是因为使用小键盘输入密码了），然后最近嗯，还挺想玩玩的，就想整理好，网上找了一下资料，解决了，这里记录下。

# 过程记录

我参考的资料里面说了，要进入修改的内个程序需要在开机的时候按住 shift ，但是，我这里不用，开机界面如下

![202211001](http://img.shuyepl.com/202211061553580.png)

在这个界面这里使用 **方向键** 的上下，移动到下面的那个选项，然后按回车键，进入如下的界面

![202211002](http://img.shuyepl.com/202211061553586.png)

这里选择下面的那个选项，但是注意不要按回车键进入，按 **e** 键进入

使用方向键控制光标找到 recovery nomodeset ，删掉，在 dis_ucode_ldr 后面输入 quiet splash rw init=/bin/bash ，按 F10 退出，进入下一个界面

![202211003](http://img.shuyepl.com/202211061553311.png)

在这个界面中输入 ` passwd ` ，回车，输入两次密码就可以修改 root 用户的密码了，（不要使用数字小键盘输入密码）

使用另外一个命令 `  passwd 用户名` ，输入两次密码，修改指定用户的用户密码。 

修改完密码之后按 Ctrl + Alt + Delete 退出，然后进入 Ubuntu 系统就可以了。



---

参考资料：

- [Ubuntu 重置密码_gxhlh的博客-CSDN博客_ubuntu重置密码](https://blog.csdn.net/qq_44721831/article/details/122182958)
- [怎么退出grub?-CSDN社区](https://bbs.csdn.net/topics/30078670)