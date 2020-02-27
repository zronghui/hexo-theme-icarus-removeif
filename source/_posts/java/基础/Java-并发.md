---
title: Java 并发
date: 2020-01-26 10:01:33
categories:
- java
- 基础
toc: true
tags:
  - Java
  - 并发
---

[CS-Notes](https://cyc2018.github.io/CS-Notes/#/notes/Java%20%E5%B9%B6%E5%8F%91)

<!--more-->



- [一、使用线程](https://cyc2018.github.io/CS-Notes/#/notes/Java 并发?id=一、使用线程)

- - [实现 Runnable 接口](https://cyc2018.github.io/CS-Notes/#/notes/Java 并发?id=实现-runnable-接口)

  - [实现 Callable 接口](https://cyc2018.github.io/CS-Notes/#/notes/Java 并发?id=实现-callable-接口)

    ```java
    public class myCallable implements Callable<Integer>{
      public Integer call(){
        return 123;
      }
    }
    ```

    

    ```java
    FutureTask<Integer> ft = new FutureTask<>(new myCallable());
    new Thread(new FutureTask<Integer>(ft).start();
    sout(ft.get());
    ```

    

  - [继承 Thread 类](https://cyc2018.github.io/CS-Notes/#/notes/Java 并发?id=继承-thread-类)

  - [实现接口 VS 继承 Thread](https://cyc2018.github.io/CS-Notes/#/notes/Java 并发?id=实现接口-vs-继承-thread)

    用接口好一些

- [二、基础线程机制](https://cyc2018.github.io/CS-Notes/#/notes/Java 并发?id=二、基础线程机制)

- - [Executor](https://cyc2018.github.io/CS-Notes/#/notes/Java 并发?id=executor)

    三种executor：

    CachedThreadPool--不限数量

    FixedThreadPool--固定数量

    SingleThreadExecutor--数量为一

    ```java
    public static void main(String[] args){
      ExecutorService executorService = Executors.newCachedThreadPool();
      for (int i=0; i<5; i++){
        executorService.execute(new myRunnable());
      }
      executorService.shutdown();
    }
    ```

    

  - [Daemon](https://cyc2018.github.io/CS-Notes/#/notes/Java 并发?id=daemon)

    

  - [sleep()](https://cyc2018.github.io/CS-Notes/#/notes/Java 并发?id=sleep)

    Sleep 可能会抛出InterruptedException，因为异常不能跨线程传播回main()中，因此必须在本地进行处理。

  - [yield()](https://cyc2018.github.io/CS-Notes/#/notes/Java 并发?id=yield)

- [三、中断](https://cyc2018.github.io/CS-Notes/#/notes/Java 并发?id=三、中断)

- - [InterruptedException](https://cyc2018.github.io/CS-Notes/#/notes/Java 并发?id=interruptedexception)

    无法中断IO阻塞和synchronized阻塞

  - [interrupted()](https://cyc2018.github.io/CS-Notes/#/notes/Java 并发?id=interrupted)

    如果一个线程的run()方法执行一个无限循环，并且没有执行sleep()等会抛出InterruptedException的操作，那么调用interrupt()方法就无法使线程提前结束。

    但是调用interrupt()方法会设置线程的中断标记，此时调用interrupted()方法会返回true。因此可以在循环体中使用interrupted()方法来判断线程是否处于中断状态，从而提前结束线程。

    ```java
    public class InterruptExample{
      private static class MyThread2 extends Thread {
        @Override
        public void run() {
          while(!interrupted()){
            // ...
          }
          sout("thread end");
        }
      }
      
      public static void main(String[] args) throws InterruptedException{
        Thread thread = MyThread2();
        thread2.start();
        thread.interrupt();
      }
    }
    ```

    

  - [Executor 的中断操作](https://cyc2018.github.io/CS-Notes/#/notes/Java 并发?id=executor-的中断操作)

    ```java
    executorService.shutdown();
    executorService.shutdownNow();
    ```

    若要中断某个线程：

    使用submit替代execute

    ```java
    Future<?> future = executorService.submit(() -> {
      // ...
    });
    future.cancel(true);
    ```

    

- [四、互斥同步](https://cyc2018.github.io/CS-Notes/#/notes/Java 并发?id=四、互斥同步)

- - [synchronized](https://cyc2018.github.io/CS-Notes/#/notes/Java 并发?id=synchronized)
  
  - [ReentrantLock](https://cyc2018.github.io/CS-Notes/#/notes/Java 并发?id=reentrantlock)
  
- [比较](https://cyc2018.github.io/CS-Notes/#/notes/Java 并发?id=比较)
  
    synchronized 是 JVM 实现的，而 ReentrantLock 是 JDK 实现的。
  
    性能大致相同
  
    ReentrantLock可中断，而synchronized不行。
  
    一个ReentrantLock可以同时绑定多个condition对象。
  
  - [使用选择](https://cyc2018.github.io/CS-Notes/#/notes/Java 并发?id=使用选择)
  
    除非使用ReentrantLock的高级功能，否则优先使用synchronized。这是因为synchronized是JVM实现的一种锁机制，JVM原生地支持它，而ReentrantLock不是所有的JDK版本都支持。并且使用synchronized不用担心没有释放锁而导致死锁问题，因为JVM会确保锁的释放。
  
- [五、线程之间的协作](https://cyc2018.github.io/CS-Notes/#/notes/Java 并发?id=五、线程之间的协作)

- - [join()](https://cyc2018.github.io/CS-Notes/#/notes/Java 并发?id=join)
  
    将当前线程挂起，直到目标线程结束。

  - [wait() notify() notifyAll()](https://cyc2018.github.io/CS-Notes/#/notes/Java 并发?id=wait-notify-notifyall)
  
    它们都是Object的方法
  
    而sleep是Thread的静态方法
  
  - [await() signal() signalAll()](https://cyc2018.github.io/CS-Notes/#/notes/Java 并发?id=await-signal-signalall)
  
    java.util.concurrent类库提供Condition类库来实现线程间的协调
  
    condition.await()
  
    conditon.signal()
  
    condition.signalAll()
  
    ```java
    public class AwaitSignalExample {
    
        private Lock lock = new ReentrantLock();
        private Condition condition = lock.newCondition();
    
        public void before() {
            lock.lock();
            try {
                System.out.println("before");
                condition.signalAll();
            } finally {
                lock.unlock();
            }
        }
    
        public void after() {
            lock.lock();
            try {
                condition.await();
                System.out.println("after");
            } catch (InterruptedException e) {
                e.printStackTrace();
            } finally {
                lock.unlock();
            }
        }
      
      public static void main(String[] args) {
          ExecutorService executorService = Executors.newCachedThreadPool();
          AwaitSignalExample example = new AwaitSignalExample();
          executorService.execute(() -> example.after());
          executorService.execute(() -> example.before());
      }
    }
    ```
  
- [六、线程状态](https://cyc2018.github.io/CS-Notes/#/notes/Java 并发?id=六、线程状态)

- - [新建（NEW）](https://cyc2018.github.io/CS-Notes/#/notes/Java 并发?id=新建（new）)
  - [可运行（RUNABLE）](https://cyc2018.github.io/CS-Notes/#/notes/Java 并发?id=可运行（runable）)
  - [阻塞（BLOCKED）](https://cyc2018.github.io/CS-Notes/#/notes/Java 并发?id=阻塞（blocked）)
  - [无限期等待（WAITING）](https://cyc2018.github.io/CS-Notes/#/notes/Java 并发?id=无限期等待（waiting）)
  - [限期等待（TIMED_WAITING）](https://cyc2018.github.io/CS-Notes/#/notes/Java 并发?id=限期等待（timed_waiting）)
  - [死亡（TERMINATED）](https://cyc2018.github.io/CS-Notes/#/notes/Java 并发?id=死亡（terminated）)

- [七、J.U.C - AQS](https://cyc2018.github.io/CS-Notes/#/notes/Java 并发?id=七、juc-aqs)

- - [CountDownLatch](https://cyc2018.github.io/CS-Notes/#/notes/Java 并发?id=countdownlatch)
  
    用来控制一个或多个线程等待多个线程

  - [CyclicBarrier](https://cyc2018.github.io/CS-Notes/#/notes/Java 并发?id=cyclicbarrier)
  
    多个线程互相等待
  
  - [Semaphore](https://cyc2018.github.io/CS-Notes/#/notes/Java 并发?id=semaphore)
  
    类似于操作系统的信号量，可以控制对互斥资源的访问线程数。
  
    ```java
    public class SemaphoreExample {
    
        public static void main(String[] args) {
            final int clientCount = 3;
            final int totalRequestCount = 10;
            Semaphore semaphore = new Semaphore(clientCount);
            ExecutorService executorService = Executors.newCachedThreadPool();
            for (int i = 0; i < totalRequestCount; i++) {
                executorService.execute(()->{
                    try {
                        semaphore.acquire();
                        System.out.print(semaphore.availablePermits() + " ");
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    } finally {
                        semaphore.release();
                    }
                });
            }
            executorService.shutdown();
        }
    }
    ```
  
- [八、J.U.C - 其它组件](https://cyc2018.github.io/CS-Notes/#/notes/Java 并发?id=八、juc-其它组件)

- - [FutureTask](https://cyc2018.github.io/CS-Notes/#/notes/Java 并发?id=futuretask)
  
  - [BlockingQueue](https://cyc2018.github.io/CS-Notes/#/notes/Java 并发?id=blockingqueue)

    java.util.concurrent.BlockingQueue接口有以下阻塞队列的实现
  
    **FIFO队列**： LinkedBlockingQueue ArrayBlockingQueue(固定长度)
  
    **优先级队列**：PriorityBlockingQueue
  
    提供了阻塞的take put 方法：如果队列为空 take 将阻塞，直到有内容；如果队列为满put将阻塞，直到队列有空闲位置
  
    使用BlockingQueue实现生产者消费者问题
  
    ```java
    public class ProducerConsumer{
      private static BlockingQueue<String> queue = new ArrayBlockingQueue<>(5);
      
      private static class Producer extends Thread {
        @Override
        public void run(){
          try{
            queue.put("product");
          }catch (InterruptedException e){
            e.printStackTrace();
          }
          sout("produce..");
        }
      }
      private static class Consumer extends Thread {
        @Override
        public void run(){
          try{
            String product = queue.take();
          } catch (InterruptedException e) {
            e.printStackTrace();
          }
          sout("consume...");
        }
      }
      public static void main(Stirng[] args) {
        for (int i = 0; i < 2; i++) {
            Producer producer = new Producer();
            producer.start();
        }
        for (int i = 0; i < 5; i++) {
            Consumer consumer = new Consumer();
            consumer.start();
        }
        for (int i = 0; i < 3; i++) {
            Producer producer = new Producer();
            producer.start();
        }
      }
    }
    ```
  
    ```html
    produce..produce..consume..consume..produce..consume..produce..consume..produce..consume..
    ```
  
  - [ForkJoin](https://cyc2018.github.io/CS-Notes/#/notes/Java 并发?id=forkjoin)
  
    ？
  
- [九、线程不安全示例](https://cyc2018.github.io/CS-Notes/#/notes/Java 并发?id=九、线程不安全示例)

- [十、Java 内存模型](https://cyc2018.github.io/CS-Notes/#/notes/Java 并发?id=十、java-内存模型)

- - [主内存与工作内存](https://cyc2018.github.io/CS-Notes/#/notes/Java 并发?id=主内存与工作内存)

  - [内存间交互操作](https://cyc2018.github.io/CS-Notes/#/notes/Java 并发?id=内存间交互操作)

  - [内存模型三大特性](https://cyc2018.github.io/CS-Notes/#/notes/Java 并发?id=内存模型三大特性)

  - - [1. 原子性](https://cyc2018.github.io/CS-Notes/#/notes/Java 并发?id=_1-原子性)
    
    - [2. 可见性](https://cyc2018.github.io/CS-Notes/#/notes/Java 并发?id=_2-可见性)

    - [3. 有序性](https://cyc2018.github.io/CS-Notes/#/notes/Java 并发?id=_3-有序性)
    
      volatile满足可见性，有序性，不能满足原子性。
    
      synchronized满足三者
    
  - [先行发生原则](https://cyc2018.github.io/CS-Notes/#/notes/Java 并发?id=先行发生原则)

  - - [1. 单一线程原则](https://cyc2018.github.io/CS-Notes/#/notes/Java 并发?id=_1-单一线程原则)
    - [2. 管程锁定规则](https://cyc2018.github.io/CS-Notes/#/notes/Java 并发?id=_2-管程锁定规则)
    - [3. volatile 变量规则](https://cyc2018.github.io/CS-Notes/#/notes/Java 并发?id=_3-volatile-变量规则)
    - [4. 线程启动规则](https://cyc2018.github.io/CS-Notes/#/notes/Java 并发?id=_4-线程启动规则)
    - [5. 线程加入规则](https://cyc2018.github.io/CS-Notes/#/notes/Java 并发?id=_5-线程加入规则)
    - [6. 线程中断规则](https://cyc2018.github.io/CS-Notes/#/notes/Java 并发?id=_6-线程中断规则)
    - [7. 对象终结规则](https://cyc2018.github.io/CS-Notes/#/notes/Java 并发?id=_7-对象终结规则)
    - [8. 传递性](https://cyc2018.github.io/CS-Notes/#/notes/Java 并发?id=_8-传递性)

- [十一、线程安全](https://cyc2018.github.io/CS-Notes/#/notes/Java 并发?id=十一、线程安全)

- - [不可变](https://cyc2018.github.io/CS-Notes/#/notes/Java 并发?id=不可变)

  - [互斥同步](https://cyc2018.github.io/CS-Notes/#/notes/Java 并发?id=互斥同步)

  - [非阻塞同步](https://cyc2018.github.io/CS-Notes/#/notes/Java 并发?id=非阻塞同步)

  - - [1. CAS](https://cyc2018.github.io/CS-Notes/#/notes/Java 并发?id=_1-cas)
    - [2. AtomicInteger](https://cyc2018.github.io/CS-Notes/#/notes/Java 并发?id=_2-atomicinteger)
    - [3. ABA](https://cyc2018.github.io/CS-Notes/#/notes/Java 并发?id=_3-aba)

  - [无同步方案](https://cyc2018.github.io/CS-Notes/#/notes/Java 并发?id=无同步方案)

  - - [1. 栈封闭](https://cyc2018.github.io/CS-Notes/#/notes/Java 并发?id=_1-栈封闭)
    
    - [2. 线程本地存储（Thread Local Storage）](https://cyc2018.github.io/CS-Notes/#/notes/Java 并发?id=_2-线程本地存储（thread-local-storage）)
    
      ![img](https://cs-notes-1256109796.cos.ap-guangzhou.myqcloud.com/6782674c-1bfe-4879-af39-e9d722a95d39.png)
    
      
    
      每个 Thread 都有一个 ThreadLocal.ThreadLocalMap 对象。
    
      ```java
      /* ThreadLocal values pertaining to this thread. This map is maintained
       * by the ThreadLocal class. */
      ThreadLocal.ThreadLocalMap threadLocals = null;
      ```
    
      当调用一个 ThreadLocal 的 set(T value) 方法时，先得到当前线程的 ThreadLocalMap 对象，然后将 ThreadLocal->value 键值对插入到该 Map 中。
    
      ```java
      public void set(T value) {
          Thread t = Thread.currentThread();
          ThreadLocalMap map = getMap(t);
          if (map != null)
              map.set(this, value);
          else
              createMap(t, value);
      }
      ```
    
      get() 方法类似。
    
      ```java
      public T get() {
          Thread t = Thread.currentThread();
          ThreadLocalMap map = getMap(t);
          if (map != null) {
              ThreadLocalMap.Entry e = map.getEntry(this);
              if (e != null) {
                  @SuppressWarnings("unchecked")
                  T result = (T)e.value;
                  return result;
              }
          }
          return setInitialValue();
      }
      ```
    
    - [3. 可重入代码（Reentrant Code）](https://cyc2018.github.io/CS-Notes/#/notes/Java 并发?id=_3-可重入代码（reentrant-code）)

- [十二、锁优化](https://cyc2018.github.io/CS-Notes/#/notes/Java 并发?id=十二、锁优化)

- - [自旋锁](https://cyc2018.github.io/CS-Notes/#/notes/Java 并发?id=自旋锁)
  
    在jdk1.6中引入了自适应的自旋锁，自适应意味着自选的次数不再固定了，而是由前一次在同一个锁上的自旋次数及锁的拥有者的状态来决定。
  
  - [锁消除](https://cyc2018.github.io/CS-Notes/#/notes/Java 并发?id=锁消除)

    锁消除是指对于被检测出不可能存在竞争的共享数据的锁进行消除。
  
  - [锁粗化](https://cyc2018.github.io/CS-Notes/#/notes/Java 并发?id=锁粗化)
  
    如果一系列的连续操作都对同一个对象反复加锁和解锁，频繁的加锁操作就会导致性能损耗。
  
    锁粗化--把加锁的范围扩展（粗化）到整个操作序列的外部
  
  - [轻量级锁](https://cyc2018.github.io/CS-Notes/#/notes/Java 并发?id=轻量级锁)
  
  - [偏向锁](https://cyc2018.github.io/CS-Notes/#/notes/Java 并发?id=偏向锁)
  
- [十三、多线程开发良好的实践](https://cyc2018.github.io/CS-Notes/#/notes/Java 并发?id=十三、多线程开发良好的实践)

  给线程起个有意义的名字，这样可以方便找bug

  缩小同步范围，从而减少锁的争用，例如对于synchronized, 应该尽量使用同步块而不是同步方法。

  多用同步工具少用wait notify。首先，CountDownLatch, CyclicBarrier, Semaphore 和 Exchanger 这些同步类简化了编码操作，而用wait和notify很难实现复杂控制流；其次，这些同步类是由最好的企业编写和维护，在后续的JDK中

  还会不断优化和完善。

  使用BlockingQueue实现生产者消费者问题

  多用并发集合少用同步集合，例如应该使用ConcurrentHashMap而不是Hashtable.

  使用本地变量和不可变类来保证线程安全。

  使用线程池而不是直接创建线程，这是因为创建线程代价很高，线程池可以有效地利用有限的线程来启动任务。

- [参考资料](https://cyc2018.github.io/CS-Notes/#/notes/Java 并发?id=参考资料)
