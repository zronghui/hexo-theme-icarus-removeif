---
title: Java 面试宝典笔记
toc: true
recommend: 1
uniqueId: '2020-09-29 09:30:40/"Java 面试宝典笔记".html'
date: 2020-09-29 17:30:40
thumbnail:
categories:
- java
- 基础
tags:
keywords:
---

[TOC]

<!--more-->



## 01.Java 面试宝典 - v1.1.pdf 的批注摘要。





### 6.什么是值传递和引用传递？

什么是值传递和引用传递？ 什么是值传递和引用传递？ 对象被值传递，意味着传递了对象的一个副本。因此，就算是改变了对象副本，也不会影响源对象的值。 对象被引用传递，意味着传递的并不是实际的对象，而是对象的引用。因此，外部对引用对象所做的改变会反映 到所有的对象上。


### 11.是否可以从一个静态（static）方法内部发出对非静态（non-static）方法的调用？

### 12.如何实现对象克隆？

如何实现对象克隆？ 如何实现对象克隆？

答：有两种方式： 1.实现 Cloneable 接口并重写 Object 类中的 clone() 方法； 2.实现 Serializable 接口，通过对象的序列化和反序列化实现克隆，可以实现真正的深度克隆。


### 13.一个“.java”源文件中是否可以包含多个类（不是内部类）？有什么限制？

### 14.Anonymous Inner Class(匿名内部类)是否可以继承其它类？是否可以实现 接口？

答：可以继承其他类或实现其他接口

### 15.内部类可以引用它的包含类（外部类）的成员吗？有没有什么限制？

 答：一个内部类对象可以访问创建它的外部类对象的成员，包括私有成员


### 2.Overload 和 Override 的区别？ Overloaded 的方法是否可以改变返回值的类型?

### 

什么是复制构造函数？


### 3.Java 中，什么是构造函数？什么是构造函数重载？什么是复制构造函数？

Java 不支持像 C++ 中那样的复制构造函数，这个不同点是因为如果你不自己写构造函数的情况下，Java不会创 建默认的复制构造函数。

### 4.构造器 Constructor 是否可被 Override?

Override? Override?

Override? 构造器 Constructor 不能被继承，因此不能重写 Override，但可以被重载 Overload。

### 6.接口和抽象类的区别是什么？

接口中所有的方法隐含的都是抽象的。而抽象类则可以同时包含抽象和非抽象的方法。

类如果要实现一个接口，它必须要实现接口声明的所有方法。但是，类可以不实现抽象类声明的所有方法，当然，在这种情况下，类也必须得声明成是抽象的。

Java 接口中声明的变量默认都是 final 的。抽象类可以包含非 final 的变量。 Java 接口中的成员函数默认是 public 的。抽象类的成员函数可以是 private，protected 或者是 public 。

抽象类也不可以被实例化，但是，如果它包含 main 方法的话是可以被调 用的。



**看起来，抽象类 就是普通的类里面加了没有实现的方法（抽象方法）。而接口限制较多，所有方法都是抽象的，变量是 final，成员函数全是 public **




### 7.下列说法正确的有（）

普通的类方法是可以和类名同名的，和构造方法唯一的区分就是，构造方法没有 返回值

### 8.Java 接口的修饰符可以为?


### 

C.final D.abstract

接口很重要，为了说明情况，这里稍微啰嗦点： （1）接口用于描述系统对外提供的所有服务,因此接口中的成员常量和方法都必须是公开(public)类型的,确保外部 使用者能访问它们；

（2）接口仅仅描述系统能做什么,但不指明如何去做,所以接口中的方法都是抽象(abstract)方法；

（3）接口不涉及和任何具体实例相关的细节,因此接口没有构造方法,不能被实例化,没有实例变量，只有静态（static）变量；

（4）接口的中的变量是所有实现类共有的，既然共有，肯定是不变的东西，因为变化的东西也不能够算共有。所 以变量是不可变(final)类型，也就是常量了。

