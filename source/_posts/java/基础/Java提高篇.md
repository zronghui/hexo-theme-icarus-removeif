---
title: Java提高篇
date: 2020-01-15 09:26:10
categories:
- java
- 基础
toc: true
tags:
- Java
---

[toc]

<!--toc-->

<!--more-->
# [理解 Java 的三大特性之封装](https://wiki.jikexueyuan.com/project/java-enhancement/java-one.html)

用户是无需知道对象内部的细节（当然也无从知道），但可以通过该对象对外的提供的接口来访问该对象（如 setter getter）
封装可以使容易地修改类的内部实现，而无需修改使用了该类的客户代码
封装里面也可以写一些逻辑判断

# [理解 Java 的三大特性之继承](https://wiki.jikexueyuan.com/project/java-enhancement/java-two.html)

继承是使用已存在的类的定义作为基础建立新类的技术，新类的定义可以增加新的数据或新的功能，也可以用父类的功能，但不能选择性地继承父类。通过使用继承我们能够非常方便地复用以前的代码，能够大大的提高开发的效率。
继承所描述的是“is-a”的关系
**子类拥有父类非 private 的属性和方法。**

## 构造器

于构造器而言，它只能够被调用，而不能被继承。 调用父类的构造方法我们使用　super()　即可。
子类默认调用父类的默认构造器，如果父类没有默认的构造器，子类的构造方法的第一句必须调用父类的构造器

## 谨慎继承

继承存在如下缺陷：
1、父类变，子类就必须变。
2、继承破坏了封装，对于父类而言，它的实现细节对与子类来说都是透明的。
3、继承是一种强耦合关系。
《Think in Java》中提供了解决办法：问一问自己是否需要从子类向父类进行向上转型。如果必须向上转型，则继承是必要的，但是如果不需要，则应当好好考虑自己是否需要继承。

# [理解 Java 的三大特性之多态](https://wiki.jikexueyuan.com/project/java-enhancement/java-three.html)

子类向上转型为父类，调用子类重写的方法。

# [Java 的四舍五入](https://wiki.jikexueyuan.com/project/java-enhancement/java-four.html)

12.5 的四舍五入值：13
-12.5 的四舍五入值：-12

```Java
// 方法一
double f = 111231.5585;
BigDecimal b = new BigDecimal(f);
double f1 = b.setScale(2, RoundingMode.HALF_UP).doubleValue();
// 方法二
java.text.DecimalFormat df =new java.text.DecimalFormat(”#.00″);
df.format(3.1415926);
// 方法三
double d = 3.1415926;
String result = String.format(”%.2f”);
```

# [抽象类与接口](https://wiki.jikexueyuan.com/project/java-enhancement/java-five.html)

## 抽象类

1、抽象类不能被实例化
2、子类必须重写抽象方法
3、只要包含一个抽象方法的抽象类，该方法必须要定义成抽象类
**4、抽象类中可以包含具体的方法，也可以不包含抽象方法。**
5、子类中的抽象方法不能与父类的抽象方法同名。
6、abstract 不能与 final 并列修饰同一个类。
7、abstract 不能与 private、static、final 或 native 并列修饰同一个方法

## 接口

1、方法默认是 public
2、成员变量默认是 public static final

## 二者的差别

主要体现在：
抽象层次不同。抽象类是对整个类的抽象，接口仅仅对行为抽象

# [使用序列化实现对象的拷贝](https://wiki.jikexueyuan.com/project/java-enhancement/java-six.html)

```Java
public class CloneUtils {
        @SuppressWarnings("unchecked")
        public static <T extends Serializable> T clone(T   obj){
            T cloneObj = null;
            try {
                //写入字节流
                ByteArrayOutputStream out = new ByteArrayOutputStream();
                ObjectOutputStream obs = new   ObjectOutputStream(out);
                obs.writeObject(obj);
                obs.close();

                //分配内存，写入原始对象，生成新对象
                ByteArrayInputStream ios = new  ByteArrayInputStream(out.toByteArray());
                ObjectInputStream ois = new ObjectInputStream(ios);
                //返回生成的新对象
                cloneObj = (T) ois.readObject();
                ois.close();
            } catch (Exception e) {
                e.printStackTrace();
            }
            return cloneObj;
        }
    }
```

使用该工具类的对象必须要实现 Serializable 接口
**实现 Serializable 接口的必须显示地声明 serialVersionUID 字段**

```Java
public class Person implements Serializable{
    private static final long serialVersionUID = 2631590509760908280L;

    ..................
    //去除clone()方法

}

public class Email implements Serializable{
    private static final long serialVersionUID = 1267293988171991494L;

    ....................
}
```

使用

```Java
Person person1 =  new Person("张三",email);
Person person2 =  CloneUtils.clone(person1);
person2.setName("李四");
Person person3 =  CloneUtils.clone(person1);
person3.setName("王五");
person1.getEmail().setContent("请与今天12:00到二会议室参加会议...");
```

# [关键字 static](https://wiki.jikexueyuan.com/project/java-enhancement/java-seven.html)

* 静态变量是随着类加载时被完成初始化的，它在内存中仅有一个，且 JVM 也只会为它分配一次内存，同时类所有的实例都共享静态变量，可以直接通过类名来访问它。
* static 修饰的方法我们称之为静态方法，我们通过类名对其进行直接调用。由于他在类加载的时候就存在了，它不依赖于任何实例，所以 static 方法必须实现，也就是说他不能是抽象方法 abstract。
* 被 static 修饰的代码块，我们称之为静态代码块，静态代码块会随着类的加载一块执行，而且他可以随意放，可以存在于该了的任何地方。

