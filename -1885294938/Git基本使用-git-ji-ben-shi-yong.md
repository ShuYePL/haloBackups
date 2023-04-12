---
title: Git基本使用
date: 2022-07-04 19:47:12.452
updated: 2022-07-04 19:54:17.334
url: /archives/git-ji-ben-shi-yong
categories: 
- Git
tags: 
---

# Linus基础命令

|  命令   |           作用           |
| :-----: | :----------------------: |
|   cd    |       改变当前目录       |
|  cd ..  |     回到上一一级目录     |
|  clear  |           清屏           |
|   pwd   |     显示当前目录路径     |
|   ls    | 列出当前目录中的所有文件 |
|  touch  |       新建一个文件       |
|   rm    |         删除文件         |
|  rm -r  |      删除一个文件夹      |
|  mkdir  |      新建一个文件夹      |
|   mv    |         移动文件         |
|  reset  | 重新初始化终端（加清屏） |
| history |     查看使用命令历史     |
|  help   |           帮助           |
|  exit   |           退出           |
|    #    |           注释           |

注意：千万不要尝试rm -rf / （这是删除电脑中全部文件）

# Git环境配置

查看环境配置：git config -l  
查看系统的本地配置：git config --system --list  
查看全局配置（用户自己配置的）：git config --global --list  
Git相关文件配置位置（和上面用命令查看的结果一样）：

- Git\etc\gitconfig : Git安装目录下的gitconfig  （--system 是系统级的配置文件）
- C:\Users\Administrator\\.gitconfig : 只适用于当前登录用户的配置  （--global 全局的配置文件）   

设置自己的用户名和邮箱，用于后面提交文件
git config --global user.name "用户名"
git config --global user.email 使用的邮箱

# Git项目搭建

初始化项目：git init（在当前目录下初始化一个项目，会创建出一些必要的文件。这是一个隐藏起来的文件，想看的话记得打开查看隐藏文件的选项）  
仓库克隆（用这个也可以创建一个项目）：git clone 项目的url  

# 文件操作

下面操作的文件都是在Git项目根目录下创建的  
查看文件状态：git status  
将文件添加到暂存区：git add 文件名  或者  git add . （这里一点表示添加所有文件）  
将文件添加到本地仓库：git commit -m "第一次提交"（-m表示添加提交信息，后面跟着的就是相应的信息）  
添加忽略文件（有些文件我们并不需要上传，可以编写一个忽略文件控制我们不要上传哪一些文件）：  

![忽略文件的相关配置](http://img.shuyepl.com/202207041941134.png)

# SSH公钥配置

生成公钥：ssh-keygen -t rsa (-t rsa表示使用rsa这种加密算法)，后面一直回车就行。   
这条命令要在C:\Users\Administrator\\.ssh文件下执行，执行之后生成两个文件，id_rsa和id_rsa.pub，前者是私钥，后者是公钥，公钥文件中的内容粘贴到gitee或者github上相应的位置就可以了。  

# 新建项目

在初始化的项目文件下（或clone下来的文件夹下）直接创建相应的文件即可。

# 忽略文件编写

将工程上传Github上时可以选择性的上传一些文件，一些中间生成的文件我们可以不必上传，将其忽略，配置.gitignore文件即可实现上传时的自动过滤。  
文件的书写可以参考上面文件操作中的内容，当然Github官网已经为我们写好了一些.gitignore文件的配置，直接粘贴复制就可以了（老cv工程师了）。  
网址：https://github.com/github/gitignore  


---

参考视频：https://www.bilibili.com/video/BV1FE411P7B3?p=6&spm_id_from=pageDriver