---
title: hexo搭建个人博客的笔记
date: 2021-10-30 21:29:07.0
updated: 2022-07-05 21:02:43.026
url: /archives/hexo搭建个人博客的笔记
categories: 
- 博客建设
tags: 
---


#### 文章简介：本文章记录了使用hexo结合yilia主题搭建个人博客过程中遇到的一些问题及相应的解决方法，还有一些改善页面显示效果的方式。

<!--more-->

## Q1：Markdown文件自动生成的文字中，tags：的作用是什么
在创建一篇文章（使用命令hexo n "文章名"）之后，打开该文章的Markdown文件，在文件的前面有如下的一段自动生成的文字：

![Q1Markdown文件头部生成的文字](http://img.shuyepl.com/202207052055096.png)

当我第一次在tags：之后填入内容时(胡乱填入的，像是：51单片机，51，单片机)，在使用相关的hexo命令时出现了相关的错误提示：

![Q1tag乱写之后出现的错误](http://img.shuyepl.com/202207052055432.png)

之后在网上查到的结果是在tags：里面写的东西是有一定格式的，必须按照特定的格式进行编写才不会出错，格式如下：

![Q1tags的正确编写格式](http://img.shuyepl.com/202207052056324.png)


在设置完tags之后可以在主页“所有文章”（如下面上图标识）中打开tags（如下面下图所示），就可以根据tag快速筛选相应的文章了。

![Q1个人博客主页_标记所有文章](http://img.shuyepl.com/202207052057062.png)

![Q1所有文章_tag作用](http://img.shuyepl.com/202207052057045.png)


## Q2：如何在个人博客中添加不蒜子统计统计记录站点访问数

（这个东西真的花了一点时间，查了很多其他人的博客，照做了，但都没有达到相应的效果，最后自己照着不蒜子官网的方式来一下子就好了）  

不蒜子网站网址：http://busuanzi.ibruce.info/  
打开这个网站后可以看到如下的界面：  

![Q2不蒜子页面](http://img.shuyepl.com/202207052057178.png)

z注意中间黄色方框内的两行代码，将这两行代码分别粘贴到blog\themes\yilia\layout\_partial\footer.ejs文档的如下位置：

![Q2不蒜子代码在文档中的位置](http://img.shuyepl.com/202207052058890.png)

然后将文档的编码方式改为utf-8（不改的话会显示乱码）  
最后显示的效果如下：

![Q2显示效果](http://img.shuyepl.com/202207052058596.png)

这里的数字很大是因为用hexo s在本地查看的原因，将文件上传到远端就好了。

## Q3：vscode中Markdown预览插件看到的结果和在yilia主题中看到的不一样

如对于以下的一段Markdown代码：

~~~markdown
- 获取一维数组元素的值：  
  >代码示例：`数组名[元素下标]`   
  例子（这里顺便将得到的值进行打印输出）：   
  >```java  
  int[] nums = {1, 2, 3, 4, 5};  
  for(int i = 0; i < 5; i++){  
      System.out.print(nums[i] + " ");  
  }  
  ```  
  输出结果：1 2 3 4 5 
~~~

在vscode中的显示结果：

![Q3vscode中的显示结果](http://img.shuyepl.com/202207052058572.png)


在yilia主题下的显示结果：

![Q3实际yilia主题下的显示结果](http://img.shuyepl.com/202207052059107.png)


## Q4：围栏代码显示失效

有时在yilia主题下显示代码块时，不知道为什么没有办法按照我指定的方式进行语法突出显示，解决方法是不要将代码块包含在反引号中，而是包含在波浪号中。（目前不知道具体的原因，只知道这样做可行）

## Q5：如何记录自己的总文章数

参考资料：[Hexo-yilia主题个性化美化及功能添加 - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/108918133)

在F:\blog\themes\yilia\layout\_partial\left-col.ejs文件中

~~~ejs
<nav class="header-menu">
	<ul>
	<% for (var i in theme.menu){ %>
		<li><a href="<%- url_for(theme.menu[i]) %>"><%= i %></a></li>
	<%}%>
	</ul>
</nav>
~~~

的后面加上下面这段代码：

~~~ejs
<nav>
    总文章数 <%=site.posts.length%>
</nav>
~~~

## Q6：如何在头像的下面显示自己的昵称

在F:\blog\themes\yilia\layout\_partial\left-col.ejs文件中

~~~ejs
<% if (theme.subtitle){ %>
<p class="header-subtitle"><%=theme.subtitle%></p>
<%}%>
~~~

的后面加上下面一段代码：

~~~html
<font size="6">薯叶</font>
~~~

font标签中size属性的值可以是1~7中的任一个整数，数字越大，显示出来的字体也就越大。

## Q7：如何解决yilia主题主页页脚翻页按钮显示bug

![Q7显示bug](http://img.shuyepl.com/202207052059121.png)

这个问题真的是困扰了我很久了，甚至我都想放弃解决这个问题了，直到在美化其他部分的时候看到一篇文章（参考文章：[(19条消息) 软件 | hexo博客主题yilia上一页下一页显示的问题_RyanCYK的计算机世界-CSDN博客](https://blog.csdn.net/CYK5201995/article/details/107769487)）介绍到了这个东西，才发现这个bug很多人都遇到过。

具体的解决方法就是在F:\blog\themes\yilia\layout\_partial\archive.ejs文件中第8、9行代码修改如下

~~~ejs
prev_text: '上一页',
next_text: '下一页'
~~~

第37、38行的代码修改如下

~~~ejs
prev_text: '上一页',
next_text: '下一页'
~~~

之后在打开与archive.ejs同级目录下的script.ejs文件，按Ctrl+f找到a标签中的`&laquo; Prev`和`Next &raquo;`，并将其删除即可。最终的显示效果如下

![Q7显示正常](http://img.shuyepl.com/202207052059449.png)