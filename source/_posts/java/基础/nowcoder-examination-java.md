---
title: nowcoder-examination-java
date: 2019-12-02 18:00:00
categories:
- java
- 基础
toc: true
tags:
- nowcoder
- examination
- java
---
# 2019-12-05 06 07
## static不能用来修饰类
除非类是内部类，此时该类作为外部类的成员变量，可以用static来修饰
## 类方法<->静态方法
静态方法中不能调用对象的变量，因为静态方法在类加载时就初始化，对象变量需要在新建对象后才能使用
## 对象空间被收集前执行finalize（）方法
而不是对象空间被收集之后再执行，如果这样的话执行finalize（）就没有意义了。
## 关于继承和实现
1.类与类之间的关系为继承，只能单继承，但可以多层继承。 
2.类与接口之间的关系为实现，既可以单实现，也可以多实现。 
3.**接口与接口之间的关系为继承，既可以单继承，也可以多继承**。 
## 面向对象方法的多态性是指
相同类型的变量、调用同一个方法时呈现出多种不同的行为特征
## Hashtable 和 HashMap 的区别是
HashMap 是内部基于哈希表实现，该类继承AbstractMap，实现Map接口
Hashtable 线程安全的，而 HashMap 是线程不安全的
Properties 类 继承了 Hashtable 类，而 Hashtable 类则继承Dictionary 类
HashMap允许将 null 作为一个 entry 的 key 或者 value，而 Hashtable 不允许。
> Hashtable：
> （1）Hashtable 是一个散列表，它存储的内容是键值对(key-value)映射。
> （2）Hashtable 的函数都是同步的，这意味着它是线程安全的。它的key、value都不可以为null。
> （3）HashTable直接使用对象的hashCode。
> HashMap：
> （1）由数组+链表组成的，基于哈希表的Map实现，数组是HashMap的主体，链表则是主要为了解决哈希冲突而存在的。
> （2）不是线程安全的，HashMap可以接受为null的键(key)和值(value)。
> （3）HashMap重新计算hash值

## java中将ISO8859-1字符串转成GB2312编码，语句为 ？  
new String("ISO8859-1".getBytes("ISO8859-1"),"GB2312")
## java如何接受request域中的参数？
request.getParameter()
## JAVA的跨平台特性表述为“一次编译，到处运行”
Java的跨平台特性是因为JVM的存在， 它可以执行.class字节码文件，而不是.java源代码
## 下列关于JAVA多线程的叙述正确的是（）
正确答案: B C   你的答案: A B D (错误)
调用start()方法和run()都可以启动一个线程
**CyclicBarrier和CountDownLatch都可以让一组线程等待其他线程**
**Callable类的call()方法可以返回值和抛出异常**
新建的线程调用start()方法就能立即进行运行状态
> A，start是开启线程，run是线程的执行体，run是线程执行的入口。
> B，CyclicBarrier和CountDownLatch都可以让一组线程等待其他线程。前者是让一组线程相互等待到某一个状态再执行。后者是一个线程等待其他线程结束再执行。
> C，Callable中的call比Runnable中的run厉害就厉害在有返回值和可以抛出异常。同时这个返回值和线程池一起用的时候可以返回一个异步对象Future。
> D，start是把线程从new变成了runnable

## 

# 2019-12-04
## 下面的程序输出的结果是( )

```java
public class A implements B{
public static void main(String args[]){
    int i;
    A a1=new  A();
    i =a1.k;
    System.out.println("i="+i);
    }
}
interface B{
    int k=10；

}
```

正确答案: i=10
> 在接口里面的变量默认都是public static final 的，它们是公共的,静态的,最终的常量.相当于全局常量，可以直接省略修饰符。
> 实现类可以直接访问接口中的变量

## 以下叙述正确的是

正确答案: D   你的答案: B (错误)
实例方法可直接调用超类的实例方法
实例方法可直接调用超类的类方法、
实例方法可直接调用子类的实例方法
实例方法可直接调用本类的实例方法
> A错误，类的实例方法是与该类的实例对象相关联的，不能直接调用，只能通过创建超类的一个实例对象，再进行调用
> B错误，**当父类的类方法定义为private时，对子类是不可见的，所以子类无法调用**
> C错误，子类具体的实例方法对父类是不可见的，所以无法直接调用， 只能通过创建子类的一个实例对象，再进行调用
> D正确，**实例方法可以调用自己类中的实例方法**