### 9.下面是 People 和 Child 类的定义和构造方法，每个构造方法都输出编号。在 执行 new Child("mike") 的时候都有哪些构造方法被顺序调用？请选择输出结果

```java
class People { 
  String name; 
  public People() { 
    System.out.print(1); 
  } 
  public People(String name) {
    System.out.print(2); 
    this.name = name; 
  } 
}

class Child extends People { 
  People father; 
  public Child(String name) { 
    System.out.print(3); 
    this.name = name; 
    father = new People(name + ":F"); 
  } 
  public Child() { 
    System.out.print(4); 
  } 
}
```




### 

D.132 答案：D

：子类没有显示调用父类构造函数，不管子类构造函数是否带参数都默认调用父类无参的构造函 数，若父类没有则编译出错。


### 11.两个对象值相同(x.equals(y) == true)，但却可有不同的 hash code，这句话对不对？

：(1)如果两个对象相同（equals 方法返回 true ），那么它们的 hashCode 值一定要相同；(2)如果两个对象的 hashCode 相同，它们并不一定相同。

如果你违背了上述原则就会发现在使用容器时，相同的对象可以出现在 Set 集合中，同时增加新元素 的效率会大大下降（对于使用哈希存储的系统，如果哈希码频繁的冲突将会造成存取性能急剧下降）。


### 12.接口是否可继承（extends）接口? 抽象类是否可实现（implements）接口? 抽象类是否可继承具体类（concrete class）?

### 13.子类父类方法执行顺序

指出下面程序的运行结果:

```java
class A{ 
  static{ 
    System.out.print("1"); 
  } 
  public A(){ 
    System.out.print("2"); 
  } 
}

class B extends A{ 
  static{ 
    System.out.print("a"); 
  } 
  public B(){ 
    System.out.print("b"); 
  } 
} 

public class Hello{ 
  public static void main(String[] args){ 
    A ab = new B(); 
    ab = new B(); 
  } 
}
```

创建对象时构造器的调用顺序是：先初始化静态成员，然后调用父类构造器，再初始化 非静态成员，最后调用自身构造器。

1a2b2b

**总结：调用顺序按照以下规则排序**

1. 静态方法>普通方法
2. 父类方法>子类方法



### 14.Class.forName（String className）这个方法的作用

通过类的全名获得该类的类对象


### 15.什么是 AOP 和 OOP，IOC 和 DI 有什么不同?

AO P

将通用需求功能从不相关类之中分离出来；同时，能够使得很多类共享一个行为，一旦行为发生变化，不必修改很多类，只要修改这 个行为就可以

控制反转和依 赖注入是同一个概念



## 第 3 章 关键字


### 1.”static” 关键字是什么意思？Java 中是否可以覆盖(override) 一个 private 或者是 static 的方法？

Java 中 static 方法不能被覆盖，因为**方法覆盖是基于运行时动态绑定**的，而 static 方法是编译时静态绑定的。s tatic 方法跟类的任何实例都不相关，所以概念上不适用。


### 3.访问修饰符 public, private, protected, 以及不写（默认）时的区别？

### 4.volatile关键字是否能保证线程安全？

答案：不能 

解析：volatile 关键字用在多线程同步中，可**保证读取的可见性**，JVM只是保证从主内存加载到线程工作内存的 值是最新的读取值，而非 cache 中。但多个线程对 volatile 的写操作，无法保证线程安全。例如假如线程 1，线 程 2 在进行 read,load 操作中，发现主内存中 count 的值都是 5，那么都会加载这个最新的值，在线程 1 对 count 进行修改之后，会 write 到主内存中，主内存中的 count 变量就会变为 6；线程 2 由于已经进行 read,load 操 作，在进行运算之后，也会更新主内存 count 的变量值为 6；导致两个线程及时用 volatile 关键字修改之后，还 是会存在并发的情况。

### 5.Java 有没有 goto?

