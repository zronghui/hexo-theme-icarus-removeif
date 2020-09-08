---
title: Java基础-CSNotes
toc: true
recommend: 1
uniqueId: '2020-09-08 15:37:24/"Java基础-CSNotes".html'
date: 2020-09-08 23:37:24
thumbnail:
categories:
tags:
keywords:
---

[TOC]

<!--more-->

\> 本文由 \[简悦 SimpRead\](http://ksria.com/simpread/) 转码， 原文地址 \[cyc2018.github.io\](https://cyc2018.github.io/CS-Notes/#/notes/Java%20%E5%9F%BA%E7%A1%80)

> 缓存池

> new Integer(123) 与 Integer.valueOf(123) 的区别在于：
>
> *   new Integer(123) 每次都会新建一个对象；
> *   Integer.valueOf(123) 会使用缓存池中的对象，多次调用会取得同一个对象的引用。

> Integer x = new Integer(123); Integer y = new Integer(123); System.out.println(x == y); Integer z = Integer.valueOf(123); Integer k = Integer.valueOf(123); System.out.println(z == k);

> valueOf() 方法的实现比较简单，就是先判断值是否在缓存池中，如果在的话就直接返回缓存池的内容。

> public static Integer valueOf(int i) { if (i >= IntegerCache.low && i <= IntegerCache.high) return IntegerCache.cache\[i + (-IntegerCache.low)\]; return new Integer(i); }

> *   StringBuilder 不是线程安全的
> *   StringBuffer 是线程安全的，内部使用 synchronized 进行同步

> [new String("abc")](#/notes/Java%20%E5%9F%BA%E7%A1%80?id=new-stringquotabcquot)
>
> 使用这种方式一共会创建两个字符串对象（前提是 String Pool 中还没有 "abc" 字符串对象）。
>
> *   "abc" 属于字符串字面量，因此编译时期会在 String Pool 中创建一个字符串对象，指向这个 "abc" 字符串字面量；
> *   而使用 new 的方式会在堆中创建一个字符串对象。

> Java 的参数是以值传递的形式传入方法中，而不是引用传递。

> Java 不能隐式执行向下转型，因为这会使得精度降低。
>
> 1.1 字面量属于 double 类型，不能直接将 1.1 直接赋值给 float 变量，因为这是向下转型。

> 1.1f 字面量才是 float 类型。
>
> ```
> float f = 1.1f;
> ```

> 因为字面量 1 是 int 类型，它比 short 类型精度要高，因此不能隐式地将 int 类型向下转型为 short 类型。
>
> ```
> short s1 = 1;
> ```

> 但是使用 += 或者 ++ 运算符会执行隐式类型转换。
>
> ```
> s1 += 1;
> s1++;
> ```
>
> 上面的语句相当于将 s1 + 1 的计算结果进行了向下转型：
>
> ```
> s1 = (short) (s1 + 1);
> ```

> String s = "a"; switch (s) { case "a": System.out.println("aaa"); break; case "b": System.out.println("bbb"); break; }

> switch 不支持 long

> final

> *   对于基本类型，final 使数值不变；
> *   对于引用类型，final 使引用不变，也就不能引用其它对象，但是被引用的对象本身是可以修改的。

> final int x = 1; final A y = new A(); y.a = 1;

> 1\. 数据

> **2\. 方法**
>
> 声明方法不能被子类重写。

> private 方法隐式地被指定为 final，如果在子类中定义的方法和基类中的一个 private 方法签名相同，此时子类的方法不是重写基类方法，而是在子类中定义了一个新的方法。

> **3\. 类**
>
> 声明类不允许被继承。

> 静态变量：又称为类变量

> 可以直接通过类名来访问它。静态变量在内存中只存在一份。

> 实例变量：每创建一个实例就会产生一个实例变量，它与该实例同生共死。

> 静态方法在类加载的时候就存在了，它不依赖于任何实例。所以静态方法必须有实现，也就是说它不能是抽象方法。

> 只能访问所属类的静态字段和静态方法，方法中不能有 this 和 super 关键字，因此这两个关键字与具体对象关联。

> 静态语句块在类初始化时运行一次。

> 静态内部类

> InnerClass innerClass = outerClass.new InnerClass(); StaticInnerClass staticInnerClass = new StaticInnerClass();

> InnerClass innerClass = outerClass.new InnerClass();

> **6\. 初始化顺序**
>
> 静态变量和静态语句块优先于实例变量和普通语句块，静态变量和静态语句块的初始化顺序取决于它们在代码中的顺序。
>
> ```
> public static String staticField = "静态变量";
> ```
>
> ```
> static {
>  System.out.println("静态语句块");
> }
> ```
>
> ```
> public String field = "实例变量";
> ```
>
> ```
> {
>  System.out.println("普通语句块");
> }
> ```
>
> 最后才是构造函数的初始化。
>
> ```
> public InitialOrderTest() {
>  System.out.println("构造函数");
> }
> ```

> 对于基本类型，== 判断两个值是否相等，基本类型没有 equals() 方法。

> 对于引用类型，== 判断两个变量是否引用同一个对象，而 equals() 判断引用的对象是否等价。

> **3\. 实现**
>
> *   检查是否为同一个对象的引用，如果是直接返回 true；
> *   检查是否是同一个类型，如果不是，直接返回 false；
> *   将 Object 对象进行转型；
> *   判断每个关键域是否相等。

> @Override public boolean equals(Object o) { if (this == o) return true; if (o == null || getClass() != o.getClass()) return false; EqualExample that = (EqualExample) o; if (x != that.x) return false; if (y != that.y) return false; return z == that.z; }

> hashCode() 返回哈希值，而 equals() 是用来判断两个对象是否等价。等价的两个对象散列值一定相同，但是散列值相同的两个对象不一定等价，这是因为计算哈希值具有随机性，两个值不同的对象可能计算出相同的哈希值。

> 在覆盖 equals() 方法时应当总是覆盖 hashCode() 方法，保证等价的两个对象哈希值也相等。

> HashSet 和 HashMap 等集合类使用了 hashCode() 方法来计算对象应该存储的位置，因此要将对象添加到这些集合类中，需要让对应的类实现 hashCode() 方法。

> 理想的哈希函数应当具有均匀性，即不相等的对象应当均匀分布到所有可能的哈希值上。这就要求了哈希函数要把所有域的值都考虑进来。可以将每个域都当成 R 进制的某一位，然后组成一个 R 进制的整数。
>
> R 一般取 31，因为它是一个奇素数，如果是偶数的话，当出现乘法溢出，信息就会丢失，因为与 2 相乘相当于向左移一位，最左边的位丢失。并且一个数与 31 相乘可以转换成移位和减法：`31*x == (x<<5)-x`，编译器会自动进行这个优化。

> ```
> @Override
> public int hashCode() {
>  int result = 17;
>  result = 31 * result + x;
>  result = 31 * result + y;
>  result = 31 * result + z;
>  return result;
> }
> ```

> Cloneable 接口只是规定，如果一个类没有实现 Cloneable 接口又调用了 clone() 方法，就会抛出 CloneNotSupportedException。

> ```
> public class CloneExample implements Cloneable {
>  private int a;
>  private int b;
> 
>  @Override
>  public Object clone() throws CloneNotSupportedException {
>      return super.clone();
>  }
> }
> ```

> clone() 是 Object 的 protected 方法，它不是 public，一个类不显式去重写 clone()，其它类就不能直接去调用该类实例的 clone() 方法。

> 浅拷贝

> return (ShallowCloneExample) super.clone();

> 深拷贝

> DeepCloneExample result = (DeepCloneExample) super.clone(); result.arr = new int\[arr.length\]; for (int i = 0; i < arr.length; i++) { result.arr\[i\] = arr\[i\]; } return result;

> **4\. clone() 的替代方案**
>
> 使用 clone() 方法来拷贝一个对象即复杂又有风险，它会抛出异常，并且还需要类型转换。Effective Java 书上讲到，最好不要去使用 clone()，可以使用拷贝构造函数或者拷贝工厂来拷贝一个对象。

> ```
> public class CloneConstructorExample {
> 
>  private int[] arr;
> 
>  public CloneConstructorExample() {
>      arr = new int[10];
>      for (int i = 0; i < arr.length; i++) {
>          arr[i] = i;
>      }
>  }
> 
>  public CloneConstructorExample(CloneConstructorExample original) {
>      arr = new int[original.arr.length];
>      for (int i = 0; i < original.arr.length; i++) {
>          arr[i] = original.arr[i];
>      }
>  }
> 
>  public void set(int index, int value) {
>      arr[index] = value;
>  }
> 
>  public int get(int index) {
>      return arr[index];
>  }
> }
> ```
>
> 构造函数的参数还可以是自身，学到了

> protected 用于修饰成员，表示在继承体系中成员对于子类可见，但是这个访问修饰符对于类没有意义。

> Java 中有三个访问权限修饰符：private、protected 以及 public，如果不加访问修饰符，表示包级可见。

> 如果子类的方法重写了父类的方法，那么子类中该方法的访问级别不允许低于父类的访问级别。这是为了确保可以使用父类实例的地方都可以使用子类实例去代替

> 字段决不能是公有的，因为这么做的话就失去了对这个字段修改行为的控制，客户端可以对其随意修改。例如下面的例子中，AccessExample 拥有 id 公有字段，如果在某个时刻，我们想要使用 int 存储 id 字段，那么就需要修改所有的客户端代码。

> ```
> public class AccessExample {
> 
>  private int id;
> 
>  public String getId() {
>      return id + "";
>  }
> 
>  public void setId(String id) {
>      this.id = Integer.valueOf(id);
>  }
> }
> ```

> **1\. 抽象类**
>
> 抽象类和抽象方法都使用 abstract 关键字进行声明。如果一个类中包含抽象方法，那么这个类必须声明为抽象类。
>
> 抽象类和普通类最大的区别是，抽象类不能被实例化，只能被继承。

> 接口

> 从 Java 8 开始，接口也可以拥有默认的方法实现，这是因为不支持默认方法的接口的维护成本太高了

> 接口的成员（字段 + 方法）默认都是 public 的，并且不允许定义为 private 或者 protected。

> 接口的字段默认都是 static 和 final 的。

> 在很多情况下，接口优先于抽象类。因为接口没有抽象类严格的类层次结构要求，可以灵活地为一个类添加行为

> **重写（Override）**
>
> 存在于继承体系中，指子类实现了一个与父类在方法声明上完全相同的一个方法

> 重写有以下三个限制：
>
> *   子类方法的访问权限必须大于等于父类方法；
> *   子类方法的返回类型必须是父类方法返回类型或为其子类型。
> *   子类方法抛出的异常类型必须是父类抛出异常类型或为其子类型。

> ```
> class SuperClass {
>  protected List<Integer> func() throws Throwable {
>      return new ArrayList<>();
>  }
> }
> 
> class SubClass extends SuperClass {
>  @Override
>  public ArrayList<Integer> func() throws Exception {
>      return new ArrayList<>();
>  }
> }
> ```

> 下面的示例中，SubClass 为 SuperClass 的子类，SubClass 重写了 SuperClass 的 func() 方法。其中：
>
> *   子类方法访问权限为 public，大于父类的 protected。
> *   子类的返回类型为 ArrayList，是父类返回类型 List 的子类。
> *   子类抛出的异常类型为 Exception，是父类抛出异常 Throwable 的子类。
> *   子类重写方法使用 @Override 注解，从而让编译器自动检查是否满足限制条件。

> 在调用一个方法时，先从本类中查找看是否有对应的方法，如果没有再到父类中查看，看是否从父类继承来。否则就要对参数进行转型，转成父类之后看是否有对应的方法。总的来说，方法调用的优先级为：
>
> *   this.func(this)
> *   super.func(this)
> *   this.func(super)
> *   super.func(super)

> ```
> class A {
> 
>  public void show(A obj) {
>      System.out.println("A.show(A)");
>  }
> 
>  public void show(C obj) {
>      System.out.println("A.show(C)");
>  }
> }
> 
> class B extends A {
> 
>  @Override
>  public void show(A obj) {
>      System.out.println("B.show(A)");
>  }
> }
> 
> class C extends B {
> }
> 
> class D extends C {
> }
> ```

> ```
> public static void main(String[] args) {
> 
>  A a = new A();
>  B b = new B();
>  C c = new C();
>  D d = new D();
> 
>  
>  a.show(a); 
>  
>  a.show(b); 
>  
>  b.show(c); 
>  
>  b.show(d); 
> 
>  
>  A ba = new B();
>  ba.show(c); 
>  ba.show(d); 
> }
> ```

> 每个类都有一个 **Class** 对象，包含了与类有关的信息。当编译一个新类时，会产生一个同名的 .class 文件，该文件内容保存着 Class 对象。

> 反射可以提供运行时的类信息，并且这个类可以在运行时才加载进来，甚至在编译时期该类的 .class 不存在也可以加载进来。

> 也可以使用 `Class.forName("com.mysql.jdbc.Driver")` 这种方式来控制类的加载，该方法会返回一个 Class 对象。

> JRE：Java Runtime Environment

> JDK：Java Development Kit

> JDK 是 Java 开发的核心，集成了 JRE 以及一些其它的工具，比如编译 Java 源码的编译器 javac 等

[10 道 Java 泛型面试题 - 云+社区 - 腾讯云](https://cloud.tencent.com/developer/article/1033693)

TODO：写一段泛型程序来实现LRU缓存?

LinkedHashMap可以用来实现固定大小的LRU缓存，当LRU缓存已经满了的时候，它会把最老的键值对移出缓存。LinkedHashMap提供了一个称为removeEldestEntry()的方法，该方法会被put()和putAll()调用来删除最老的键值对。当然，如果你已经编写了一个可运行的JUnit测试，你也可以随意编写你自己的实现代码。

[注解Annotation实现原理与自定义注解例子 - 贾树丙 - 博客园](https://www.cnblogs.com/acm-bingzi/p/javaAnnotation.html)



\> 本文由 \[简悦 SimpRead\](http://ksria.com/simpread/) 转码， 原文地址 \[www.cnblogs.com\](https://www.cnblogs.com/acm-bingzi/p/javaAnnotation.html)

> 注解的用处：

> 1、生成文档

> 2、跟踪代码依赖性，实现替代配置文件功能。

> ### 元注解：
>
> java.lang.annotation 提供了四种元注解，专门注解其他的注解（在自定义注解的时候，需要使用到元注解）：  
>    @Documented – 注解是否将包含在 JavaDoc 中  
>    @Retention – 什么时候使用该注解  
>    @Target – 注解用于什么地方  
>    @Inherited – 是否允许子类继承该注解

> 自定义注解类编写的一些规则:

> 1\. Annotation 型定义为 @interface

> 2\. 参数成员只能用 public 或默认 (default) 这两个访问权修饰

> 3\. 参数成员只能用基本类型 byte、short、char、int、long、float、double、boolean 八种基本数据类型和 String、Enum、Class、annotations 等数据类型，以及这一些类型的数组.

> 4\. 要获取类方法和字段的注解信息，必须通过 Java 的反射技术来获取 Annotation 对象，因为你除此之外没有别的获取注解对象的方法

> ```Java
> 1 import java.lang.annotation.Documented;
> 2 import java.lang.annotation.Retention;
> 3 import java.lang.annotation.Target;
> 4 import static java.lang.annotation.ElementType.FIELD;
> 5 import static java.lang.annotation.RetentionPolicy.RUNTIME;
> 8 /**
> 9  * 水果供应者注解
> 10  */
> 11 @Target(FIELD)
> 12 @Retention(RUNTIME)
> 13 @Documented
> 14 public @interface FruitProvider {
> 15     /**
> 16      * 供应商编号
> 17      */
> 18     public int id() default -1;
> 20     /**
> 21      * 供应商名称
> 22      */
> 23     public String name() default "";
> 25     /**
> 26      * 供应商地址
> 27      */
> 28     public String address() default "";
> 29 }
> ```

> ```Java
> 1 import java.lang.reflect.Field;
> 3 /**
> 4  * 注解处理器
> 5  */
> 6 public class FruitInfoUtil {
> 7     public static void getFruitInfo(Class<?> clazz){
> 9         String strFruitName=" 水果名称：";
> 10         String strFruitColor=" 水果颜色：";
> 11         String strFruitProvicer="供应商信息：";
> 13         Field[] fields = clazz.getDeclaredFields();
> 15         for(Field field :fields){
> 16             if(field.isAnnotationPresent(FruitName.class)){
> 17                 FruitName fruitName = (FruitName) field.getAnnotation(FruitName.class);
> 18                 strFruitName=strFruitName+fruitName.value();
> 19                 System.out.println(strFruitName);
> 20             }
> 21             else if(field.isAnnotationPresent(FruitColor.class)){
> 22                 FruitColor fruitColor= (FruitColor) field.getAnnotation(FruitColor.class);
> 23                 strFruitColor=strFruitColor+fruitColor.fruitColor().toString();
> 24                 System.out.println(strFruitColor);
> 25             }
> 26             else if(field.isAnnotationPresent(FruitProvider.class)){
> 27                 FruitProvider fruitProvider= (FruitProvider) field.getAnnotation(FruitProvider.class);
> 28                 strFruitProvicer=" 供应商编号："+fruitProvider.id()+" 供应商名称："+fruitProvider.name()+" 供应商地址："+fruitProvider.address();
> 29                 System.out.println(strFruitProvicer);
> 30             }
> 31         }
> 32     }
> 33 }
> ```

> ```Java
> 1 import test.FruitColor.Color;
> 3 /**
> 4  * 注解使用
> 5  */
> 6 public class Apple {
> 8     @FruitName("Apple")
> 9     private String appleName;
> 11     @FruitColor(fruitColor=Color.RED)
> 12     private String appleColor;
> 14     @FruitProvider(id=1,)
> 15     private String appleProvider;
> 17     public void setAppleColor(String appleColor) {
> 18         this.appleColor = appleColor;
> 19     }
> 20     public String getAppleColor() {
> 21         return appleColor;
> 22     }
> 24     public void setAppleName(String appleName) {
> 25         this.appleName = appleName;
> 26     }
> 27     public String getAppleName() {
> 28         return appleName;
> 29     }
> 31     public void setAppleProvider(String appleProvider) {
> 32         this.appleProvider = appleProvider;
> 33     }
> 34     public String getAppleProvider() {
> 35         return appleProvider;
> 36     }
> 38     public void displayName(){
> 39         System.out.println("水果的名字是：苹果");
> 40     }
> 41 }
> ```

> ```Java
> 1 /**
> 2  * 输出结果
> 3  */
> 4 public class FruitRun {
> 5     public static void main(String[] args) {
> 6         FruitInfoUtil.getFruitInfo(Apple.class);
> 7     }
> 8 }
> ```



