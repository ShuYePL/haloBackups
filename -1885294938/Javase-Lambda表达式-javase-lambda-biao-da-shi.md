---
title: Javase-Lambda表达式
date: 2022-11-14 15:19:33.236
updated: 2022-11-14 15:19:33.236
url: /archives/javase-lambda-biao-da-shi
categories: 
- Javase
tags: 
---

# Lambda 表达式的作用

简化代码，就是之前本来有些类功能很简单的，写成内部类可简化一些东西了，但还是嫌它麻烦，就用 Lambda 表达式简化一下。

Lambda 表达式是属于函数式编程的概念了，使用 Lambda 表达式简化代码有一个条件，Lambda 表达式简化代码的那一个接口必须是函数式接口。

**这也是函数式接口的概念** ：一个接口里面只包含唯一一个抽象方法。

结合下面的内容其实好像就可以理解为什么 Lambda 表达式简化的条件必须是函数式接口了，因为当接口里面只有一个抽象方法的时候，实现该接口必然就是要实现这一个方法，这样的话，实现这个方法的一些常见操作，像是写 new 这个单词这种事情就可以省略了嘛。

**函数式接口示例** ：

~~~java
interface Ilike {
    void lambda();
}
~~~

# Lambda 表达式怎么来的

## 简化代码的演变

### 先是写一个接口，一个实现类，再创建一个对象，调用它的方法

```java
package com.shuyepl.lambdaDemo;

public class Demo01 {
    public static void main(String[] args) {
        // 接口调用实现类
        Ilike like = new Like();
        like.lambda();
    }
}

// 函数式接口
interface Ilike {
    void lambda();
}

// 实现类
class Like implements Ilike {
    @Override
    public void lambda() {
        System.out.println("谢谢狂神！十分感谢！");
    }
}
```

### 静态内部类

```java
package com.shuyepl.lambdaDemo;

public class Demo01 {
    // 静态内部类
    static class Like2 implements Ilike {
        @Override
        public void lambda() {
            System.out.println("谢谢狂神！十分感谢！");
            System.out.println("谢谢狂神！十分感谢！");
        }
    }

    public static void main(String[] args) {
        // 静态内部类调用
        System.out.println("静态内部类调用");
        Ilike like2 = new Like2();
        like2.lambda();
    }
}

// 函数式接口
interface Ilike {
    void lambda();
}
```

### 局部内部类

```java
package com.shuyepl.lambdaDemo;

public class Demo01 {
    public static void main(String[] args) {
        // 3局部内部类
        class Like3 implements Ilike {
            @Override
            public void lambda() {
                System.out.println("局部内部类");
                System.out.println("谢谢狂神！十分感谢！");
                System.out.println("谢谢狂神！十分感谢！");
                System.out.println("谢谢狂神！十分感谢！");
            }
        }

        Ilike ilike3 = new Like3();
        ilike3.lambda();
    }
}

// 函数式接口
interface Ilike {
    void lambda();
}
```

### 匿名内部类

```java
package com.shuyepl.lambdaDemo;

public class Demo01 {
    public static void main(String[] args) {
        // 4匿名内部类
        System.out.println("匿名内部类");
        Ilike ilike4 = new Ilike() {
            @Override
            public void lambda() {
                System.out.println("谢谢狂神！十分感谢！");
                System.out.println("谢谢狂神！十分感谢！");
                System.out.println("谢谢狂神！十分感谢！");
                System.out.println("谢谢狂神！十分感谢！");

            }
        };
        ilike4.lambda();
    }
}

// 函数式接口
interface Ilike {
    void lambda();
}
```

### Lambda 表达式，只留下核心逻辑代码，将外围重复的代码全部去掉，追求简化

```java
package com.shuyepl.lambdaDemo;

public class Demo01 {
    public static void main(String[] args) {
        // 5Lambda表达式
        System.out.println("lambda表达式");
        Ilike ilike5 = () -> {
            System.out.println("谢谢狂神！十分感谢！");
            System.out.println("谢谢狂神！十分感谢！");
            System.out.println("谢谢狂神！十分感谢！");
            System.out.println("谢谢狂神！十分感谢！");
            System.out.println("谢谢狂神！十分感谢！");
        };
        ilike5.lambda();
    }
}

// 函数式接口
interface Ilike {
    void lambda();
}
```

如果接口方法只需要一个参数的话，可以将 () 省略掉，多个参数的话就不可以省略，但是，如果没有参数，要写一个空的 () 这个不能省略；如果方法里面只有一行代码的话，可以省略 {}，多行代码就不可以省略；如果多个参数，不想写参数类型的话，可以将参数类型省略，但是省略的话要将全部参数类型都省略，不想省略的话就全部都写。

省略参数类型的例子

~~~java
package com.shuyepl.lambdaDemo;

public class Demo02 {
    // 省略参数类型
    Hi hi = (name, greet) -> {
        System.out.println(greet + ", " + name + "!");
    };
}

// 函数式接口
interface Hi {
    void hi(String name, String greet);
}
~~~

不省略参数类型的例子

```java
package com.shuyepl.lambdaDemo;

public class Demo02 {
    // 省略参数类型
    Hi hi = (String name, String greet) -> {
        System.out.println(greet + ", " + name + "!");
    };
}

// 函数式接口
interface Hi {
    void hi(String name, String greet);
}
```

如果只有一行代码，且有返回值，会自动返回返回值，如果是代码块的，则需要写返回语句

```java
package com.shuyepl.lambdaDemo;

public class Demo03 {
    public static void main(String[] args) {
        // 不写花括号的情况下，可以不用写return语句，自动返回
        Add add = (a, b) -> a + b;
        int retAdd = add.add(2, 3); // 5
        System.out.println(retAdd);

        // 写上花括号了，需要写return语句才行
        Sub sub = (a, b) -> {return a - b;};
        int retSub = sub.sub(3, 2); // 1
        System.out.println(retSub);
    }
}

interface Add {
    int add(int a, int b);
}

interface Sub {
    int sub(int a, int b);
}
```

---

参考资料：

- [多线程10：Lamda表达式_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1V4411p7EF?p=10&vd_source=3b9bc9314c9590a1d18a84ef490fb982)
- [Java Lambda 表达式 | 菜鸟教程 (runoob.com)](https://www.runoob.com/java/java8-lambda-expressions.html)