答：goto 是 Java 中的保留字，在目前版本的 Java 中没有使用。


### 6.Java 中的 final关键字有哪些用法？



### 7.什么时候用 assert？


### 

断言用于调试目的： assert(a > 0); // throws an AssertionError if a <= 0 断言可以有两种形式： assert Expression1; assert Expression1 : Expression2 ; Expression1 应该总是产生一个布尔值 Expression2 可以是得出一个值的任意表达式；这个值用于生成显示更多调试信息的字符串消息


### 2.用最有效率的方法算出 2 乘以 8 等於几?

### 3.存在使 i + 1 < i的数吗? 

答案：存在 解析：如果 i 为 int 型，那么当 i 为 int 能表示的最大整数时

i+1 就溢出变成负数了

扩展：存在使 i > j || i <= j 不成立的数吗? 答案：存在 解析：比如 Double.NaN 或 Float.NaN 。


### 4.0.6332 的数据类型是（）

### 5.System.out.println("5" + 2);的输出结果应该是（）。

A.52

Java 会自动将 2 转换为字符串。


### 8.int 和 Integer 有什么区别?

### 9.char 型变量中能不能存贮一个中文汉字?为什么?

char 类型可以存储一个中文汉字，因为 Java 中使用的编码是 Unicode（不选择任何特定的编码，直接使用 字符在字符集中的编号，这是统一的唯一方法），一个 char 类型占 2 个字节（16bit），所以放一个中文是没问 题的。

### 10.Math.round(11.5) 等于多少? Math.round(-11.5)等于多少?

参数加 1/ 2 后求其 floor

：Math.round(11.5)==12 Math.round(-11.5)==-11



## 第 5 章 字符串与数组

### 1.下面程序的运行结果是（）

下面程序的运行结果是（） 

```java
String str1 = "hello"; 
String str2 = "he" + new String("llo"); 
System.err.println(str1 == str2);
```

 答案：false 解析：因为 str2 中的 llo 是新申请的内存块，而 == 判断的是对象的地址而非值，所以不一样。如果是 2 = str1 ，那么就是 true 了。


### 

### 2.下面代码的运行结果为?

String s; System.out.println("s=" + s);

C.由于 String s 没有初始化，代码不能编译通过


### 3.String 是最基本的数据类型吗?

：不是。Java 中的基本数据类型只有 8 个：byte、short、int、long、float、double、char、boolean；除 了基本类型（primitive type）和枚举类型（enumeration type），剩下的都是引用类型（reference type）。

### 4.数组有没有 length() 方法? String 有没有 length() 方法？

 答：数组没有 length()方法，有 length 的属性。String 有 length()方法。


### 

### 5.是否可以继承 String 类?

答：String 类是 final 类，不可以被继承。


### 

### 6.String 和StringBuilder、StringBuffer 的区别?

其中 String 是只读字符串，也就意味着 String 引用的字符串内容是不能被改变的。而 StringBuffer 和 Stri ngBuilder 类表示的字符串对象可以直接进行修改。

StringBuilder 是 JDK 1.5 中引入的，它和 StringBuffer 的 方法完全相同，区别在于它是在单线程环境下使用的，因为它的所有方面都没有被 synchronized 修饰，因此它 的效率也比 StringBuffer 略高。

### 7.String s=new String(“xyz”);创建了几个字符串对象？

答：两个对象，一个是静态存储区的"xyz",一个是用new创建在堆上的对象。

### 8.将字符 “12345” 转换成 long 型 

解答: String s=”12345″; long num=Long.valueOf(s).longValue();


### 10.String s = "Hello";s = s + " world!”; 这两行代码执行后，原始的 String 对象中的内容到底变了没有？

### 11.如何把一段逗号分割的字符串转换成一个数组?

用正则表达式，代码大概为： String [] result = orgStr.split(“,”);

### 12.下面这条语句一共创建了多少个对象： String s=“a”+”b”+”c”+”d”;

