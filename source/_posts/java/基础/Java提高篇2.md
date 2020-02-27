---
title: Java提高篇2
date: 2020-01-16 14:41:11
categories:
- java
- 基础
toc: true
tags:
- Java
---

[toc]

<!--more-->
# [详解匿名内部类](https://wiki.jikexueyuan.com/project/java-enhancement/java-ten.html)

1、匿名内部类中不能存在任何的静态成员变量和静态方法
2、匿名内部类不能是抽象的，它必须要实现继承的类或者实现的接口的所有抽象方法
3、当所在的方法的形参需要被内部类里面使用时，该形参必须为 final
4、我们一般都是利用构造器来完成某个实例的初始化工作的，但是匿名内部类是没有构造器的！那怎么来初始化匿名内部类呢？使用构造代码块！利用构造代码块能够达到为匿名内部类创建一个构造器的效果。

```java
public class OutClass {
    public InnerClass getInnerClass(final int     age,final String name){
        return new InnerClass() {
            int age_ ;
            String name_;
            //构造代码块完成初始化工作
            {
                if(0 < age && age < 200){
                    age_ = age;
                    name_ = name;
                }
            }
            public String getName() {
                return name_;
            }

            public int getAge() {
                return age_;
            }
        };
    }

    public static void main(String[] args) {
        OutClass out = new OutClass();

        InnerClass inner_1 = out.getInnerClass(201, "chenssy");
        System.out.println(inner_1.getName());

        InnerClass inner_2 = out.getInnerClass(23, "chenssy");
        System.out.println(inner_2.getName());
    }
}
```

# [代码块](https://wiki.jikexueyuan.com/project/java-enhancement/java-twelve.html)

## 静态代码块

主要目的就是对静态属性进行初始化

```Java
public class Test {
    static{
        System.out.println("静态代码块");
    }
}
```

## 构造代码块

```Java
public class Test {
    {
        System.out.println("执行构造代码块...");
    }
}
```

## 静态代码块、构造代码块、构造函数执行顺序

1、静态代码块，它是随着类的加载而被执行，只要类被加载了就会执行，而且只会加载一次，主要用于给类进行初始化。
2、构造代码块，每创建一个对象时就会执行一次，且优先于构造函数，主要用于初始化不同对象共性的初始化内容和初始化实例环境。
3、构造函数，每创建一个对象时就会执行一次。同时构造函数是给特定对象进行初始化，而构造代码是给所有对象进行初始化，作用区域不同。
通过上面的分析，他们三者的执行顺序应该为：静态代码块 > 构造代码块 > 构造函数。

# [equals() 方法总结](https://wiki.jikexueyuan.com/project/java-enhancement/java-thirteen.html)

数组域 : 使用 Arrays.equals
在 equals() 中使用 getClass 进行类型判断
我们在覆写 equals() 方法时，一般都是推荐使用 getClass 来进行类型判断，不是使用 instanceof

# [异常(二) - Java 提高篇 - 极客学院Wiki](https://wiki.jikexueyuan.com/project/java-enhancement/java-seventeen.html)

结论一：尽可能的减小 try 块
结论二：保证所有资源都被正确释放。充分运用 finally 关键词。
结论三：catch 语句应当尽量指定具体的异常类型，而不应该指定涵盖范围太广的 Exception 类。 不要一个 Exception 试图处理所有可能出现的异常。
结论四：既然捕获了异常，就要对它进行适当的处理。不要捕获异常之后又把它丢弃，不予理睬。 不要做一个不负责的人。
结论五：在异常处理模块中提供适量的错误原因信息，组织错误信息使其易于理解和阅读。

```java
public void test() throws XxxException{
    try {
        //do something:可能抛出异常信息的代码块
    } catch (Exception e) {
        throw new XxxException(e);
    }
}
```

# [详解 Java 定时任务](https://wiki.jikexueyuan.com/project/java-enhancement/java-add1.html)

## 一、简介

Timer 是一种定时器工具，用来在一个后台线程计划执行指定任务，而 TimerTask 一个抽象类，它的子类代表一个可以被 Timer 计划的任务

### Timer 类

Timer 类可以保证多个线程可以共享单个 Timer 对象而无需进行外部同步，所以 Timer 类是线程安全的

