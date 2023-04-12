---
title: UML-Rational Rose与UML简单笔记
date: 2022-11-25 08:17:04.648
updated: 2022-11-25 08:17:04.648
url: /archives/uml-rationalrose-yu-uml-jian-dan-bi-ji
categories: 
- UML
tags: 
---

# 前言

在做项目真正写代码之前，需要先进行系统设计，规划好相关的内容，之后才能按照设计去写代码，这部分设计的内容就是 UML 。

UML （Unified Modeling Language）：统一建模语言（标准建模语言）。始与 1997 年的 OMG 标准，一个支持模型化和软件系统开发的图形化语言，为软件开发的所有阶段提供模型化和可视化支持，包括由需求分析到规格，到构造和配置。软件开发的时候，系统设计师 / 系统架构师给出 UML 设计图，程序员根据 UML 设计图来进行编码和开发。

## 能够实现 UML 的建模工具

- IBM Rational Rose
- StarUML
- MS Visio (流程图比较强)
- ...

## UML 常见的图有哪些

类图（Class Diagram）：描述类的信息（属性、方法），以及类和类之间的关系信息。

用例图（Use Case Diagram）：站在系统用户（系统角色）的角度分析系统存在哪些功能。

时序图（Sequence Diagram）：描述程序的执行过程，方法的调用过程，方法的返回值等信息（方法的执行）。

状态图：.....

活动图：.....

# Rational Rose 使用

## 保存文件

保存文件的时候选择保存文件的后缀名为 **.mdl** 

## 打开文件

我这台电脑不知道怎么回事，直接双击 mdl 文件不能自动使用 Rational Rose 打开，必须要在 Rational Rose 里面点击 Fild --> open 选择相应的 mdl 文件才能打开。

## 改变显示的样式

在类或接口的上面右键 Options --> Dtereotype Display 中，可以选择显示的图标样式。

Options 中还可以显示很多的东西，比如方法的返回值类型：Options --> Show Operation Signature

## 类图

在 Rational Rose 中的 Logical View 目录下实现类图，一般创建一些目录来组织这些类（类的数量比较多的话，比较好组织管理），创建类图的话就是选择相应的包文件（package），然后右键 new --> Class Diagram ，创建完成之后双击就可以进入画布进行操作了。

### 类和类之间的关系

- 泛化关系（is a）

    ![202211007](http://img.shuyepl.com/202211241350850.png)

- 实现关系（like a）

    ![202211008](http://img.shuyepl.com/202211241455192.png)

- 关联关系（has a）

    ![202211009](http://img.shuyepl.com/202211241458960.png)

    ![202211010](http://img.shuyepl.com/202211241510720.png)

- 聚合关系

    聚合关系描述的是整体和部分的关系，聚合关系是比较特殊的关联关系，在聚合关系中，整体的 生命周期不会决定部分的生命周期。

    在 UML 中双击关联关系的内条线，然后在部分的那一方中，将 Navigable 前面的勾去掉（箭头消失了），在整体的哪一方勾选上 Aggregate （出现了一个菱形），就代表了聚合关系。

    ![202211011](http://img.shuyepl.com/202211241524480.png)

- 组合关系

    组合关系可以看做一种特殊的聚合关系，整体的声明周期决定部分的声明周期，部分是依附在整体上面的，部分离开了整体是无法独立存在的。

    在聚合关系图的基础上，在整体所在的一方中（双击内条线）将 By Value 前面的勾打上。

    ![202211012](http://img.shuyepl.com/202211241532833.png)

- 依赖关系

    依赖关系是所有关系中最弱的一种，这种关系通常体现类和局部变量之间的关系。

    ![202211013](http://img.shuyepl.com/202211241537903.png)

## 用例图

站在系统用户的角度分析系统存在哪些功能。使用用例图的时候需要先进行系统角色的抽取。在 Rational Rose 的 Use Case View 中实现用例图。对着包右键 new --> Actor 就是创建系统角色。

之后在相应的包下右键 new --> Use Case Diagram 在画布里面将系统角色拖进去，就可以为他创建功能了。

![202211014](http://img.shuyepl.com/202211241551978.png)

## 时序图

时序图中描述方法的调用过程，程序的执行流程，以及方法执行结束的返回值情况。Rational Rose 中在 Logical View 里面实现，右键 new --> Sequence Diagram 



用例图中的一个使用案例对应一个时序图。

![202211015](http://img.shuyepl.com/202211241602786.png)

时序图发送的信息用的是那个实线的箭头，返回的信息使用的是内个虚线的箭头。



---

参考资料：

- [【动力节点】UML与Rational Rose__老杜_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1L4411x76j/?spm_id_from=333.880.my_history.page.click&vd_source=3b9bc9314c9590a1d18a84ef490fb982)