String s=“a”+”b”+”c”+”d”;

**代码被编译器在编译时优化**后，相当于直接定义了一个”abcd”的字符串，所以，上面的代码应 该只创建了一个 String 对象。写如下两行代码， String s ="a" + "b" + "c" + "d"; System.out.println(s== "abcd"); 最终打印的结果应该为 true。


### 

### 2.ArrayList list = new ArrayList(20);中的 list 扩充几次?

解析：这里有点迷惑人，大家都知道默认 ArrayList 的长度是 10 个，所以如果你要往 list 里添加 20 个元素肯定 要扩充一次（扩充为原来的 1.5 倍），但是这里显示指明了需要多少空间，所以就一次性为你分配这么多空 间，也就是不需要扩充了。


### 5.什么是迭代器(Iterator)？

### 6.Iterator和ListIterator的区别是什么？

Iterator 可用来遍历 Set 和 List 集合，但是 ListIterator 只能用来遍历 List 。 Iterator 对集合只能是前向遍历，ListIterator 既可以前向也可以后向。 ListIterator 实现了 Iterator 接口，并包含其他的功能，比如：增加元素，替换元素，获取前一个和后一个元素的 索引，等等。

### 7.快速失败(fail-fast)和安全失败(fail-safe)的区别是什么？

java.util 包下面的所有的集合类都是快速失败的，而 java.util.concurrent 包下面的所有的类都是安全失败的

快速失败的迭代器会抛出 ConcurrentModificationException 异常，而安全失败的迭代器永远不会抛出这样的异常。

注：用 iterator.remove 应该不会引起异常


### 9.hashCode() 和 equals() 方法的重要性体现在什么地方？

### 10.HashMap 和 Hashtable 有什么区别？

HashMap 和 Hashtable 都实现了 Map 接口，因此很多特性非常相似。但是，他们有以下不同点： HashMap 允许键和值是 null，而 Hashtable 不允许键或者值是 null。 Hashtable 是同步的，而 HashMap 不是。因此， HashMap 更适合于单线程环境，而 Hashtable 适合于多线 程环境。 HashMap 提供了可供应用迭代的键的集合，因此，HashMap 是快速失败的。另一方面，Hashtable 提供了对 键的列举(Enumeration)。 一般认为 Hashtable 是一个遗留的类。

### 11.数组(Array)和列表(ArrayList)有什么区别？什么时候应该使用 Array 而不是 ArrayList？

Array 可以包含基本类型和对象类型，ArrayList 只能包含对象类型。

Array 大小是固定的，ArrayList 的大小是动态变化的。

ArrayList 提供了更多的方法和特性，比如：addAll()，removeAll()，iterator()等等。

### 12.ArrayList 和 LinkedList 有什么区别？


### 

ArrayList 是基于索引的数据接口，它的底层是数组

以O(1)时间复杂度对元素进行随机访问

，LinkedList 是以元素列表的形式存储它的数据

查找某个元素的时间复杂度是O(n)

LinkedList 比 ArrayList 更占内存，因为 LinkedList 为每一个节点存储了两个引用，一个指向前一个元素，一 个指向下一个元素。

### 13.Comparable 和Comparator 接口是干什么的？列出它们的区别。

包含一个 compareTo(x) 方法的 Comparable 接口

包含 compare(a, b) 和 equals() 两个方法的 Comparator 接口


### 15.Enumeration 接口和 Iterator 接口的区别有哪些？

### 16.HashSet 和 TreeSet 有什么区别？


### 

HashSet 是由一个 hash 表来实现的，因此，它的元素是无序的。add()，remove()，contains()方法的时间复 杂度是 O(1)。 另一方面，TreeSet 是由一个树形的结构来实现的，它里面的元素是有序的。因此，add()，remove()，contains() 方法的时间复杂度是 O(logn)。

### 17.List、Set、Map 是否继承自 Collection 接口？

