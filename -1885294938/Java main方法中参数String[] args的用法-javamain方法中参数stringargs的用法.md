---
title: Java main方法中参数String[] args的用法
date: 2021-11-21 11:39:57.0
updated: 2022-11-04 20:57:00.072
url: /archives/javamain方法中参数stringargs的用法
categories: 
- Javase
tags: 
---


#### 内容简介：记录下main方法中String[] args这个参数的一些相关知识点。

<!--more-->

先前一段时间学了数组，老师也将String[] args的一些作用讲了一下，现在记录一下：String[] args和其他数组看上去一模一样，所以在看到这个的时候也可以判断它就是一个一维数组了（废话），这个一维数组可以存储我们在启动.class文件时输入的一些参数，它的操作方式和其他一维数组没什么区别。也就是我们可以使用一维数组遍历的方式对args数组进行遍历操作。就像下面这段代码一样（这段代码在后面会用到）。
例子：
~~~java
public class Array12 {
    public static void main(String[] args) {
        for(int i = 0; i < args.length; i++){
            System.out.println(args[i]);
        }
    }
}
~~~
# 输入数据的方法
在命令行中，我们在启动.class文件的时候顺便把想要输入的参数写入到启动命令的后面并把相应的参数以空格隔开就可以完成。例如将上面的一段代码输入带文本文档中并保存，在命令行中运行（在命令行中输入一些数据的情况下），结果如下：
~~~test
C:\Users\jh\Desktop>javac Array12.java

C:\Users\jh\Desktop>java Array12 a b c d
a
b
c
d
~~~

在IDEA中我们输入数据的方式要简单得多，具体的操作方式如下：在代码编辑区右键，选择Modify Run Configuration...

![202111001](http://img.shuyepl.com/202207042000909.png)

之后在Program arguments中输入想要输入的参数即可

![202111002](http://img.shuyepl.com/202207042000432.png)

记得当输入多个参数的时候，参数与参数之间要用空格隔开

![202111003](http://img.shuyepl.com/202207042001109.png)


之后点击右下角的Apply和ok就可以了，运行结果如下：  
~~~
a
b
c
d
e
f
g
~~~