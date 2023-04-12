---
title: Github 配置 SSH
date: 2022-05-30 21:22:00.0
updated: 2022-07-05 21:02:28.191
url: /archives/github配置ssh
categories: 
- 博客建设
tags: 
---



#### 内容简介：Github 上配置 SSH 的方式

<!--more-->

首先，确认你已经安装好 Git ，并且你有一个 Github 账号，然后，我们开始吧

在桌面上右键点击，选择 **Git Bash Here** ，在命令行中输入以下命令，生成秘钥

~~~
ssh-keygen -t rsa -C "邮箱地址"
~~~

之后输入三次回车，输入下面的命令切换目录

~~~
cd ~/.ssh
~~~

输入下面的命令查看是否有 id_rsa 和 id_rsa.pub 这两个文件夹

~~~
ls
~~~

其中 ，id_rsa 是私钥，自己保存的，不能透露给其他人， id_rsa.pub 是公钥，将里面的内容复制到 Github 上的相应位置就配置 ssh 成功了，现在，我们输入下面的命令查看公钥的内容

~~~
cat id_rsa.pub
~~~

将这条命令下面显示出来的内容全部复制

登录 Github ，在右上角用户头像那里的下拉菜单中选择 **Settings**

![001](http://img.shuyepl.com/202207042044191.png)

点击 **New SSH key**

![002](http://img.shuyepl.com/202207042044470.png)

上面的 **Title** 那里的内容是公钥的标题，随便写都可以，要是你想以后好辨认一点的话，也可以起个比较好的名字，下面的 **Key** 那里就将我们刚刚复制的内容粘贴上去就可以了，然后点击左下角的绿色按钮 **Add SSH key**

![003](http://img.shuyepl.com/202207042045913.png)

然后，over，添加成功了