## 下面代码的执行结果是 :

```java
class Chinese{
    private static Chinese objref =new Chinese();
    private Chinese(){}
    public static Chinese getInstance() { return objref; }
}

public class TestChinese {
    public static void main(String [] args) {
    Chinese obj1 = Chinese.getInstance();
    Chinese obj2 = Chinese.getInstance();
    System.out.println(obj1 == obj2);
}
}
```
正确答案: true
> 饿汉式单例模式，在类创建时，就已经实例化完成，在调用Chinese.getInstance()时，直接获取静态对象

## JavaWEB中有一个类，当会话种邦定了属性或者删除了属性时，他会得到通知，这个类是：
HttpSessionAttributeListener

## 以下哪几种方式可用来实现线程间通知和唤醒：( )

```java
Object.wait/notify/notifyAll
Condition.await/signal/signalAll
```
> wait()、notify()和notifyAll()是 Object类 中的方法 ；
> Condition是在java 1.5中才出现的，它用来替代传统的Object的wait()、notify()实现线程间的协作，相比使用Object的wait()、 notify()，使用Condition1的await()、signal()这种方式实现线程间协作更加安全和高效。

# 2019-12-03
## ArrayList是可改变大小的数组，而LinkedList是双向链接串列
<!--more-->
## 命令javac-d参数的用途是？
指定编译后类层次的根目录
> -d destination 目的地
> -s source 起源地
> javac -d 指定放置生成的类文件的位置
> javac -s 指定放置生成的源文件的位置

