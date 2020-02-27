---
title: Java 集合2
date: 2020-01-16 20:56:13
categories:
- java
- 基础
toc: true
tags:
- Java
---

[toc]

<!--more-->
# [hashCode](https://wiki.jikexueyuan.com/project/java-enhancement/java-twentysix.html)

对于 hashCode，我们应该遵循如下规则：
在一个应用程序执行期间，如果一个对象的 equals 方法做比较所用到的信息没有被修改的话，则对该对象**调用 hashCode** 方法多次，它必须始终如一地**返回同一个整数**。
如果两个对象根据 equals(Object o) 方法是相等的，则调用这两个对象中任一对象的 hashCode 方法必须产生相同的整数结果。
如果两个对象根据 equals(Object o) 方法是不相等的，则调用这两个对象中任一个对象的 hashCode 方法，不要求产生不同的整数结果。但如果能不同，则可能提高散列表的性能。
即：
如果 x.equals(y) 返回“true”，那么 x 和 y 的 hashCode() 必须相等。
如果 x.equals(y) 返回“false”，那么 x 和 y 的 hashCode() 有可能相等，也有可能不等。
![lz3BR5FWCwibOSX](https://i.loli.net/2020/01/16/lz3BR5FWCwibOSX.jpg)

# [TreeMap](https://wiki.jikexueyuan.com/project/java-enhancement/java-twentyseven.html)

## 一、红黑树简介

红黑树是一颗自平衡的排序二叉树。
平衡二叉树必须具备如下特性：它是一棵空树或它的左右两个子树的高度差的绝对值不超过 1，并且左右两个子树都是一棵平衡二叉树。也就是说该二叉树的任何一个等等子节点，其左右子树的高度都相近。

![](https://wiki.jikexueyuan.com/project/java-enhancement/images/27-1.png)

对于一棵有效的红黑树二叉树而言我们必须增加如下规则：

**1、每个节点都只能是红色或者黑色**

**2、根节点是黑色**

**3、每个叶节点（NIL 节点，空节点）是黑色的。**

**4、如果一个结点是红的，则它两个子节点都是黑的。也就是说在一条路径上不能出现相邻的两个红色结点。**

**5、从任一节点到其每个叶子的所有路径都包含相同数目的黑色节点。**

下图为一颗典型的红黑二叉树。

![](https://wiki.jikexueyuan.com/project/java-enhancement/images/27-2.png)

对于红黑二叉树而言它主要包括三大基本操作：左旋、右旋、着色。

![](https://wiki.jikexueyuan.com/project/java-enhancement/images/27-3.gif)

左旋

![](https://wiki.jikexueyuan.com/project/java-enhancement/images/27-4.gif)

右旋

**1、[红黑树系列集锦](http://blog.csdn.net/v_JULY_v/article/category/774945)**

**2、[红黑树数据结构剖析](http://www.cnblogs.com/fanzhidongyzby/p/3187912.html)**

**3、[红黑树](http://blog.csdn.net/eric491179912/article/details/6179908)**

## 二、TreeMap 数据结构

TreeMap 中同时也包含了如下几个重要的属性：

```Java
//比较器，因为TreeMap是有序的，通过comparator接口我们可以对TreeMap的内部排序进行精密的控制
private final Comparator<? super K> comparator;
//TreeMap红-黑节点，为TreeMap的内部类
private transient Entry<K,V> root = null;
//容器大小
private transient int size = 0;
//TreeMap修改次数
private transient int modCount = 0;
//红黑树的节点颜色--红色
private static final boolean RED = false;
//红黑树的节点颜色--黑色
private static final boolean BLACK = true;
```

对于叶子节点 Entry 是 TreeMap 的内部类，它有几个重要的属性：

```Java
//键
K key;
//值
V value;
//左孩子
Entry<K,V> left = null;
//右孩子
Entry<K,V> right = null;
//父亲
Entry<K,V> parent;
//颜色
boolean color = BLACK;
```

三、TreeMap put() delete() 方法
略

# [TreeSet](https://wiki.jikexueyuan.com/project/java-enhancement/java-twentyeight.html)

ceiling：返回此 set 中大于等于给定元素的最小元素；如果不存在这样的元素，则返回 null。
floor：返回此 set 中小于等于给定元素的最大元素；如果不存在这样的元素，则返回 null
higher：返回此 set 中严格大于给定元素的最小元素；如果不存在这样的元素，则返回 null
lower：返回此 set 中严格小于给定元素的最大元素；如果不存在这样的元素，则返回 null

clone：返回 TreeSet 实例的浅表副本。属于浅拷贝。
comparator：返回对此 set 中的元素进行排序的比较器；如果此 set 使用其元素的自然顺序，则返回 null。

iterator：返回在此 set 中的元素上按升序进行迭代的迭代器
descendingIterator：返回在此 set 元素上按降序进行迭代的迭代器
descendingSet：返回此 set 中所包含元素的逆序视图

pollFirst：获取并移除第一个（最低）元素；如果此 set 为空，则返回 null
pollLast：获取并移除最后一个（最高）元素；如果此 set 为空，则返回 null。
first：返回此 set 中当前第一个（最低）元素
last：返回此 set 中当前最后一个（最高）元素

remove size isEmpty

# [Java 集合细节（二）：asList 的缺陷](https://wiki.jikexueyuan.com/project/java-enhancement/java-thirtysix.html)

## 一、避免使用基本数据类型数组转换为列表

```java
public static void main(String[] args) {
    Integer[] ints = {1,2,3,4,5};
    List list = Arrays.asList(ints);
    System.out.println("list'size：" + list.size());
    System.out.println("list.get(0) 的类型:" +  list.get(0).getClass());
    System.out.println("list.get(0) == ints[0]：" + list.get(0).equals(ints[0]));
}
----------------------------------------
outPut:
list'size：5
list.get(0) 的类型:class java.lang.Integer
list.get(0) == ints[0]：true
```

## 二、asList 产生的列表不可操作

asList 返回的列表只不过是一个披着 list 的外衣，它并没有 list 的基本特性（变长）。该 list 是一个长度不可变的列表，传入参数的数组有多长，其返回的列表就只能是多长。

# [Java 集合细节（三）：subList 的缺陷](https://wiki.jikexueyuan.com/project/java-enhancement/java-thirtyseven.html)

## 一、subList 返回仅仅只是一个视图

subList 返回的只是原列表的一个视图，它所有的操作最终都会作用在原列表上

## 二、subList 生成子列表后，不要试图去操作原列表

对于子列表视图，它是动态生成的，生成之后就不要操作原列表了，否则必然都导致视图的不稳定而抛出异常。最好的办法就是将原列表设置为只读状态，要操作就操作子列表：

```java
//通过subList生成一个与list1一样的列表 list3
List<Integer> list3 = list1.subList(0, list1.size());

//对list1设置为只读状态
list1 = Collectio
ns.unmodifiableList(list1);
```

生成子列表后，不要试图去操作原列表，否则会造成子列表的不稳定而产生异常

## 三、推荐使用 subList 处理局部列表

在开发过程中我们一定会遇到这样一个问题：获取一堆数据后，需要删除某段数据。例如，有一个列表存在 1000 条记录，我们需要删除 100-200 位置处的数据

```java
list1.subList(100, 200).clear();
```

# [Java 集合细节（四）：保持 compareTo 和 equals 同步](https://wiki.jikexueyuan.com/project/java-enhancement/java-thirtyeight.html)

**Collections.binarySearch(list, student);**
在 sorted list 中获取指定元素的索引

```java
public static void main(String[] args){
    List<Student> list = new ArrayList<>();
    list.add(new Student("1", "chenssy1", 24));
    list.add(new Student("2", "chenssy1", 26));

    Collections.sort(list);   //排序

    Student student = new Student("2", "chenssy1", 26);

    //检索student在list中的位置
    int index1 = list.indexOf(student);
    int index2 = Collections.binarySearch(list, student);

    System.out.println("index1 = " + index1);
    System.out.println("index2 = " + index2);
}
```

运行结果：

```java
index1 = 0
index2 = 1
```

因为 indexOf 和 binarySearch 的实现机制不同
**indexOf 是基于 equals 来实现的**, 只要 equals 返回 TRUE 就认为已经找到了相同的元素。
**binarySearch 是基于 compareTo 方法**的，当 compareTo 返回 0 时就认为已经找到了该元素。在我们实现的 Student 类中我们覆写了 compareTo 和 equals 方法，但是我们的 compareTo、equals 的比较依据不同，一个是基于 age、一个是基于 name。比较依据不同那么得到的结果很有可能会不同。所以知道了原因，我们就好修改了：将两者之间的比较依据保持一致即可。

对于 compareTo 和 equals 两个方法我们可以总结为：compareTo 是判断元素在排序中的位置是否相等，equals 是判断元素是否相等，既然一个决定排序位置，一个决定相等，所以我们非常有必要确保当排序位置相同时，其 equals 也应该相等。

# [Iterator - Java 提高篇 - 极客学院Wiki](https://wiki.jikexueyuan.com/project/java-enhancement/java-thirty.html)


若不使用iterator, 每一种集合对应一种遍历方法，客户端代码无法复用
对于数组我们是使用下标来进行处理的:
```java
int[] arrays = new int[10];
for(int i = 0 ; i < arrays.length ; i++){
   int a = arrays[i];
   //do something
}
```
对于 ArrayList 是这么处理的:
```java
List<String> list = new ArrayList<String>();
for(int i = 0 ; i < list.size() ;  i++){
  String string = list.get(i);
  //do something
}
```
Iterator 模式腾空出世，它总是用同一种逻辑来遍历集合。使得客户端自身不需要来维护集合的内部结构，所有的内部状态都由 Iterator 来维护。客户端从不直接和集合类打交道，它总是控制 Iterator，向它发送”向前”，”向后”，”取当前元素”的命令，就可以间接遍历整个集合。
```java
Iterator iterator = list.iterator();
while(iterator.hasNext()){
    String string = iterator.next();
    //do something
}
```
# [fail-fast 机制 - Java 提高篇 - 极客学院Wiki](https://wiki.jikexueyuan.com/project/java-enhancement/java-thirtyfour.html)
fail-fast，它是 Java 集合的一种错误检测机制
比如，若一个线程在遍历ArrayList, 另一个线程修改了ArrayList, 此时遍历的线程会触发fail-fast机制，抛出ConcurrentModificationException异常。

ArrayList 中无论 add、remove、clear 方法只要是涉及了改变 ArrayList 元素的个数的方法都会导致 modCount 的改变。ArrayList的迭代器在调用 next()、remove() 方法时都是调用 checkForComodification() 方法，该方法主要就是检测 modCount == expectedModCount 。 若不等则抛出 ConcurrentModificationException 异常，从而产生 fail-fast 机制。

## 两种解决方案：
方案一：在遍历过程中所有涉及到改变 modCount 值得地方全部加上 synchronized 或者直接使用 Collections.synchronizedList，这样就可以解决。但是不推荐，因为增删造成的同步锁可能会阻塞遍历操作。
方案二：使用 CopyOnWriteArrayList 来替换 ArrayList。推荐使用该方案。
CopyOnWriteArrayList 为何物？ArrayList 的一个线程安全的变体，其中所有可变操作（add、set 等等）都是通过对底层数组进行一次新的复制来实现的。 该类产生的开销比较大，但是在两种情况下，它非常适合使用。1：在不能或不想进行同步遍历，但又需要从并发线程中排除冲突时。2：当遍历操作的数量大大超过可变操作的数量时。遇到这两种情况使用 CopyOnWriteArrayList 来替代 ArrayList 再适合不过了。
