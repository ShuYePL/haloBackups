---
title: Maven的依赖无法查看源码的解决方法
date: 2023-01-30 23:14:00.625
updated: 2023-01-30 23:14:00.625
url: /archives/maven-de-yi-lai-wu-fa-cha-kan-yuan-ma-de-jie-jue-fang-fa
categories: 
- Maven
tags: 
---

今天在搞Maven项目的时候想查看一下一些依赖的源码，发现Ctrl+鼠标左键点击之后出现的都是反编译的代码，我想看带注释的源码，搞了一个晚上，找了各种方法够没用，IDEA里面Project Structure-->libraries里面源码和doc文档一直都是爆红的状态，最后终于自己搞定了，记录一下这个问题的解决方法。

我去Maven仓库看了一下文件，发现里面源码的文件和doc文档的文件后缀都是.LASTUPDATED，网上查了一下，意思就是这个东西没下载成功，这个因素有多种，网络呀，配置的镜像点有没有资源呀都会这样，我之前给我的Maven配置了阿里的镜像点资源

~~~xml
<mirror>
    <id>maven-default-http-blocker</id>
    <mirrorOf>external:http:*</mirrorOf>
    <name>Pseudo repository to mirror external repositories initially using HTTP.</name>
    <url>http://0.0.0.0/</url>
    <blocked>true</blocked>
</mirror>
~~~

我一直以为这个镜像源下载东西很快，应该不会是它的问题，但是我已经用了很多其他方法都没有解决了，所以最后将这个镜像点注释掉，再一次加载Maven的资源后发现好了，源码和doc文档都有了，就离谱。