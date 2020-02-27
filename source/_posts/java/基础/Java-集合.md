---
title: Java 集合
date: 2020-01-16 15:57:38
categories:
- java
- 基础
toc: true
tags:
- Java
---

[toc]

<!--more-->
# [集合大家族 - Java 提高篇 - 极客学院Wiki](https://wiki.jikexueyuan.com/project/java-enhancement/java-twenty.html)

<details>
  <summary>点击查看图片</summary>
![1Ysxl73BwkfSpvM](https://i.loli.net/2020/01/16/1Ysxl73BwkfSpvM.jpg)
</details>

## 一、Collection 接口

Collection 接口是最基本的集合接口

## 二、List 接口

List 接口为 Collection 直接接口。List 所代表的是有序的 Collection
实现 List 接口的集合主要有：ArrayList、LinkedList、Vector、Stack。

### 2.1、ArrayList

ArrayList 是一个动态数组，也是我们最常用的集合。它允许任何符合规则的元素插入甚至包括 null。每一个 ArrayList 都有一个初始容量（10）。随着容器中的元素不断增加，容器的大小也会随着增加。在每次向容器中增加元素的同时都会进行容量检查，当快溢出时，就会进行扩容操作。所以**如果我们明确所插入元素的多少，最好指定一个初始容量值**，避免过多的进行扩容操作而浪费时间、效率。
O(1)： size、isEmpty、get、set、iterator 和 listIterator
O(n)： add
ArrayList 擅长于随机访问。同时 ArrayList 是非同步的。

#### 构造函数

ArrayList 提供了三个构造函数：
ArrayList()：默认构造函数，提供初始容量为 10 的空列表。
ArrayList(int initialCapacity)：构造一个具有指定初始容量的空列表。
ArrayList(Collection<? extends E> c)：构造一个包含指定 collection 的元素的列表

#### 新增

add(E e)：将指定的元素添加到此列表的尾部
add(int index, E element)：将指定的元素插入此列表中的指定位置。
addAll(Collection<? extends E> c)
addAll(int index, Collection<? extends E> c)
set(int index, E element)

#### 删除

remove(int index)、remove(Object o)、removeRange(int fromIndex, int toIndex)、removeAll()

#### 查找

get(int index)

#### 扩容

**ensureCapacity()**，该方法就是 ArrayList 的扩容方法
当我们清楚知道业务数据量或者需要插入大量元素前，我可以使用 ensureCapacity 来手动增加 ArrayList 实例的容量，以减少递增式再分配的数量

### 2.2、LinkedList

LinkedList 是一个双向链表
可以通过较低的代价在 List 中进行插入和删除操作。
与 ArrayList 一样，LinkedList 也是非同步的。
如果多个线程同时访问一个 List，则必须自己实现访问同步。一种解决方法是在创建 List 时构造一个同步的 List：

```Java
List list = Collections.synchronizedList(new LinkedList(…));
```

#### 构造方法

LinkedList 提供了两个构造方法：LinkedList() 和 LinkedList(Collection<? extends E> c)

#### 增加方法

add(E e)
add(int index, E element)
addAll(Collection<? extends E> c)
addAll(int index, Collection<? extends E> c)
**addFirst(E e)
addLast(E e)**

#### 移除方法

clear()： 从此列表中移除所有元素。
remove()/removeFirst()：获取并移除此列表的第一个元素
remove(int index)：移除此列表中指定位置处的元素。
remove(Objec o)/removeFirstOccurrence(Object o)：从此列表中移除首次出现的指定元素（如果存在）。
removeLast()：移除并返回此列表的最后一个元素。
removeLastOccurrence(Object o)：从此列表中移除最后一次出现的指定元素（从头部到尾部遍历列表时）。

#### 查找方法

get(int index)
getFirst()
getLast()
indexOf(Object o)
lastIndexOf(Object o)

### 2.3、Vector

与 ArrayList 相似，但是 Vector 是同步的。所以说 **Vector 是线程安全的动态数组**。它的操作与 ArrayList 几乎一样。

### 2.4、Stack

Stack 继承自 Vector
push pop peek empty/isEmpty search

## 三、Set 接口

与 List 一样，它同样允许 null 的存在，同时要注意任何可变对象，如果在对集合中元素进行操作时，导致 e1.equals(e2)==true，则必定会产生某些问题（？）。实现了 Set 接口的集合有：EnumSet、HashSet、TreeSet。

### 3.1、EnumSet

是枚举的专用 Set。所有的元素都是枚举类型。

### 3.2、HashSet

HashSet 堪称**查询速度最快的集合**，因为其内部是以 HashCode 来实现的。它内部元素的顺序是由哈希码来决定的，所以它不保证 set 的迭代顺序；特别是它不保证该顺序恒久不变。
**HashSet 底层使用了 HashMap 实现**

### 3.3、TreeSet

基于 TreeMap，生成一个总是**处于排序状态的 set**，内部以 TreeMap 来实现。它是使用元素的自然顺序对元素进行排序，或者根据创建 Set 时提供的 Comparator 进行排序，具体取决于使用的构造方法。

## 四、Map 接口

实现 map 的有：HashMap、TreeMap、HashTable、Properties、EnumMap。

### 4.1、HashMap

以哈希表数据结构实现，查找对象时通过哈希函数计算其位置，它是为快速查询而设计的，其内部定义了一个 hash 表数组（Entry[] table），元素会通过哈希转换函数将元素的哈希地址转换成数组中存放的索引，如果有冲突，则使用散列链表的形式将所有相同哈希地址的元素串起来，可能通过查看 HashMap.Entry 的源码它是一个单链表结构。

#### 二、构造函数

HashMap 提供了三个构造函数：
HashMap()：构造一个具有默认初始容量 (16) 和默认加载因子 (0.75) 的空 HashMap。
HashMap(int initialCapacity)
HashMap(int initialCapacity, float loadFactor)

#### 三、数据结构

table 数组 + 链表
![xPe6ROlngwJqdL8](https://i.loli.net/2020/01/16/xPe6ROlngwJqdL8.jpg)
HashMap 底层实现还是数组，只是数组的每一项都是一条链。其中参数 initialCapacity 就代表了该数组的长度

### 4.2、TreeMap

键以某种排序规则排序，内部以 red-black（红-黑）树数据结构实现，实现了 SortedMap 接口

### 4.3、HashTable

也是以哈希表数据结构实现的，解决冲突时与 HashMap 也一样也是采用了散列链表的形式，不过性能比 HashMap 要低

## 五、Queue

队列，它主要分为两大类，一类是阻塞式队列，队列满了以后再插入元素则会抛出异常，主要包括 ArrayBlockQueue、PriorityBlockingQueue、LinkedBlockingQueue。
另一种队列则是双端队列，支持在头、尾两端插入和移除元素，主要包括：ArrayDeque、LinkedBlockingDeque、LinkedList。

## 六、异同点

出处：[http://blog.csdn.net/softwave/article/details/4166598](http://blog.csdn.net/softwave/article/details/4166598)

### 6.1、Vector 和 ArrayList

1，vector 是线程同步的，所以它也是线程安全的，而 arraylist 是线程异步的，是不安全的。如果不考虑到线程的安全因素，一般用 arraylist 效率比较高。
2，如果集合中的元素的数目大于目前集合数组的长度时，vector 增长率为目前数组长度的 100%,而 arraylist 增长率为目前数组长度的 50%.如过在集合中使用数据量比较大的数据，用 vector 有一定的优势。
3，如果查找一个指定位置的数据，vector 和 arraylist 使用的时间是相同的，都是 0(1),这个时候使用 vector 和 arraylist 都可以。而如果移动一个指定位置的数据花费的时间为 0(n-i)n 为总长度，这个时候就应该考虑到使用 linklist,因为它移动一个指定位置的数据所花费的时间为 0(1),而查询一个指定位置的数据时花费的时间为 0(i)。

### 6.3、HashMap 与 TreeMap

1、HashMap 通过 hashcode 对其内容进行快速查找，而 TreeMap 中所有的元素都保持着某种固定的顺序，如果你需要得到一个有序的结果你就应该使用 TreeMap（HashMap 中元素的排列顺序是不固定的）
2、在 Map 中插入、删除和定位元素，HashMap 是最好的选择。但如果您要按自然顺序或自定义顺序遍历键，那么 TreeMap 会更好。使用 HashMap 要求添加的键类明确定义了 hashCode() 和 equals() 的实现。 这个 TreeMap 没有调优选项，因为该树总处于平衡状态。

### 6.4、hashTable 与 hashMap

1、历史原因：Hashtable 是基于陈旧的 Dictionary 类的，HashMap 是 Java 1.2 引进的 Map 接口的一个实现 。
2、同步性：Hashtable 是线程安全的，也就是说是同步的，而 HashMap 是线程序不安全的，不是同步的 。
但是，**通过 Collections 类的 synchronizedMap 方法是可以让 HashMap 线程同步**
3、 hashMap 允许 key 为 null，hashTable 不行
当 HashMap 遇到为 null 的 key 时，它会调用 putForNullKey 方法来进行处理。对于 value 没有进行任何处理，只要是对象都可以。

```java
if (key == null)
    return putForNullKey(value);
```

而当 HashTable 遇到 null 时，他会直接抛出 NullPointerException 异常信息。

```java
if (value == null) {
    throw new NullPointerException();
}
```

## 七、对集合的选择

### 7.1、对 List 的选择

1、对于随机查询与迭代遍历操作，数组比所有的容器都要快。所以在随机访问中一般使用 ArrayList
2、LinkedList 使用双向链表对元素的增加和删除提供了非常好的支持，而 ArrayList 执行增加和删除元素需要进行元素位移。
3、对于 Vector 而已，我们一般都是避免使用。
4、将 ArrayList 当做首选，毕竟对于集合元素而已我们都是进行遍历，只有当程序的性能因为 List 的频繁插入和删除而降低时，再考虑 LinkedList。

### 7.2、对 Set 的选择

1、HashSet 由于使用 HashCode 实现，所以在某种程度上来说它的性能永远比 TreeSet 要好，尤其是进行增加和查找操作。
2、虽然 TreeSet 没有 HashSet 性能好，但是由于它可以维持元素的排序，所以它还是存在用武之地的。

### 7.3、对 Map 的选择

1、HashMap 与 HashSet 同样，支持快速查询。虽然 HashTable 速度的速度也不慢，但是在 HashMap 面前还是稍微慢了些，所以 HashMap 在查询方面可以取代 HashTable。
2、由于 TreeMap 需要维持内部元素的顺序，所以它通常要比 HashMap 和 HashTable 慢。
