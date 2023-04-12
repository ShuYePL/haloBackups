---
title: Java多线程
date: 2023-03-16 19:14:28.869
updated: 2023-03-16 19:14:28.869
url: /archives/java-duo-xian-cheng
categories: 
- Javase
tags: 
---

# 多线程

一个程序对应于一个进程，一个进程可以对应多个线程。一个线程程指的是进程中一个单一顺序的控制流，可以把线程当做是一个程序里面需要处理的各种分立的任务，像是一个 Java 程序的执行就对应着一个进程，但是它在运行的时候至少包含两个线程，一个是 main 线程，一个是 gc 线程。

> 进程：一个进程包括由操作系统分配的内存空间，包含一个或多个线程。一个线程不能独立的存在，它必须是进程的一部分。一个进程一直运行，直到所有的非守护线程都结束运行后才能结束。

# 创建线程的方法

- 实现Runnable接口 （*）

    ~~~java
    package com.shuyepl.demo;
    
    public class demo {
        public static void main(String[] args) {
            Thread t2 = new Thread(new MyThread());
            t2.start();
    
            for (int i = 0; i < 10; i++) {
                System.out.println("main ==> " + i);
            }
        }
    }
    
    class MyThread implements Runnable {
        public void run(){
            for (int i = 0; i < 10; i++) {
                System.out.println("MyThread ==> " + i);
            }
        }
    }
    ~~~

    运行结果：

    ~~~
    main ==> 0
    main ==> 1
    main ==> 2
    main ==> 3
    main ==> 4
    main ==> 5
    MyThread ==> 0
    main ==> 6
    main ==> 7
    main ==> 8
    main ==> 9
    MyThread ==> 1
    MyThread ==> 2
    MyThread ==> 3
    MyThread ==> 4
    MyThread ==> 5
    MyThread ==> 6
    MyThread ==> 7
    MyThread ==> 8
    MyThread ==> 9
    ~~~

- 继承Thread类本身 （*）

    ~~~java
    package com.shuyepl.demo;
    
    public class demo {
        public static void main(String[] args) {
            MyThread t1 = new MyThread();
            t1.start();
    
            for (int i = 0; i < 10; i++) {
                System.out.println("main ==> " + i);
            }
        }
    }
    
    class MyThread extends Thread {
        public void run(){
            for (int i = 0; i < 10; i++) {
                System.out.println("MyThread ==> " + i);
            }
        }
    }
    ~~~

    运行结果：

    ~~~
    main ==> 0
    MyThread ==> 0
    MyThread ==> 1
    MyThread ==> 2
    MyThread ==> 3
    MyThread ==> 4
    MyThread ==> 5
    MyThread ==> 6
    MyThread ==> 7
    MyThread ==> 8
    main ==> 1
    main ==> 2
    MyThread ==> 9
    main ==> 3
    main ==> 4
    main ==> 5
    main ==> 6
    main ==> 7
    main ==> 8
    main ==> 9
    ~~~

- 通过Callable和Future创建线程

# 使用多线程的建议

通过上面的例子可以看出来，使用继承 Thread 类来创建多线程的方式和使用实现 Runnable 接口来实现多线程的方式差不多，区别在于前者可以直接使用对象的 run 方法来进行多线程，而后者则需要将对象传入一个 Thread 对象中，再去调用 Thread 对象的 run 方法。

在使用的时候，考虑到 Java 里面只支持单继承，所以，采用实现 Runnable 接口的方式实现多线程是一个比较好的方式。

同时，使用 Runnable 实现多线程还有一个好处是，可以将同一个实现了 Runnable 接口的类对象丢进多个 Thread 对象中，实现多个线程。

下面是一个简单的例子，缺点是没有加上相应的线程控制，所以会出现一些不合预期的情况，比如从结果中可以看到，t3 和 t2 共同抢到了 1 号 gift ，而我们想的是每个 gift 只能给一个人，这样，就出现问题了。

~~~java
public class Demo01 {
    public static void main(String[] args){
        MyThread myThread = new MyThread();
        new Thread(myThread, "t1").start();
        new Thread(myThread, "t2").start();
        new Thread(myThread, "t3").start();
    }
}

class MyThread implements Runnable {
    public int total = 10;

    public void run () {
        while (true) {
            if (total <= 0) {
                break;
            }
            System.out.println(Thread.currentThread().getName() + " get " + (10 - total) + " gift ");
            total--;
        }
    }
}
~~~

运行结果：

~~~
t3 get 0 gift 
t3 get 1 gift 
t1 get 0 gift 
t1 get 3 gift 
t2 get 0 gift 
t1 get 4 gift 
t1 get 6 gift 
t1 get 7 gift 
t1 get 8 gift 
t1 get 9 gift 
t3 get 2 gift 
t2 get 5 gift 
~~~

# 线程的生命周期

一个线程的可能状态有五种： **新建状态、就绪状态、运行状态、死亡状态、阻塞状态**

