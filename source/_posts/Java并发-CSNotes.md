---
title: Java并发-CSNotes
toc: true
recommend: 1
uniqueId: '2020-09-09 07:10:41/"Java并发-CSNotes".html'
date: 2020-09-09 15:10:41
thumbnail:
categories:
tags:
keywords:
---

[TOC]

<!--more-->



\> 本文由 \[简悦 SimpRead\](http://ksria.com/simpread/) 转码， 原文地址 \[cyc2018.github.io\](https://cyc2018.github.io/CS-Notes/#/notes/Java%20%E5%B9%B6%E5%8F%91?id=executor)

> 当所有非守护线程结束时，程序也就终止，同时会杀死所有守护线程。
>
> main() 属于非守护线程。
>
> 在线程启动之前使用 setDaemon() 方法可以将一个线程设置为守护线程。

> Thread.sleep(millisec)

> sleep() 可能会抛出 InterruptedException，因为异常不能跨线程传播回 main() 中，因此必须在本地进行处理。线程中抛出的其它异常也同样需要在本地进行处理。

> 对静态方法 Thread.yield() 的调用声明了当前线程已经完成了生命周期中最重要的部分，可以切换给其它线程来执行。该方法只是对线程调度器的一个建议，而且也只是建议具有相同优先级的其它线程可以运行。

> 主要有三种 Executor：
>
> *   CachedThreadPool：一个任务创建一个线程；
> *   FixedThreadPool：所有任务只能使用固定大小的线程；
> *   SingleThreadExecutor：相当于大小为 1 的 FixedThreadPool。
>
> 这里写的应该有错误，CachedThreadPool 没有线程数量限制，FIxedThreadPool 有线程数量限制，SingleThreadExecutor 只能有 1 个线程。 最常用的是 CachedThreadPool

> 通过调用一个线程的 interrupt() 来中断该线程，如果该线程处于阻塞、限期等待或者无限期等待状态，那么就会抛出 InterruptedException，从而提前结束该线程。但是不能中断 I/O 阻塞和 synchronized 锁阻塞。
>
> 为什么不能中断 io 阻塞和 synchronized 锁阻塞

> 如果一个线程的 run() 方法执行一个无限循环，并且没有执行 sleep() 等会抛出 InterruptedException 的操作，那么调用线程的 interrupt() 方法就无法使线程提前结束。
>
> 除了非 io 阻塞和 synchronized 锁阻塞的线程，interrupt 还可能中断不了一般的线程？

> 但是调用 interrupt() 方法会设置线程的中断标记，此时调用 interrupted() 方法会返回 true。因此可以在循环体中使用 interrupted() 方法来判断线程是否处于中断状态，从而提前结束线程。

> 调用 Executor 的 shutdown() 方法会等待线程都执行完毕之后再关闭，但是如果调用的是 shutdownNow() 方法，则相当于调用每个线程的 interrupt() 方法。

> 如果只想中断 Executor 中的一个线程，可以通过使用 submit() 方法来提交一个线程，它会返回一个 Future<?> 对象，通过调用该对象的 cancel(true) 方法就可以中断线程。

> ```
> Future<?> future = executorService.submit(() -> {
>  
> });
> future.cancel(true);
> ```

> 1\. 同步一个代码块

> 它只作用于同一个对象，如果调用两个对象上的同步代码块，就不会进行同步。

> 对于以下代码，两个线程调用了不同对象的同步代码块，因此这两个线程就不需要同步。从输出结果可以看出，两个线程交叉执行。

> ```
> public static void main(String[] args) {
>  SynchronizedExample e1 = new SynchronizedExample();
>  SynchronizedExample e2 = new SynchronizedExample();
>  ExecutorService executorService = Executors.newCachedThreadPool();
>  executorService.execute(() -> e1.func1());
>  executorService.execute(() -> e2.func1());
> }
> ```

> 2\. 同步一个方法

> 它和同步代码块一样，作用于同一个对象。

> 3\. 同步一个类

> 作用于整个类，也就是说两个线程调用同一个类的不同对象上的这种同步语句，也会进行同步。

> ```
> public class SynchronizedExample {
> 
>  public void func2() {
>      synchronized (SynchronizedExample.class) {
>          for (int i = 0; i < 10; i++) {
>              System.out.print(i + " ");
>          }
>      }
>  }
> }
> ```

> 4\. 同步一个静态方法

> 作用于整个类。

> [使用选择](#/notes/Java%20%E5%B9%B6%E5%8F%91?id=%e4%bd%bf%e7%94%a8%e9%80%89%e6%8b%a9)
> ---------------------------------------------------------------------------------
>
> 除非需要使用 ReentrantLock 的高级功能，否则优先使用 synchronized。这是因为 synchronized 是 JVM 实现的一种锁机制，JVM 原生地支持它，而 ReentrantLock 不是所有的 JDK 版本都支持。并且使用 synchronized 不用担心没有释放锁而导致死锁问题，因为 JVM 会确保锁的释放。

> 调用 wait() 使得线程等待某个条件满足，线程在等待时会被挂起，当其他线程的运行使得这个条件满足时，其它线程会调用 notify() 或者 notifyAll() 来唤醒挂起的线程。
>
> 它们都属于 Object 的一部分，而不属于 Thread。

> 只能用在同步方法或者同步控制块中使用

> 使用 wait() 挂起期间，线程会释放锁。这是因为，如果没有释放锁，那么其它线程就无法进入对象的同步方法或者同步控制块中，那么就无法执行 notify() 或者 notifyAll() 来唤醒挂起的线程，造成死锁。

> ```
> public class WaitNotifyExample {
> 
>  public synchronized void before() {
>      System.out.println("before");
>      notifyAll();
>  }
> 
>  public synchronized void after() {
>      try {
>          wait();
>      } catch (InterruptedException e) {
>          e.printStackTrace();
>      }
>      System.out.println("after");
>  }
> }
> ```

> ```
> public static void main(String[] args) {
>  ExecutorService executorService = Executors.newCachedThreadPool();
>  WaitNotifyExample example = new WaitNotifyExample();
>  executorService.execute(() -> example.after());
>  executorService.execute(() -> example.before());
> }
> ```

> **wait() 和 sleep() 的区别**
>
> *   wait() 是 Object 的方法，而 sleep() 是 Thread 的静态方法；
> *   wait() 会释放锁，sleep() 不会。

> java.util.concurrent 类库中提供了 Condition 类来实现线程之间的协调，可以在 Condition 上调用 await() 方法使线程等待，其它线程调用 signal() 或 signalAll() 方法唤醒等待的线程。

> 相比于 wait() 这种等待方式，await() 可以指定等待的条件，因此更加灵活。
>
> 使用 Lock 来获取一个 Condition 对象。

> ```
> public class AwaitSignalExample {
> 
>  private Lock lock = new ReentrantLock();
>  private Condition condition = lock.newCondition();
> 
>  public void before() {
>      lock.lock();
>      try {
>          System.out.println("before");
>          condition.signalAll();
>      } finally {
>          lock.unlock();
>      }
>  }
> 
>  public void after() {
>      lock.lock();
>      try {
>          condition.await();
>          System.out.println("after");
>      } catch (InterruptedException e) {
>          e.printStackTrace();
>      } finally {
>          lock.unlock();
>      }
>  }
> }
> ```

> CountDownLatch
>
> 这里说的不清楚，建议看官方文档，说的很好很清楚。 这里的图片和代码都不清楚

> CyclicBarrier

> 和 CountdownLatch 相似，都是通过维护计数器来实现的。线程执行 await() 方法之后计数器会减 1，并进行等待，直到计数器为 0，所有调用 await() 方法而在等待的线程才能继续执行。

> CyclicBarrier 的计数器通过调用 reset() 方法可以循环使用

> Semaphore 类似于操作系统中的信号量，可以控制对互斥资源的访问线程数。

> 模拟了对某个服务的并发请求，每次只能有 3 个客户端同时访问，请求总数为 10。

> [ForkJoin](#/notes/Java%20%E5%B9%B6%E5%8F%91?id=forkjoin)
> ---------------------------------------------------------
>
> 主要用于并行计算中，和 MapReduce 原理类似，都是把大的计算任务拆分成多个小任务并行计算。

> ForkJoin 使用 ForkJoinPool 来启动，它是一个特殊的线程池，线程数量取决于 CPU 核数。

> ForkJoinPool 实现了工作窃取算法来提高 CPU 的利用率。每个线程都维护了一个双端队列，用来存储需要执行的任务。工作窃取算法允许空闲的线程从其它线程的双端队列中窃取一个任务来执行。窃取的任务必须是最晚的任务，避免和队列所属线程发生竞争。

> 所有的变量都存储在主内存中，每个线程还有自己的工作内存，工作内存存储在高速缓存或者寄存器中，保存了该线程使用的变量的主内存副本拷贝。

> 线程只能直接操作工作内存中的变量，不同线程之间的变量值传递需要通过主内存来完成。

> int 类型读写操作满足原子性只是说明 load、assign、store 这些单个操作具备原子性

> AtomicInteger 能保证多个线程修改的原子性。

> 可见性指当一个线程修改了共享变量的值，其它线程能够立即得知这个修改。Java 内存模型是通过在变量修改后将新值同步回主内存，在变量读取前从主内存刷新变量值来实现可见性的。

> 有序性是指：在本线程内观察，所有操作都是有序的

> 在一个线程观察另一个线程，所有操作都是无序的，无序是因为发生了指令重排序。在 Java 内存模型中，允许编译器和处理器对指令进行重排序，重排序过程不会影响到单线程程序的执行，却会影响到多线程并发执行的正确性。

> volatile 关键字通过添加内存屏障的方式来禁止指令重排，即重排序时不能把后面的指令放到内存屏障之前

> 也可以通过 synchronized 来保证有序性，它保证每个时刻只有一个线程执行同步代码，相当于是让线程顺序执行同步代码

> 不可变（Immutable）的对象一定是线程安全的

> 多线程环境下，应当尽量使对象成为不可变，来满足线程安全

> 对于集合类型，可以使用 Collections.unmodifiableXXX() 方法来获取一个不可变的集合。

> 互斥同步属于一种悲观的并发策略，总是认为只要不去做正确的同步措施，那就肯定会出现问题。无论共享数据是否真的会出现竞争，它都要进行加锁

> 基于冲突检测的乐观并发策略：先进行操作，如果没有其它线程争用共享数据，那操作就成功了，否则采取补偿措施（不断地重试，直到成功为止）

> 这种同步操作称为非阻塞同步

> 比较并交换（Compare-and-Swap，CAS）

> ### [3\. ABA](#/notes/Java%20%E5%B9%B6%E5%8F%91?id=_3-aba)
>
> 如果一个变量初次读取的时候是 A 值，它的值被改成了 B，后来又被改回为 A，那 CAS 操作就会误认为它从来没有被改变过。

> 可以使用 java.lang.ThreadLocal 类来实现线程本地存储功能。

> ![](https://cs-notes-1256109796.cos.ap-guangzhou.myqcloud.com/6782674c-1bfe-4879-af39-e9d722a95d39.png)

> 每个 Thread 都有一个 ThreadLocal.ThreadLocalMap 对象。

> 应该尽可能在每次使用 ThreadLocal 后手动调用 remove()，以避免出现 ThreadLocal 经典的内存泄漏甚至是造成自身业务混乱的风险。

> 自旋锁的思想是让一个线程在请求一个共享数据的锁时执行忙循环（自旋）一段时间，如果在这段时间内能获得锁，就可以避免进入阻塞状态。

> 自旋锁虽然能避免进入阻塞状态从而减少开销，但是它需要进行忙循环操作占用 CPU 时间，它只适用于共享数据的锁定状态很短的场景。

> 在 JDK 1.6 中引入了自适应的自旋锁。自适应意味着自旋的次数不再固定了，而是由前一次在同一个锁上的自旋次数及锁的拥有者的状态来决定。

> [锁粗化](#/notes/Java%20%E5%B9%B6%E5%8F%91?id=%e9%94%81%e7%b2%97%e5%8c%96)
> -----------------------------------------------------------------------
>
> 如果一系列的连续操作都对同一个对象反复加锁和解锁，频繁的加锁操作就会导致性能损耗。