## 局限

1、它只能调用 static 变量。
2、它只能调用 static 方法。
3、不能以任何形式引用 this、super。
4、static 变量在定义时必须要进行初始化，且初始化时间要早于非静态变量。

# [详解内部类](https://wiki.jikexueyuan.com/project/java-enhancement/java-eight.html)

## 特点

使用内部类最大的优点就在于它能够非常好的解决多重继承的问题
内部类可以用多个实例，每个实例都有自己的状态信息，并且与其他外围对象的信息相互独立。
在单个外围类中，可以让多个内部类以不同的方式实现同一个接口，或者继承同一个类。
创建内部类对象的时刻并不依赖于外围类对象的创建。
内部类提供了更好的封装，除了该外围类，其他类都不能访问。

## 成员内部类

在成员内部类中要注意两点
第一：成员内部类中不能存在任何 static 的变量和方法；
第二：成员内部类是依附于外围类的，所以只有先创建了外围类才能够创建内部类。

<details>
  <summary>点击查看代码</summary>
```Java
public class OuterClass {
    public void display(){
        System.out.println("OuterClass...");
    }

```
public class InnerClass{
    public OuterClass getOuterClass(){
        return OuterClass.this;
    }
}

public static void main(String[] args) {
    OuterClass outerClass = new OuterClass();
    OuterClass.InnerClass innerClass = outerClass.new InnerClass();
    innerClass.getOuterClass().display();
}
```

## }

Output:
OuterClass...

```

</details>

OuterClassName.this，这样就能够产生一个正确引用外部类的引用了
## 局部内部类
有这样一种内部类，它是嵌套在方法和作用域内的
定义在方法里：
<details>
  <summary>点击查看代码</summary>
```Java
public class Parcel5 {
    public Destionation destionation(String str){
        class PDestionation implements Destionation{
            private String label;
            private PDestionation(String whereTo){
                label = whereTo;
            }
            public String readLabel(){
                return label;
            }
        }
        return new PDestionation(str);
    }

    public static void main(String[] args) {
        Parcel5 parcel5 = new Parcel5();
        Destionation d = parcel5.destionation("chenssy");
    }
}
```

</details>

定义在作用域内：

<details>
  <summary>点击查看代码</summary>
```java
public class Parcel6 {
    private void internalTracking(boolean b){
        if(b){
            class TrackingSlip{
                private String id;
                TrackingSlip(String s) {
                    id = s;
                }
                String getSlip(){
                    return id;
                }
            }
            TrackingSlip ts = new TrackingSlip("chenssy");
            String string = ts.getSlip();
        }
    }

```
public void track(){
    internalTracking(true);
}

public static void main(String[] args) {
    Parcel6 parcel6 = new Parcel6();
    parcel6.track();
}
```

}

```

</details>

## 匿名内部类
<details>
  <summary>点击查看代码</summary>
```java
public class OuterClass {
    public InnerClass getInnerClass(final int   num,String str2){
        return new InnerClass(){
            int number = num + 3;
            public int getNumber(){
                return number;
            }
        };        /* 注意：分号不能省 */
    }

    public static void main(String[] args) {
        OuterClass out = new OuterClass();
        InnerClass inner = out.getInnerClass(2, "chenssy");
        System.out.println(inner.getNumber());
    }
}

interface InnerClass {
    int getNumber();
}
----------------
Output:
5
```

</details>

new 匿名内部类，这个类首先是要存在的。如果我们将那个 InnerClass 接口注释掉，就会出现编译出错。
注意 getInnerClass() 方法的形参，第一个形参是用 final 修饰的，而第二个却没有。同时我们也发现第二个形参在匿名内部类中没有使用过，所以当所在方法的形参需要被匿名内部类使用，那么这个形参就必须为 final。
匿名内部类是没有构造方法的

## 静态内部类

1、它的创建是不需要依赖于外围类的。
2、它不能使用任何外围类的非 static 成员变量和方法。

<details>
  <summary>点击查看代码</summary>
```java
public class OuterClass {
    private String sex;
    public static String name = "chenssy";

```
/**
 *静态内部类
 */
static class InnerClass1{
    /* 在静态内部类中可以存在静态成员 */
    public static String _name1 = "chenssy_static";

    public void display(){
        /* 
         * 静态内部类只能访问外围类的静态成员变量和方法
         * 不能访问外围类的非静态成员变量和方法
         */
        System.out.println("OutClass name :" + name);
    }
}

/**
 * 非静态内部类
 */
class InnerClass2{
    /* 非静态内部类中不能存在静态成员 */
    public String _name2 = "chenssy_inner";
    /* 非静态内部类中可以调用外围类的任何成员,不管是静态的还是非静态的 */
    public void display(){
        System.out.println("OuterClass name：" + name);
    }
}

/**
 * @desc 外围类方法
 * @author chenssy
 * @data 2013-10-25
 * @return void
 */
public void display(){
    /* 外围类访问静态内部类：内部类. */
    System.out.println(InnerClass1._name1);
    /* 静态内部类 可以直接创建实例不需要依赖于外围类 */
    new InnerClass1().display();

    /* 非静态内部的创建需要依赖于外围类 */
    OuterClass.InnerClass2 inner2 = new OuterClass().new InnerClass2();
    /* 方位非静态内部类的成员需要使用非静态内部类的实例  */
    System.out.println(inner2._name2);
    inner2.display();
}

public static void main(String[] args) {
    OuterClass outer = new OuterClass();
    outer.display();
}
```

## }

Output:
chenssy_static
OutClass name :chenssy
chenssy_inner
OuterClass name：chenssy

```
</details>