答：List、Set 是，Map 不是



## 第 8 章 Java 平台与内存管理

### 1.GC线程是否为守护线程？（）

答案：是 解析：线程分为守护线程和非守护线程（即用户线程）。 只要当前JVM实例中尚存在任何一个非守护线程没有结束，守护线程就全部工作；只有当最后一个非守护线程结 束时，守护线程随着 JVM 一同结束工作。 守护线程最典型的应用就是 GC (垃圾回收器)


### 2.解释内存中的栈（stack）、堆(heap)和静态存储区的用法。

### 3.Java 中会存在内存泄漏吗


### 

理论上 Java 因为有垃圾回收机制（GC）不会存在内存泄露问题

在实际开发中，可能会存在无用但可达的对象，这些对象不能被 GC 回收也会发生内存 泄露


### 4.GC 是什么？为什么要有 GC？

### 5.第 3 行中生成的 object在第几行执行后成为 garbage collection 的对象？

```java
1.public class MyClass { 
  2.public StringBuffer aMethod() { 
    3.StringBuffer sf = new StringBuffer(“Hello”); 
    4.StringBuffer[] sf_arr = new StringBuffer[1]; 
    5.sf_arr[0] = sf; 
    6.sf = null; 
    7.sf_arr[0] = null; 
    8.return sf; 
   9.} 
10.} 
```

答：第 7 行

sf 和 sf_arr[0] 都指向 那个对象，都不可达时才会被判定为垃圾



### 2.下面程序的运行结果?

Thread 类中 start() 和 run() 方法的区别了。start() 用来启动一个线程，当调用 start 方法 后，系统才会开启一个新的线程，进而调用 run() 方法来执行任务，而单独的调用run() 就跟调用普通方法是一样 的，已经失去线程的特性了。因此在启动一个线程的时候一定要使用 start() 而不是 run()。


### 3.进程和线程的区别是什么？

### 4.创建线程有几种不同的方式？你喜欢哪一种？为什么？

有三种方式可以用来创建线程：

 • 继承 Thread 类 

• 实现 Runnable 接口 

• 应用程序可以使用 Executor 框架来创建线程池 

实现 Runnable 接口这种方式更受欢迎，因为这不需要继承 Thread 类。在应用设计中已经继承了别的对象的情 况下，这需要多继承（而 Java 不支持多继承），只能实现接口。同时，线程池也是非常高效的，很容易实现和 使用。


### 9.如何确保 N 个线程可以访问 N 个资源同时又不导致死锁？

### 10.sleep() 和 wait() 有什么区别?


### 

调用 sleep 不会释放对象锁

wait()方法导致本线程放弃对象锁

只有针对此对象发出 notify 方法（或 notifyAll）后本线程才进入对象锁定池准备获得对象锁进 入就绪状态

### 11.sleep() 和 yield() 有什么区别?

有什么区别? 有什么区别? 答： ① sleep() 方法给其他线程运行机会时不考虑线程的优先级，因此会给低优先级的线程以运行的机会；yield() 方 法只会给相同优先级或更高优先级的线程以运行的机会； ② 线程执行 sleep() 方法后转入阻塞（blocked）状态，而执行 yield() 方法后转入就绪（ready）状态； ③ sleep() 方法声明抛出InterruptedException，而 yield() 方法没有声明任何异常； ④ sleep() 方法比 yield() 方法（跟操作系统相关）具有更好的可移植性。


### 16.启动一个线程是用 run() 还是 start() 方法?

### 17.什么是线程池（thread pool）？

线程池顾名思义就是事先创建若干个可执行的线程放入一个池（容器）中，需要的时候 从池中获取线程不用自行创建，使用完毕不需要销毁线程而是放回池中，从而减少创建和销毁线程对象的开销。



### 1.下列属于关系型数据库的是（）

### 2.在进行数据库编程时，连接池有什么作用？

