---
title: Java基础
date: 2020-01-12 17:13:47
categories:
- java
- 基础
toc: true
tags:
- Java
---
[Java 基础](https://github.com/ZhongFuCheng3y/3y#coffeejava%E5%9F%BA%E7%A1%80)

<!--more-->

[toc]

## 2018 年如何快速学 Java

学习 JavaWeb 的路线如下：
Tomcat(简单过一下)
XML/注解(简单过一下)
**Servlet(重点理解)
HTTP 协议(重点理解)
Filter 过滤器(重点理解)**
Listener 监听器(简单过一下)
JSP(简单过一下)
AJAX、JSON(简单过一下)

## 泛型就这么简单

### 为什么需要泛型

> 首先，我们来试想一下：没有泛型，集合会怎么样
> Collection、Map 集合对元素的类型是没有任何限制的。本来我的 Collection 集合装载的是全部的 Dog 对象，但是外边把 Cat 对象存储到集合中，是没有任何语法错误的。
> 把对象扔进集合中，集合是不知道元素的类型是什么的，仅仅知道是 Object。因此在 get()的时候，返回的是 Object。外边获取该对象，还需要强制转换
> 有了泛型以后：
> 代码更加简洁【不用强制转换】
> 程序更加健壮【只要编译时期没有警告，那么运行时期就不会出现 ClassCastException 异常】
> 可读性和稳定性【在编写集合的时候，就限定了类型】

#### 3.1 泛型类

```Java
public class ObjectTool<T> {
    private T obj;

    public T getObj() {
        return obj;
    }

    public void setObj(T obj) {
        this.obj = obj;
    }
}
```

#### 3.2 泛型方法

```Java
//定义泛型方法..
public <T> void show(T t) {
    System.out.println(t);

}
// 使用
// 用户传递进来的是什么类型，返回值就是什么类型了
public static void main(String[] args) {
    //创建对象
    ObjectTool tool = new ObjectTool();

    //调用方法,传入的参数是什么类型,返回值就是什么类型
    tool.show("hello");
    tool.show(12);
    tool.show(12.5);

}
```

#### 3.4 类型通配符

有个需求：方法接收一个集合参数，遍历集合并把集合元素打印出来

```java
// 1. 未指定类型
// public void test(List list){
// 2. 只能接收装着object的list
// public void test(List<Object> list){
// 3. ? 表示任意类型
public void test(List<?> list){
    for(int i=0;i<list.size();i++){

        System.out.println(list.get(i));

    }
}
```

注意： 使用？时，只能调用与类型无关的方法

##### 3.4.1 类型通配符的上限

需求：接收一个 List 集合，它只能操作数字类型的元素【Float、Integer、Double、Byte 等数字类型都行】，怎么做？？？
设定通配符上限
List<? extends Number>

```Java
public static void test(List<? extends Number> list) {

}
```

##### 3.4.2 类型通配符的上限

//传递进来的只能是 Type 或 Type 的父类

```java
<? super Type>
```

#### 3.5 通配符和泛型方法

通配符 和 泛型方法 实现的功能很接近，怎么选择？
**原则：**
通配符要求参数之间没有依赖关系

### 四、泛型的应用

baseDAO

<details>
  <summary>点击查看代码</summary>
```java
public abstract class BaseDao<T> {

```
//模拟hibernate....
private Session session;
private Class clazz;


//哪个子类调的这个方法，得到的class就是子类处理的类型（非常重要）
public BaseDao(){
    Class clazz = this.getClass();  //拿到的是子类
    ParameterizedType  pt = (ParameterizedType) clazz.getGenericSuperclass();  //BaseDao<Category>
    clazz = (Class) pt.getActualTypeArguments()[0];
    System.out.println(clazz);
}
public void add(T t){
    session.save(t);
}
public T find(String id){
    return (T) session.get(clazz, id);
}
public void update(T t){
    session.update(t);
}
public void delete(String id){
    T t = (T) session.get(clazz, id);
    session.delete(t);
}
```

}

```


</details>

CategoryDao

```java
public class CategoryDao extends BaseDao<Category> {
}
```

BookDao

```java
public class BookDao extends BaseDao<Book> {
}
```

## 注解就这么简单

### 二、为什么我们需要用到注解？

注解可以给类、方法上注入信息

### 三、基本 Annotation

java.lang 包下存在着 5 个基本的 Annotation

#### 3.1@Overridemailto:3.1@Override

重写

#### 3.2@Deprecatedmailto:3.2@Deprecated

过时
在程序中调用它的时候，在 IDE 上会出现一条横杠，说明该方法是过时的

#### 3.3@SuppressWarningsmailto:3.3@SuppressWarnings

抑制编译器警告

#### 3.4@SafeVarargsmailto:3.4@SafeVarargs

Java 7“堆污染”警告 ？
什么是堆污染呢？？当**把一个不是泛型的集合赋值给一个带泛型的集合**的时候，这种情况就很容易发生堆污染….

#### 3.5@FunctionalInterfacemailto:3.5@FunctionalInterface

@FunctionalInterface 用来指定该接口是函数式接口

### 四、自定义注解基础

#### 4.1 标记 Annotation

没有任何成员变量的注解：标记注解。如@Overrided

```Java
public @interface MyAnnotation{
}
```

#### 4.2 元数据 Annotation

带有成员变量的注解：元数据 annotation。
注解中声明成员变量类似于声明方法

```Java
public @interface MyAnnotation{
    String username();
    int age();
}
```

注意：在注解上定义的成员变量只能是 String、数组、Class、枚举类、注解

#### 4.3 使用自定义注解

##### 4.3.1 常规使用

有一个 add 的方法，需要 username 和 age 参数，我们通过注解来让该方法拥有这两个变量

```Java
@MyAnnotation(username="Adam", age=16)
public void add(String username, int age){

}
```

##### 4.3.2 默认值

注解可以声明默认值

```Java
public @Interface MyAnnotation{
    String username() default "abc";
    int age() default 11;
}
```

修饰的时候就不用给出具体的值

```Java
@MyAnnotation()
public void add(String username, int age){
}
```

##### 4.3.3 注解属性为 value

若注解中只有一个属性 value，则可以不指定 value，直接赋值

```Java
@MyAnnotation("abd")
public void add(String value){
}
```

#### 4.4 把自定义注解的基本信息注入到方法上

?

### 五、JDK 的元 Annotation

5.1@Retentionmailto:5.1@Retention
只能用于修饰其他的 Annotation, 用于指定被修饰的 Annotation 被保留多长时间。?
5.2@Targetmailto:5.2@Target
只能用于修饰其他的 Annotation, 用于指定被修饰的 Annotation 用于修饰哪些程序单元
5.3@Documentedmailto:5.3@Documented
@Documented 用于指定被该 Annotation 修饰的 Annotation 类将被 javadoc 工具提取成文档。
5.4@Inheritedmailto:5.4@Inherited
@Inherited 也是用来修饰其他的 Annotation 的，被修饰过的 Annotation 将具有继承性。。。

### 六、注入对象到方法或成员变量上

6.1 把对象注入到方法上
?
6.2 把对象注入到成员变量
?

## Object 对象你真理解了吗？

### 一、Object 对象简介

主要有一下方法
registerNatives()【底层实现、不研究】
hashCode()
equals(Object obj)
clone()
toString()
notify()
notifyAll()
wait(long timeout)【还有重载了两个】
finalize()

### 二、equals 和 hashCode 方法

重写 equals()方法，就必须重写 hashCode()的方法？
equals()方法默认是比较对象的地址，使用的是 == 等值运算符
hashCode()方法对底层是散列表的对象有提升性能的功能
同一个对象(如果该对象没有被修改)：那么重复调用 hashCode()那么返回的 int 是相同的！
hashCode()方法默认是由对象的地址转换而来的

#### 2.1 equals 和 hashCode 方法重写

一般来说，比较的是对象地址是没有意义的
![7Bqjw4bGxHy9sKF](https://i.loli.net/2020/01/13/7Bqjw4bGxHy9sKF.jpg)

#### 2.2 String 实现的 equals 和 hashCode 方法

String 已经实现了 equals 和 hashCode 方法了，可以直接使用 String.equals()来判断两个字符串是否相等！

### 三、toString 方法

### 四、clone 方法

#### 4.1 clone 用法

如何克隆对象呢？无论是浅拷贝还是深拷贝都是这两步：
1.克隆的对象要实现 Cloneable 接口
2.重写 clone 方法，最好修饰成 public

##### 1.浅拷贝

```Java
public class Person implements Cloneable{
    private Date date;
    
    @Override
    public Object clone() throws ClonNotSupportedException {
        return super.clone();
    }
}
```

##### 2.深拷贝

```Java
public class Person implements Cloneable{
    private Date date;
    
    @Override
    public Object clone() throws CloneNotSupportedException {
        Person person = (Person)super.clone();
        person.date = (Date)date.clone();
        return person;
    }
}
```

### 五、wait 和 notify 方法

无论是 wait、notify 还是 notifyAll()都需要由监听器对象(锁对象)来进行调用
简单来说：他们都是在同步代码块中调用的，否则会抛出异常！
notify()唤醒的是在等待队列的某个线程(不确定会唤醒哪个)，notifyAll()唤醒的是等待队列所有线程

导致 wait()的线程被唤醒可以有 4 种情况
该线程被中断
wait()时间到了
被 notify()唤醒
被 notifyAll()唤醒

调用 wait()的线程会释放掉锁

#### 5.1 为什么 wait 和 notify 在 Object 方法上？

锁对象是任意的，所以这些方法必须定义在 Object 类中

#### 5.2 notify 方法调用后，会发生什么？

#### 5.3 sleep 和 wait 有什么区别？

主要的区别在于 Object.wait()在释放 CPU 同时，释放了对象锁的控制。
而 Thread.sleep()没有对锁释放

### 六、finalize()方法

finalize()方法将在垃圾回收器清除对象之前调用，但该方法不知道何时调用，具有不定性。一般我们都不会重写它~
一个对象的 finalize()方法只会被调用一次

## [JDK10都发布了，nio你了解多少？](https://mp.weixin.qq.com/s?__biz=MzI4Njg5MDA5NA==&mid=2247484235&idx=1&sn=4c3b6d13335245d4de1864672ea96256&chksm=ebd7424adca0cb5cb26eb51bca6542ab816388cf245d071b74891dd3f598ccd825f8611ca20c&scene=21###wechat_redirect)

### 前言

[Java IO，硬骨头也能变软 - 知乎](https://zhuanlan.zhihu.com/p/28286559)
按操作方式分类结构图：
![7cexHj8yiGf3woM](https://i.loli.net/2020/01/13/7cexHj8yiGf3woM.jpg)
按操作对象分类结构图
![hNcX5Oy8v7j9fpn](https://i.loli.net/2020/01/13/hNcX5Oy8v7j9fpn.jpg)

### 二、NIO 快速入门

3 个核心部分
buffer channel selector

#### 2.1buffer 缓冲区和 Channel 管道

Channel 不与数据打交道，它只负责运输数据。与数据打交道的是 Buffer 缓冲区
Channel--> 运输
Buffer--> 数据
相对于传统 IO 而言，流是单向的

##### 2.1.1buffer 缓冲区核心要点

Buffer 是抽象类
ByteBuffer 是使用最多的实现类
核心方法：put get
Buffer 类有 4 个核心属性
Capacity 容量
缓冲区能够容纳的数据元素的最大数量
Limit 上界
缓冲区数据总数
Position 位置
下一个读写的位置
Mark 标记
记录上一次读写的位置

##### 2.1.2buffer 代码演示

```Java
public static void main(String[] args) {

        // 创建一个缓冲区
        ByteBuffer byteBuffer = ByteBuffer.allocate(1024);

        // 看一下初始时4个核心变量的值
        System.out.println("初始时-->limit--->"+byteBuffer.limit());
        System.out.println("初始时-->position--->"+byteBuffer.position());
        System.out.println("初始时-->capacity--->"+byteBuffer.capacity());
        System.out.println("初始时-->mark--->" + byteBuffer.mark());

        System.out.println("--------------------------------------");

        // 添加一些数据到缓冲区中
        String s = "Java3y";
        byteBuffer.put(s.getBytes());

        // 看一下初始时4个核心变量的值
        System.out.println("put完之后-->limit--->"+byteBuffer.limit());
        System.out.println("put完之后-->position--->"+byteBuffer.position());
        System.out.println("put完之后-->capacity--->"+byteBuffer.capacity());
        System.out.println("put完之后-->mark--->" + byteBuffer.mark());
    }
```

![vfZzkiJsea6rXdw](https://i.loli.net/2020/01/13/vfZzkiJsea6rXdw.jpg)
flip() 后，写模式转换为读模式
clear() 后，读模式转换为写模式

##### 2.1.3FileChannel 通道核心要点

Channel 通道只负责传输数据、不直接操作数据的
获取 channel

```Java
 // 1. 通过本地IO的方式来获取通道
        FileInputStream fileInputStream = new FileInputStream("F:\\3yBlog\\JavaEE常用框架\\Elasticsearch就是这么简单.md");

        // 得到文件的输入通道
        FileChannel inchannel = fileInputStream.getChannel();

        // 2. jdk1.7后通过静态方法.open()获取通道
        FileChannel.open(Paths.get("F:\\3yBlog\\JavaEE常用框架\\Elasticsearch就是这么简单2.md"), StandardOpenOption.WRITE);
```

channel 与 buffer 使用示例
![JhsTeUNHRjlBo52](https://i.loli.net/2020/01/13/JhsTeUNHRjlBo52.png)
![YypOBsjvCJzGLVZ](https://i.loli.net/2020/01/13/YypOBsjvCJzGLVZ.png)

##### 2.1.4 直接与非直接缓冲区

？

##### 2.1.5scatter 和 gather、字符集

分散读取(scatter)：将一个通道中的数据分散读取到多个缓冲区中
聚集写入(gather)：将多个缓冲区中的数据集中写入到一个通道中
scatter gather 代码见原文

### 三、IO 模型理解

3.0 学习 I/O 模型需要的基础
3.1 阻塞 I/O 模型
3.2 非阻塞 I/O 模型
3.3I/O 复用模型
3.4I/O 模型总结

### 四、使用 NIO 完成网络通信

#### 4.1NIO 基础继续讲解

NIO 被叫为 no-blocking io，其实是在网络这个层次中理解的，对于 FileChannel 来说一样是阻塞。
通常使用 NIO 是在网络中使用的，网上大部分讨论 NIO 都是在网络通信的基础之上的！说 NIO 是非阻塞的 NIO 也是网络中体现的

在网络中使用 NIO 往往是 I/O 模型的多路复用模型！
Selector 选择器就可以比喻成麦当劳的广播。
通过 selector，一个线程能够管理多个 Channel 的状态
![5CaWiqeOx2JMNUD](https://i.loli.net/2020/01/13/5CaWiqeOx2JMNUD.jpg)

#### 4.2NIO 阻塞形态

#### 4.3NIO 非阻塞形态

在客户端上要想获取得到服务端的数据，也需要注册在 register 上(监听读事件)

<details>
  <summary>点击查看代码</summary>
```java
public class NoBlockClient2 {

```
public static void main(String[] args) throws IOException {

    // 1. 获取通道
    SocketChannel socketChannel = SocketChannel.open(new InetSocketAddress("127.0.0.1", 6666));

    // 1.1切换成非阻塞模式
    socketChannel.configureBlocking(false);

    // 1.2获取选择器
    Selector selector = Selector.open();

    // 1.3将通道注册到选择器中，获取服务端返回的数据
    socketChannel.register(selector, SelectionKey.OP_READ);

    // 2. 发送一张图片给服务端吧
    FileChannel fileChannel = FileChannel.open(Paths.get("X:\\Users\\ozc\\Desktop\\新建文件夹\\1.png"), StandardOpenOption.READ);

    // 3.要使用NIO，有了Channel，就必然要有Buffer，Buffer是与数据打交道的呢
    ByteBuffer buffer = ByteBuffer.allocate(1024);

    // 4.读取本地文件(图片)，发送到服务器
    while (fileChannel.read(buffer) != -1) {

        // 在读之前都要切换成读模式
        buffer.flip();

        socketChannel.write(buffer);

        // 读完切换成写模式，能让管道继续读取文件的数据
        buffer.clear();
    }


    // 5. 轮训地获取选择器上已“就绪”的事件--->只要select()>0，说明已就绪
    while (selector.select() > 0) {
        // 6. 获取当前选择器所有注册的“选择键”(已就绪的监听事件)
        Iterator<SelectionKey> iterator = selector.selectedKeys().iterator();

        // 7. 获取已“就绪”的事件，(不同的事件做不同的事)
        while (iterator.hasNext()) {

            SelectionKey selectionKey = iterator.next();

            // 8. 读事件就绪
            if (selectionKey.isReadable()) {

                // 8.1得到对应的通道
                SocketChannel channel = (SocketChannel) selectionKey.channel();

                ByteBuffer responseBuffer = ByteBuffer.allocate(1024);

                // 9. 知道服务端要返回响应的数据给客户端，客户端在这里接收
                int readBytes = channel.read(responseBuffer);

                if (readBytes > 0) {
                    // 切换读模式
                    responseBuffer.flip();
                    System.out.println(new String(responseBuffer.array(), 0, readBytes));
                }
            }

            // 10. 取消选择键(已经处理过的事件，就应该取消掉了)
            iterator.remove();
        }
    }
}
```

}

```

</details>



服务端
<details>
  <summary>点击查看代码</summary>
```Java
public class NoBlockServer {

    public static void main(String[] args) throws IOException {

        // 1.获取通道
        ServerSocketChannel server = ServerSocketChannel.open();

        // 2.切换成非阻塞模式
        server.configureBlocking(false);

        // 3. 绑定连接
        server.bind(new InetSocketAddress(6666));

        // 4. 获取选择器
        Selector selector = Selector.open();

        // 4.1将通道注册到选择器上，指定接收“监听通道”事件
        server.register(selector, SelectionKey.OP_ACCEPT);

        // 5. 轮训地获取选择器上已“就绪”的事件--->只要select()>0，说明已就绪
        while (selector.select() > 0) {
            // 6. 获取当前选择器所有注册的“选择键”(已就绪的监听事件)
            Iterator<SelectionKey> iterator = selector.selectedKeys().iterator();

            // 7. 获取已“就绪”的事件，(不同的事件做不同的事)
            while (iterator.hasNext()) {

                SelectionKey selectionKey = iterator.next();

                // 接收事件就绪
                if (selectionKey.isAcceptable()) {

                    // 8. 获取客户端的链接
                    SocketChannel client = server.accept();

                    // 8.1 切换成非阻塞状态
                    client.configureBlocking(false);

                    // 8.2 注册到选择器上-->拿到客户端的连接为了读取通道的数据(监听读就绪事件)
                    client.register(selector, SelectionKey.OP_READ);

                } else if (selectionKey.isReadable()) { // 读事件就绪

                    // 9. 获取当前选择器读就绪状态的通道
                    SocketChannel client = (SocketChannel) selectionKey.channel();

                    // 9.1读取数据
                    ByteBuffer buffer = ByteBuffer.allocate(1024);

                    // 9.2得到文件通道，将客户端传递过来的图片写到本地项目下(写模式、没有则创建)
                    FileChannel outChannel = FileChannel.open(Paths.get("2.png"), StandardOpenOption.WRITE, StandardOpenOption.CREATE);
                    while (client.read(buffer) > 0) {
                        // 在读之前都要切换成读模式
                        buffer.flip();
                        outChannel.write(buffer);
                        // 读完切换成写模式，能让管道继续读取文件的数据
                        buffer.clear();
                    }
                }
                // 10. 取消选择键(已经处理过的事件，就应该取消掉了)
                iterator.remove();
            }
        }
    }
}
```

</details>

#### 4.4 管道和 DataGramChannel

## COW 奶牛！Copy On Write 机制了解一下

### 一、Linux 下的 copy-on-write

#### 1.1 简单来用用 fork

fork 用于创建子进程

#### 1.2 再来看看 exec()函数

exec 函数的作用就是：装载一个新的程序（可执行映像）覆盖当前进程内存空间中的映像，从而执行不同的任务

1.3 回头来看 Linux 下的 COW 是怎么一回事
二、解释一下 Redis 的 COW
三、文件系统的 COW
？

## 给女朋友讲解什么是 Optional【JDK 8 特性】

### 一、基础铺垫

#### 1.1Lambda 简化代码例子

##### 创建线程：

```Java
public static void main(String[] args) {
    // 用匿名内部类的方式来创建线程
    new Thread(new Runnable() {
        @Override
        public void run() {
            System.out.println("公众号：Java3y---回复1进群交流");
        }
    });

    // 使用Lambda来创建线程
    new Thread(() -> System.out.println("公众号：Java3y---回复1进群交流"));
}
```

##### 遍历 Map 集合：

```Java
public static void main(String[] args) {
    Map<String, String> hashMap = new HashMap<>();
    hashMap.put("公众号", "Java3y");
    hashMap.put("交流群", "回复1");

    // 使用增强for的方式来遍历hashMap
    for (Map.Entry<String, String> entry : hashMap.entrySet()) {
        System.out.println(entry.getKey()+":"+entry.getValue());
    }

    // 使用Lambda表达式的方式来遍历hashMap
    hashMap.forEach((s, s2) -> System.out.println(s + ":" + s2));
}

```

##### 在 List 中删除某个元素

```java
public static void main(String[] args) {

    List<String> list = new ArrayList<>();
    list.add("Java3y");
    list.add("3y");
    list.add("光头");
    list.add("帅哥");

    // 传统的方式删除"光头"的元素
    ListIterator<String> iterator = list.listIterator();
    while (iterator.hasNext()) {
        if ("光头".equals(iterator.next())) {
            iterator.remove();
        }
    }

    // Lambda方式删除"光头"的元素
    list.removeIf(s -> "光头".equals(s));

    // 使用Lambda遍历List集合
    list.forEach(s -> System.out.println(s));
}
```

#### 1.1 函数式接口

函数式接口的特点：有@FunctionalInterface 注解，接口有且仅有一个抽象方法！
如

```Java
@FunctionalInterface
public interface Runnable {
    public abstract void run();
}
```

### 二、Optional 类

#### 2.1 创建 Optional 容器

创建 Optional 容器有两种方式：
调用 ofNullable()方法，传入的对象可以为 null
调用 of()方法，传入的对象不可以为 null，否则抛出 NullPointerException

#### 2.2Optional 容器简单的方法

```Java
// 得到容器中的对象，如果为null就抛出异常
public T get() {
    if (value == null) {
        throw new NoSuchElementException("No value present");
    }
    return value;
}

// 判断容器中的对象是否为null
public boolean isPresent() {
    return value != null;
}

// 如果容器中的对象存在，则返回。否则返回传递进来的参数
public T orElse(T other) {
    return value != null ? value : other;
}
```

#### 2.3O ptional 容器进阶用法

##### 2.3.1 ifPresent 方法

```Java
public void ifPresent(Consumer<? super T> consumer) {
    if (value != null)
        consumer.accept(value);
}

@FunctionalInterface
public interface Consumer<T> {
    void accept(T t);
}
```

用法

```Java
public static void main(String[] args) {
    User user = new User();
    user.setName("Java3y");
    test(user);
}

public static void test(User user) {
    Optional<User> optional = Optional.ofNullable(user);
    // 如果存在user，则打印user的name
    optional.ifPresent((value) -> System.out.println(value.getName()));
    // 旧写法
    if (user != null) {
        System.out.println(user.getName());
    }
}
```

##### 2.3.2 orElseGet 和 orElseThrow 方法

```Java
public static void main(String[] args) {
    User user = new User();
    user.setName("Java3y");
    test(user);
}

public static void test(User user) {
    Optional<User> optional = Optional.ofNullable(user);
    // 如果存在user，则直接返回，否则创建出一个新的User对象
    User user1 = optional.orElseGet(() -> new User());
    // 旧写法
    if (user != null) {
        user = new User();
    }
}
```

##### 2.3.3 filter 方法

```Java
public static void test(User user) {
    Optional<User> optional = Optional.ofNullable(user);
    // 如果容器中的对象存在，并且符合过滤条件，返回装载对象的Optional容器，否则返回一个空的Optional容器
    optional.filter((value) -> "Java3y".equals(value.getName()));
}
```

##### 2.3.4 map 方法

```Java
public static void test(User user) {
    Optional<User> optional = Optional.ofNullable(user);
    // 如果容器的对象存在，则对其执行调用mapping函数得到返回值。然后创建包含mapping返回值的Optional，否则返回空Optional。
    optional.map(user1 -> user1.getName()).orElse("Unknown");
}
// 上面一句代码对应着最开始的老写法：
public String tradition(User user) {
    if (user != null) {
        return user.getName();
    }else{
        return "Unknown";
    }
}
```

##### 2.3.5 flatMap 方法

##### 2.3.6 总结

<details>
  <summary>点击查看代码</summary>
```Java
public static void main(String[] args) {
    User user = new User();
    user.setName("Java3y");
    System.out.println(test(user));
}

// 以前的代码 v1
public static String test2(User user) {
if (user != null) {
String name = user.getName();
if (name != null) {
return name.toUpperCase();
} else {
return null;
}
} else {
return null;
}
}

// 以前的代码 v2
public static String test3(User user) {
if (user != null && user.getName() != null) {
return user.getName().toUpperCase();
} else {
return null;
}
}

// 现在的代码
public static String test(User user) {
return Optional.ofNullable(user)
.map(user1 -> user1.getName())
.map(s -> s.toUpperCase()).orElse(null);
}

```
</details>

