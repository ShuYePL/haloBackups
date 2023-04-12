---
title: moudle创建的时候选择了Maven模版，但没有创建相应的文件结构
date: 2023-03-23 23:27:25.489
updated: 2023-03-24 09:10:25.779
url: /archives/moudle-chuang-jian-de-shi-hou-xuan-ze-le-maven-mo-ban--dan-mei-you-chuang-jian-xiang-ying-de-wen-jian-jie-gou
categories: 
- Maven
tags: 
---

# 问题描述
我创建Maven项目之后没有去看IDEA里面JDK的配置是否是配置好的，因为每次创建项目之后我的JDK都会爆红，需要自己重新选择一下，包括两个地方，一个地方是Settings，一个是Project Structure里面的，两个地方都得设置一下，然后再重新创建Maven项目就可以了。

> 我猜测这个东西是因为JDK没有配置好，IDEA在创建的时候卡在检查JDK的这一步了，然后就没有创建好相关的文件，或者，就是IDEA中使用模板创建Maven项目对JDK有依赖，没有配置好相关的JDK，没有办法从网上下载相应的模板，或是卡在中间的哪一个文件的创建上了。