## 以下代码执行后输出结果为（ ）
public class Test {
```java
    public static void main(String[] args) {
        System.out.println("return value of getValue(): " +
        getValue());
    }
     public static int getValue() {
         try {
             return 0;
         } finally {
             return 1;
         }
     }
 } 
```
> 当try，catch和finally中都有return时，先执行try中return，并暂存，再执行finnally中并返回最新的值

## 在jdk1.5之后，下列 java 程序输出结果为______。
```java
int i=0;
Integer j = new Integer(0);
System.out.println(i==j);
System.out.println(j.equals(i));
```
true,true
> 1、基本型和基本型封装型进行“==”运算符的比较，基本型封装型将会自动拆箱变为基本型后再进行比较，因此Integer(0)会自动拆箱为int类型再进行比较，显然返回true；
> 2、基本型封装类型调用equals(),但是参数是基本类型，这时候，先会进行自动装箱，基本型转换为其封装类型，再进行3中的比较。

## 说明输出结果。

```java
ackage test;
import java.util.Date; 
public class SuperTest extends Date{ 
    private static final long serialVersionUID = 1L; 
    private void test(){ 
       System.out.println(super.getClass().getName()); 
    } 
      
    public static void main(String[]args){ 
       new SuperTest().test(); 
    } 
}
```
test.SuperTest
> super.getClass().getName()
> 返回的是test.SuperTest，与Date类无关
> 要返回Date类的名字需要写super.getClass().getSuperclass()

## 以下哪些类是线程安全的（）
正确答案: A D E   你的答案: 空 (错误)
Vector
HashMap
ArrayList
StringBuffer
Properties
> A，Vector相当于一个线程安全的List
> B，HashMap是非线程安全的，其对应的线程安全类是HashTable
> C，Arraylist是非线程安全的，其对应的线程安全类是Vector
> D，StringBuffer是线程安全的，相当于一个线程安全的StringBuilder
> E，Properties实现了Map接口，是线程安全的



# 2019-12-01
## javac的作用是（    ）。
将源程序编译成字节码

```
javac helloworld.java
java helloworld
```

## HashMap的数据结构是怎样的
数组+链表

> JDK8以后，HashMap的数据结构是数组+链表+红黑树
> HashMap内部包含了一个默认大小为 16 Entry 类型的数组 table,其中每个Entry 是一个链表，当链表长度大于等于 8 时会将链表转换为红黑树。
> HashMap 由数组+链表组成的，数组是 HashMap 的主体，链表则是主要为了解决哈希冲突而存在的

<!--more-->
## 下面关于垃圾收集的描述哪个是错误的？
a 使用垃圾收集的程序不需要明确释放对象
b 现代垃圾收集能够处理循环引用问题
c 垃圾收集能提高程序员效率
d **使用垃圾收集的语言没有内在泄漏问题**
也会有内存泄露问题，例如访问资源文件，流不关闭，访问数据库等连接不关闭

## 重载的方法就是形参列表的不同，和返回值无关
## 用户不能调用构造方法，只能通过new关键字自动调用。【X】
> 在类内部可以用户可以使用关键字this.构造方法名()调用（参数决定调用的是本类对应的构造方法）
> 在子类中用户可以通过关键字super.父类构造方法名()调用（参数决定调用的是父类对应的构造方法。）
> 反射机制对于任意一个类，都能够知道这个类的所有属性和方法，包括类的构造方法。

## 下面有关JSP内置对象的描述，说法错误的是？
正确答案: C   你的答案: 空 (错误)
session对象：session对象指的是客户端与服务器的一次会话，从客户连到服务器的一个WebApplication开始，直到客户端与服务器断开连接为止
request对象：客户端的请求信息被封装在request对象中，通过它才能了解到客户的需求，然后做出响应
application对象：~~多个~~一个application对象实现了用户间数据的共享，可存放全局变量
response对象：response对象包含了响应客户请求的有关信息

## volatile能保证数据的可见性，但不能完全保证数据的原子性，synchronized即保证了数据的可见性也保证了原子性
## java程序内存泄露的最直接表现是（ ）
正确答案: C   你的答案: 空 (错误)
频繁FullGc
jvm崩溃
程序抛内存控制的Exception
java进程异常消失
> java是自动管理内存的，通常情况下程序运行到稳定状态，内存大小也达到一个 基本稳定的值
> 但是内存泄露导致Gc不能回收泄露的垃圾，内存不断变大.
> 最终超出内存界限，抛出OutOfMemoryExpection

## 下面有关webservice的描述，错误的是？
正确答案: B   你的答案: 空 (错误)
Webservice是跨平台，跨语言的远程调用技术
Webservice通信机制实质就是json数据交换
Webservice采用了soap协议（简单对象协议）进行通信
WSDL是用于描述 Web Services 以及如何对它们进行访问
> Web service顾名思义是基于web的服务，它是一种跨平台，跨语言的服务。
> 我们可以这样理解它，比如说我们可以调用互联网上查询天气信息的web服务，把它嵌入到我们的B/S程序中，当用户从我们的网点看到天气信息时，会认为我们为他提供很多的服务，但其实我们什么也没做，只是简单的调用了一下服务器上的一端代码而已。Web service 可以将你的服务发布到互联网上让别人去调用，也可以调用别人发布的web service，和使用自己的代码一样。
> 它是采用XML传输格式化的数据，它的通信协议是SOAP(简单对象访问协议).

## java用（）机制实现了进程之间的同步执行
正确答案: A   你的答案: C (错误)
监视器
虚拟机
多个CPU
异步调用
> 首先jvm中没有进程的概念 ，但是jvm中的线程映射为操作系统中的进程，对应关系为1：1。那这道题的问的就是jvm中线程如何异步执行 。  在jvm中 是使用监视器锁来实现不同线程的异步执行，  在语法的表现就是synchronized。

## 关于访问权限说法正确的是？
类定义前面可以修饰public,protected和private
内部类前面可以修饰public,protected和private
局部内部类前面可以修饰public,protected和private
以上说法都不正确
> 对于外部类来说，只有两种修饰，public和默认（default），因为外部类放在包中，只有两种可能，包可见和包不可见。
> 在Java中，可以将一个类定义在另一个类里面或者一个方法里边，这样的类称为内部类，广泛意义上的内部类一般包括四种：成员内部类，局部内部类，匿名内部类，静态内部类 。
> 1.成员内部类
> （1）该类像是外部类的一个成员，可以无条件的访问外部类的所有成员属性和成员方法（包括private成员和静态成员）；
> （2）成员内部类拥有与外部类同名的成员变量时，会发生隐藏现象，即默认情况下访问的是成员内部类中的成员。如果要访问外部类中的成员，需要以下形式访问：【外部类.this.成员变量  或  外部类.this.成员方法】；
> （3）在外部类中如果要访问成员内部类的成员，必须先创建一个成员内部类的对象，再通过指向这个对象的引用来访问；
> （4）成员内部类是依附外部类而存在的，也就是说，如果要创建成员内部类的对象，前提是必须存在一个外部类的对象；
> （5）内部类可以拥有private访问权限、protected访问权限、public访问权限及包访问权限。如果成员内部类用private修饰，则只能在外部类的内部访问；如果用public修饰，则任何地方都能访问；如果用protected修饰，则只能在同一个包下或者继承外部类的情况下访问；如果是默认访问权限，则只能在同一个包下访问。外部类只能被public和包访问两种权限修饰。
> 2.局部内部类
> （1）局部内部类是定义在一个方法或者一个作用域里面的类，它和成员内部类的区别在于局部内部类的访问仅限于方法内或者该作用域内；
> （2）局部内部类就像是方法里面的一个局部变量一样，是不能有public、protected、private以及static修饰符的。
> 3.匿名内部类
> （1）一般使用匿名内部类的方法来编写事件监听代码；
> （2）匿名内部类是不能有访问修饰符和static修饰符的；
> （3）匿名内部类是唯一一种没有构造器的类；
> （4）匿名内部类用于继承其他类或是实现接口，并不需要增加额外的方法，只是对继承方法的实现或是重写。
> 4.内部静态类
> （1）静态内部类是不需要依赖于外部类的，这点和类的静态成员属性有点类似；
> （2）不能使用外部类的非static成员变量或者方法。

## throw thorws
1、throws出现在方法头，throw出现在方法体 2、throws表示出现异常的一种可能性，并不一定会发生异常；throw则是抛出了异常，执行throw则一定抛出了某种异常。 3、两者都是消极的异常处理方式，只是抛出或者可能抛出异常，是不会由函数处理，真正的处理异常由它的上层调用处理。

## 以下可以正确获取结果集的有
正确答案: A D   你的答案: 空 (错误)

```java
Statement sta=con.createStatement();
ResultSet rst=sta.executeQuery(“select * from book”);

Statement sta=con.createStatement(“select * from book”); ResultSet rst=sta.executeQuery();

PreparedStatement pst=con.prepareStatement();
ResultSet rst=pst.executeQuery(“select * from book”);

PreparedStatement pst=con.prepareStatement(“select * from book”);
ResultSet rst=pst.executeQuery();
```
> 创建Statement是不传参的，PreparedStatement是需要传入sql语句
> 1、PreparedStatement 继承 Statement，PreparedStatement 实例包含已编译的 SQL 语句， 所以其执行速度要快于 Statement 对象。
> 2、作为 Statement 的子类，PreparedStatement 继承了 Statement 的所有功能。三种方法execute、 executeQuery 和 executeUpdate 已被更改以使之不再需要参数
> 3、PreparedStatement尽最大可能提高性能. 最重要的一点是极大地提高了安全性.

## final、finally和finalize的区别中，下述说法正确的有？
正确答案: A B   你的答案: A B C D (错误)
final用于声明属性，方法和类，分别表示属性不可变，方法不可覆盖，类不可继承。
finally是异常处理语句结构的一部分，表示总是执行。
finalize是Object类的一个方法，在垃圾收集器执行的时候会调用被回收对象的此方法，可以覆盖此方法提供垃圾收集时的其他资源的回收，例如关闭文件等。
引用变量被final修饰之后，不能再指向其他对象，它指向的对象的内容也是不可变的。

> A，D考的一个知识点，final修饰变量，变量的引用（也就是指向的地址）不可变，但是引用的内容可以变（地址中的内容可变）。
> B，finally表示总是执行。但是其实finally也有不执行的时候，但是这个题不要扣字眼。
> 1. 在try中调用System.exit(0)，强制退出了程序，finally块不执行。
> 2. 在进入try块前，出现了异常，finally块不执行。
> C，finalize方法，这个选项错就错在，这个方法一个对象只能执行一次，只能在第一次进入被回收的队列，而且对象所属于的类重写了finalize方法才会被执行。第二次进入回收队列的时候，不会再执行其finalize方法，而是直接被二次标记，在下一次GC的时候被GC。
> 
> [放一张图吧](https://uploadfiles.nowcoder.com/images/20180716/3807435_1531748778229_B1F90475F3162B313B750B56294240E0)