尤其是数据库服务器不在本地时，每次建立连接都需要进行 TCP 的三次握手，再加上网络延迟，造成的开销是不可忽视的

创建连接和释放连接都有很大的开销

为了提升系统访问数据库的性能，可以事先创建若 干连接置于连接池中，需要时直接从连接池获取，使用结束时归还连接池而不必关闭连接，从而避免频繁创建和 释放连接所造成的开销，这是典型的用空间换取时间的策略

基于 Java 的开源数 据库连接池主要有： C3P0、Proxool、DBCP、BoneCP、Druid等

池化技术在 Java 开发中是很常见的，在使用线程时创建线程池的道理与此相同

### 3.什么是 DAO 模式？


### 

DAO（DataAccess Object）顾名思义是一个为数据库或其他持久化机制提供了抽象接口的对象，在不暴露数据库实现细节的前提下提供了各种数据操作

### 4.什么是ORM？

对象关系映射（Object-Relational Mapping，简称 ORM）

ORM 是通过使用描述对象和数据库之间映射的元数据（可以用 X ML 或者是注解），将 Java 程序中的对象自动持久化到关系数据库中或者将关系数据库表中的行转换成 Java 对 象，其本质上就是将数据从一种形式转换到另外一种形式。

### 5.JDBC 中如何进行事务处理？


### 

通过调用setAutoCommit(false)可以设置手动提交事务；当事务完成 后用 commit()显式提交事务；如果在事务处理过程中发生异常则通过 rollback() 进行事务回滚


### 6.事务的 ACID 是指什么？

### 7.使用 JDBC 操作数据库时，如何提升读取数据的性能？如何提升更新数据的 性能？

 答：要提升读取数据的性能，可以指定通过结果集（ResultSet）对象指定每次抓取数据的大小（fetch size）；要提升更新数据的性能可以使用PreparedStatement语句构建批处理（batch）


### 

### 9.你认为在表上建立索引可以提高数据库系统的效率吗，为什么？

 答：不一定 建立太多的索引将会影响更新和插入的速度，因为它需要同样更新每个索引文件。对于一个经常需要更新和插入 的表格，就没有必要为一个很少使用的 where 子句单独建立索引了，对于比较小的表，排序的开销不会很大，也 没有必要建立另外的索引。





## Java 面试笔记 - v1.pdf 的批注摘要。

### Path 与 Classpath?

Path 和 Classpath 是操作系统的环境变量. 

• Path 定义了系统可以在哪里找到可执行文件(.exe) 

• classpath 定义了 .class 文件的位置.

### Ear, Jar 和 War 文件的区别?

• Jar files are intended to hold generic libraries of Java classes, resources, etc. 

• War files are intended to contain complete Web applications. 

• Ear files are intended to contain complete enterprise applications.

### 什么是 AOP

它可以运行期动态代理实现在不修改源代码的情况下给程序动态统一添加功能的一种技术它可以运行期动态代理实现在不修改源代码的情况下给程序动态统一添加功能的一种技术

### statement 和 prepared statement

Statement每次执行sql语句,数据库都要执行sql语句的编译. 最好用于仅执行一次查询并返回结果的情形，效率高于PreparedStatement

Prepared statements offer better performance, as they are pre-compiled e SQL statements many timestimes.

Prepared statements are more secure because they use bind variables, which can prevent SQL injection attack.





## 单例模式

[Java: 单例模式我只推荐两种](https://juejin.im/post/6844903858276139021)

```java
public class Singleton{
  private volatile static Singleton singleton;
  private Singleton(){}
  public Singleton getInstance(){
    if(singleton==null){
      synchronized(singleton){
        if(singleton==null){
          singleton = new Singleton();
        }
      }
    }
    return singleton;
  }
}
```



```java
public class Singleton{
  private Singleton(){}
  private static SingletonContainer(){
    private static final singleton = new Singleton();
  }
  public Singleton getInstance(){
    return SingletonContainer.singleton;
  }
}
```





### 