![2022090012](https://img.shuyepl.com/202209211551097.png)

新建状态（创建状态）：当一个线程被 new 出来之后就是这个状态。

就绪状态：线程调用 start() 方法后等待 CPU 的调度；处于运行状态的线程释放 CPU 资源进入就绪状态，等待 CPU 的再次调度；阻塞状态解除之后进入就绪状态。

运行状态：就绪状态的线程获得 CPU 资源后进入运行状态。

处于运行状态的线程可以转变为以下三种状态：阻塞状态、死亡状态、就绪状态。

阻塞分为三种：等待阻塞、同步阻塞、其他阻塞

- 等待阻塞：调用线程的wait方法会使线程进入等待阻塞
- 同步阻塞：获取同步锁失败时会进入同步阻塞
- 其他阻塞：通过线程的sleep方法或者join方法进入该状态，等该状态结束之后会进入就绪状态

死亡状态：线程自然执行完毕或者外部干涉终止该线程，一旦进入死亡状态就不能再次启动。

# 线程的一些常用方法

|              方法              |                  说明                  |
| :----------------------------: | :------------------------------------: |
|  setPriority(int newPriority)  |            更改线程的优先级            |
| static void sleep(long millis) |    让正在执行的线程休眠指定的毫秒数    |
|          void join()           | 插入该线程进行执行，等待该线程执行结束 |
|      static void yield()       |    暂停执行的线程对象，执行其他线程    |
|        void interrupt()        |                中断线程                |
|       boolean isAlive()        |        测试线程是否处于活动状态        |
|             stop()             |             停止线程的运行             |

## 线程停止

上面写的那些能让线程停止的方法不太建议使用，建议让线程自动停止，就是说在程序 run 方法里面写上让线程结束退出（运行到结尾）的条件。 

退出线程的示例：

```java
package com.shuyepl.threadState;

public class StopTest implements Runnable {
    // 判断线程是否停止的标志位,true 不停止，false 停止
    private boolean flag = true;

    @Override
    public void run() {
        int i = 0;
        while (flag) {
            System.out.println("running......" + i++);
        }
    }

    // 让线程停下来的一个stop方法，我自己写的
    public void stop() {
        flag = false;
    }

    public static void main(String[] args) {
        // 创建并启动线程
        StopTest stopTest = new StopTest();
        new Thread(stopTest).start();

        // 满足条件时调用我们自己写的stop方法，让线程停下来
        for (int i = 0; i < 1000; i++) {
            System.out.println("main ==> " + i);
            if (i == 900) {
                stopTest.stop();
            }
        }
    }
}
```

## 线程休眠

sleep 执行的时候不会释放对象上面的锁。

巧妙利用 sleep 方法可以模拟网络延时，还有就是放大一些并发问题的发生性（就是在本地环境让我们看到）。

```java
package com.shuyepl.threadState;

import java.text.SimpleDateFormat;
import java.util.Date;

public class SleepTest {
    public static void main(String[] args) {
        // 创建Date对象
        Date nowTime = null;

        // SimpleDateFormat对象的创建
        SimpleDateFormat sdf = new SimpleDateFormat("HH:mm:ss");

        // 循环每一秒打印一次时间，打印时间间隔1秒
        for (int i = 0; i < 10; i++) {
            // 获取当前时间，格式化为字符串并打印
            nowTime = new Date();
            System.out.println(sdf.format(nowTime));
            // 休眠一秒
            try {
                Thread.sleep(1000);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    }
}
```

## 线程礼让

使用 yield 方法可以让当前的线程放弃使用的 CPU 时间片，返回到**就绪状态** ，再次等待 CPU 的调度，这种礼让的方式不一定会成功让其它线程执行，看 CPU 如何调度了。

```java
package com.shuyepl.threadState;

public class YieldTest implements Runnable {
    @Override
    public void run() {
        for (int i = 0; i < 100; i++) {
            System.out.println("run ==> " + i);
            // 循环变量是10的倍数就礼让一次
            if (i % 10 == 0) {
                Thread.yield();
            }
        }
    }

    public static void main(String[] args) {
        new Thread(new YieldTest()).start();
        for (int i = 0; i < 100; i++) {
            System.out.println("main ==> " + i);
        }
    }
}
```

## 线程插队

join 方法可以让当前执行的线程暂停，而让调用 join 方法的线程执行（这个就是插队的线程），等待插队的线程执行结束之后，原来暂停的线程才继续执行。

```java
package com.shuyepl.threadState;

public class JoinTest implements Runnable {
    @Override
    public void run() {
        for (int i = 0; i < 100; i++) {
            System.out.println("run ==> " + i);
        }
    }

    public static void main(String[] args) {
        Thread thread = new Thread(new JoinTest());
        thread.start();

        for (int i = 0; i < 100; i++) {
            System.out.println("main ==> " + i);
            if (i == 50) {
                try {
                    // 插队，main线程阻塞，等待thread线程执行结束，main线程才执行
                    thread.join();
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
        }
    }
}
```

这个 join 方法，只会让执行这个代码的线程阻塞，比如下面这个代码，join 方法是在 main 里面执行的，只有 main 线程会阻塞，其他线程依旧正常执行

```java
package com.shuyepl.threadState;

public class JoinTest implements Runnable {
    @Override
    public void run() {
        for (int i = 0; i < 100; i++) {
            System.out.println(Thread.currentThread() + "run ==> " + i);
        }
    }

    public static void main(String[] args) {
        Thread thread1 = new Thread(new JoinTest(), "t1");
        thread1.start();

        Thread thread2 = new Thread(new JoinTest(), "t2");
        thread2.start();


        for (int i = 0; i < 100; i++) {
            System.out.println("main ==> " + i);
            if (i == 50) {
                try {
                    // 插队，main线程阻塞，等待thread1线程执行结束，main线程才执行，thread2线程不受影响
                    thread1.join();
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
        }
    }
}
```

# 线程状态检测

通过 getState() 方法可以查看线程的当前状态，这些状态分别如下：

|   线程状态    |                             解释                             |
| :-----------: | :----------------------------------------------------------: |
|      NEW      |                   尚未启动的线程在这个状态                   |
|   RUNNABLE    |                   线程在Java虚拟机中运行了                   |
|    BLOCKED    |            被阻塞等待监视器锁定的线程处于这个状态            |
|    WAITING    |        等待另外一个线程执行特定动作的线程处于这个状态        |
| TIMED_WAITING | 正在等待另外一个线程执行动作到达指定等待时间的线程处于这个状态 |
|  TERMINATED   |                   已经退出的线程所处的状态                   |

# 线程优先级

使用 getPriority() 方法获取线程优先级，使用 setPriority(int priority) 设置线程优先级。

在 Thread 类中有几个常量，分别表示最小优先级，最大优先级和默认优先级

- Thread.MIN_PRIORITY = 1;
- Thread.MAX_PRIORITY = 10;
- Thread.NORM_PRIORITY = 5;

Java 提供一个线程调度器监控程序中处于就绪状态的线程，线程调度器根据优先级决定调度哪个线程来执行，优先级的高低只是线程被 CPU 调度的概率高低而已，具体调度还得看 CPU 的决策。

# 守护线程

线程其实有两种，**用户线程** 和 **用户线程** ，虚拟机会确保用户线程执行完毕，但不会等待守护线程执行完毕，gc 垃圾回收就是一种守护线程。

调用线程的 setDaemon(true) 方法可将线程设置为守护线程，这个方法的默认参数是 false 的，也就是用户线程。

```java
package com.shuyepl.threadState;

public class DaemonTest implements Runnable {
    @Override
    public void run() {
        for (int i = 0; i < 10000000; i++) {
            System.out.println("我是一个守护线程");
        }
    }

    public static void main(String[] args) {
        Thread thread = new Thread(new DaemonTest());
        // 设置线程为守护线程
        thread.setDaemon(true);
        thread.start();

        for (int i = 0; i < 10; i++) {
            System.out.println("我是一个用户线程"); // 运行结果是守护线程没有完整运行
        }
    }
}
```

# 线程同步机制

多个线程同时操作同一个对象的时候，可能会发生错误，例如：我和我的朋友分享了我的银行卡账号和密码，卡里有100块钱，如果我们同时一个在手机操作取钱，一个在 ATM 机操作取钱，加入我们的这两个取钱的线程同步启动，并且所有执行的代码都同步运行（很理想），那我们看到的银行卡的余额就都是100块对吧，如果这个时候我在 ATM 取100块，我的朋友在手机上取一百块，那因为我们从数据库中查看到的数据都是有100块钱余额，那就都取成功了，余额变成0了，结果是卡里的100块变成200块了，这个很魔幻对不对，算是一个很致命的错误了，银行不可能让人这样子取钱的，不然早破产了，那这里面就涉及到线程控制了。

线程控制简单点说就是大家执行到修改数据库中数据的时候需要排队，一个一个来，不能一起来。用上面那个银行的例子来说的话就是，当我们两个人取钱的请求到达服务器之后，必须排队去操作我卡里的余额，只有前面一个人的业务完成之后，修改完数据库中的余额了，后面的人才能进入数据库中去操作，所以，因为我已经先取完100块钱了，数据库中的余额变为0了，所以，我的朋友进来想取100块钱的话，就余额不足了，取款失败。

## synchronized

我们控制线程的方式是使用 synchronized 关键字给线程加锁，在使用的过程中，为了程序的运行速度等考虑，建议只给操作数据的部分加上锁，而不用把查数据的代码也一起加锁，这样会拖慢整个程序的运行速度。

---

文章引用资料来自：

- [菜鸟教程](https://www.runoob.com/)
- [多线程28：总结_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1V4411p7EF/?p=28&spm_id_from=333.880.my_history.page.click&vd_source=3b9bc9314c9590a1d18a84ef490fb982)