Timer 提供了 schedule 方法，该方法有多中重载方式来适应不同的情况，如下：
schedule(TimerTask task, Date time)：安排在指定的时间执行指定的任务
schedule(TimerTask task, Date firstTime, long period) ：安排指定的任务在指定的时间开始进行重复的固定延迟执行
schedule(TimerTask task, long delay) ：安排在指定延迟后执行指定的任务。
schedule(TimerTask task, long delay, long period) ：安排指定的任务从指定的延迟后开始进行重复的固定延迟执行。

scheduleAtFixedRate(TimerTask task, Date firstTime, long period)：安排指定的任务在指定的时间开始进行重复的固定速率执行。
scheduleAtFixedRate(TimerTask task, long delay, long period)：安排指定的任务在指定的延迟后开始进行重复的固定速率执行。

<details>
  <summary>[scheduleWithFixedDelay 和 scheduleAtFixedRate 的区别 - 简书](https://www.jianshu.com/p/2bed76de59a9)</summary>
ScheduledExecutorService#scheduleAtFixedRate() 指的是“以固定的频率”执行，period（周期）指的是两次成功执行之间的时间
比如，scheduleAtFixedRate(command, 5, 2, second)，第一次开始执行是5s后，假如执行耗时1s，那么下次开始执行是7s后，再下次开始执行是9s后

而 ScheduledExecutorService#scheduleWithFixedDelay() 指的是“以固定的延时”执行，delay（延时）指的是一次执行终止和下一次执行开始之间的延迟
scheduleWithFixedDelay(command, 5, 2, second)，第一次开始执行是 5s 后，假如执行耗时 1s，执行完成时间是 6s 后，那么下次开始执行是 8s 后，再下次开始执行是 11s 后

</details>

### TimerTask

TimerTask 类是一个抽象类，有一个抽象方法 run() 方法
还有两个非抽象的方法：
boolean cancel()：取消此计时器任务。
long scheduledExecutionTime()：返回此任务最近实际执行的安排执行时间

### 分析 schedule 和 scheduleAtFixedRate

schedule 方法侧重保存间隔时间的稳定，而 scheduleAtFixedRate 方法更加侧重于保持执行频率的稳定

## 三、Timer 的缺陷

### 3.1、Timer 的缺陷

如果 TimerTask 抛出未检查的异常，Timer 将会停止整个线程

对于 Timer 的缺陷，我们可以考虑 ScheduledThreadPoolExecutor 来替代。Timer 是基于绝对时间的，对系统时间比较敏感，而 ScheduledThreadPoolExecutor 则是基于相对时间；Timer 是内部是单一线程，而 ScheduledThreadPoolExecutor 内部是个线程池，所以可以支持多个任务并发执行。

### 3.2、用 ScheduledExecutorService 替代 Timer

对于 Timer 的缺陷，我们可以考虑 ScheduledThreadPoolExecutor 来替代。Timer 是基于绝对时间的，对系统时间比较敏感，而 ScheduledThreadPoolExecutor 则是**基于相对时间**；Timer 是内部是单一线程，而 ScheduledThreadPoolExecutor 内部是个**线程池**，所以可以支持多个任务并发执行。

<details>
  <summary>旧方案</summary>
```java
public class TimerTest03 {
    Timer timer;
    public TimerTest03(){
        timer = new Timer();
        timer.schedule(new TimerTaskTest03(), 1000, 2000);
    }
    public static void main(String[] args) {
        new TimerTest03();
    }
}

public class TimerTaskTest03 extends TimerTask{
    @Override
    public void run() {
        Date date = new Date(this.scheduledExecutionTime());
        System.out.println("本次执行该线程的时间为：" + date);
    }
}

```


</details>
<details>
  <summary>新方案</summary>
```Java
public class ScheduledExecutorTest {
    private  ScheduledExecutorService scheduExec;

    public long start;

    ScheduledExecutorTest(){
        this.scheduExec =  Executors.newScheduledThreadPool(2);  
        this.start = System.currentTimeMillis();
    }

    public void timerOne(){
        scheduExec.schedule(new Runnable() {
            public void run() {
                System.out.println("timerOne,the time:" + (System.currentTimeMillis() - start));
                try {
                    Thread.sleep(4000);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
        },1000,TimeUnit.MILLISECONDS);
    }

    public void timerTwo(){
        scheduExec.schedule(new Runnable() {
            public void run() {
                System.out.println("timerTwo,the time:" + (System.currentTimeMillis() - start));
            }
        },2000,TimeUnit.MILLISECONDS);
    }

    public static void main(String[] args) {
        ScheduledExecutorTest test = new ScheduledExecutorTest();
        test.timerOne();
        test.timerTwo();
    }
}
```

</details>
