---
title: Hexo 博客添加一只小猫咪
date: 2022-06-23 20:11:00.0
updated: 2022-07-05 21:01:45.224
url: /archives/hexo博客添加一只小猫咪
categories: 
- 博客建设
tags: 
---



#### 文章简介：记录一下为 Hexo 博客添加看板娘的过程

<!--more-->

将命令行切换到博客文件的位置，输入下面的命令，安装依赖

~~~cmd
npm install --save hexo-helper-live2d
~~~

点击[这里](https://github.com/xiazeyu/live2d-widget-models/tree/master/packages)下载模型

上面的连接点击过去会看到很多文件夹，每一个文件夹都是一个模型，加入模型的时候，将要使用的模型的对应文件直接整个丢进 node_modules 文件夹中就可以了

然后将下面的内容加入到博客的 _config.yml 文件中或者主题的 _config.yml 文件中

~~~yml
## Live2D看板娘
live2d:
  # 是否要开启看板娘，false 就是关闭
  enable: true
  pluginModelPath: assets/
  model:
    # 这里填入你想要显示的模板的名称，也就是你加入到 node_modules 文件中内个模型的名字
    use: live2d-widget-model-hijiki
  display:
  	# 这里可以调整模型显示的位置，可以写 left 或者 right，将模型显示在左下角或者右下角
    position: left
    # width 和 height 属性可以调整整个模型显示的大小，这里我的小猫在 1:2 的比例下都能显示出来
    width: 150
    height: 300
  mobile:
    # 是否要在手机端显示，true 就是要，false 就是不要
    show: false   
  rect:
    opacity:0.7
~~~

接下来说一些注意点，在修改上面的配置的时候，每一次配置之后想要在页面上查看到改变后的效果，是需要重新启动服务的，也就是说，在本地端开启的服务要先关闭，然后

~~~cmd
hexo clean
hexo s
~~~

行了，over

---

参考资料：[hexo 添加live2d看板动画 - 简书 (jianshu.com)](https://www.jianshu.com/p/3a6342e16e57)