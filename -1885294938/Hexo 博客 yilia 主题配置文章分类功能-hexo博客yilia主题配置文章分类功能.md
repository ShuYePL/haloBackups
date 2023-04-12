---
title: Hexo 博客 yilia 主题配置文章分类功能
date: 2022-06-23 17:23:00.0
updated: 2022-07-05 21:01:50.5
url: /archives/hexo博客yilia主题配置文章分类功能
categories: 
- 博客建设
tags: 
---



#### 内容简介：Hexo 博客 yilia 主题配置文章分类功能 ，主要记录自己更改的一点点页面样式

<!--more-->

Hexo 博客 yilia 主题下配置文章分类功能的文章在网上一搜一大把，但是，都是一模一样的文章，(⊙o⊙)…

照着那样做就能做出分类功能了，但是，内个样子不是很好看，所以在配置好了之后我自己修改了一些样式

PS：没学过 ejs ，所以就是用普通的 html 和一点点的 css 简单改了一下而已，而且，因为我现在这个这是初步改一下，我还没感觉很满意，所以样式都是写在标签内的，等我有时间，有想法（重点是这个）之后我再对这个页面的样式进行修改，并且将样式分离到单独的文件中。

---

下面的分类功能搭建的过程：

首先，在博客目录下使用下面的命令行指令

~~~cmd
hexo new page categories
~~~

上面的 categories 是可以更改的，看个人喜好

然后，在博客目录下会生成 source -> categories -> index.md 这个文件，打开它，将里面的内容修改如下

~~~text
---
title: 文章分类
date: 2022-06-23 10:07:14
type: categories
layout: "categories"

---
~~~

打开 yilia 主题的 _config.yml 文件，在文件的头部可以看到下面的 menu 选项，在下面添加分类选项（这里填写的东西最后会显示在头像的下面）

~~~cmd
menu:
  主页: /
  #随笔: /tags/随笔/
  分类: /categories

~~~



然后打开博客目录下的 themes -> yilia -> source -> main.0cf68a.css ，在末尾的地方添加以下的代码

~~~css
/* 这个是文章分类页面的css样式，目前只看到这一个地方是有用的 */
.category-all-page {
  margin: 30px 40px 30px 40px;
  position: relative;
  min-height: 100vh;  /* 这个控制分类页面的最小高度 */
}
.category-all-page h2 {
  margin: 20px 0;
}
.category-all-page .category-all-title {
  text-align: center;
}
.category-all-page .category-all {
  margin-top: 20px;
}
.category-all-page .category-list {
  margin: 0;
  padding: 0;
  list-style: none;
}
.category-all-page .category-list-item-list-item {
  margin: 10px 15px;
}
.category-all-page .category-list-item-list-count {
  /* 这个地方调整的是分类链接后面跟着的括号和里面的数字的样式 */
  color: $grey; 
}
.category-all-page .category-list-item-list-count:before {
  /* 分类连接后面的文章数前面的符号 */
  display: inline;
  content: " (";
}
.category-all-page .category-list-item-list-count:after {
  /* 分类连接后面的文章数后面的符号 */
  display: inline;
  content: ") ";
}
.category-all-page .category-list-item {
  margin: 100px 10px;
}
.category-all-page .category-list-count {
  color: $grey;
}
.category-all-page .category-list-count:before {
  display: inline;
  content: " (";
}
.category-all-page .category-list-count:after {
  display: inline;
  content: ") ";
}
.category-all-page .category-list-child {
  padding-left: 10px;
}
~~~

在 yilia -> layout 目录下创建一个文件，名字为 categories.ejs ，内容如下

~~~ejs
<article class="article article-type-post show">
  <!-- 这个是头顶的那个很丑的部分，待会改一下 -->
  <header style="border-bottom: 1px solid #ccc;border-left:none;">
    <div itemprop="name" sytle="">
      <!-- <%= page.title %> -->
      <p style="color:black;font-size:40px;text-align:center;">文章分类</p>
      <h2 style="margin:1%;text-align:center">共计&nbsp;<%= site.categories.length %>&nbsp;个分类</h2>
    </div>
  </header>

  <div class="category-all-page">
    <% if (site.categories.length){ %>
        <%- list_categories(site.categories, {
          show_count: true,
          class: 'category-list-item',
          style: 'list',
          depth:3,  
          separator: ''
        }) %> 
    <% } %>
  </div>
</article>
~~~

到这里，这个分类功能就已经算是配置完成了，要想对文章进行分类，只需要在文章的头部那里添加如下的文字就行了

~~~
categories：
	- 这里填写文章属于哪一个分类
~~~

像是下面这样的

~~~
title: Github 配置 SSH
date: 2022-05-30 21:22:00
tags:
	- 个人博客
categories:
	- 个人博客
~~~

行了，over