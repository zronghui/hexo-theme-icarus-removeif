---
title: 04 guava
date: 2020-02-09 10:04:30
categories:
- java
- module test
toc: true
tags:
---

[toc]

<!--more-->



[Google Guava官方教程（中文版） | 并发编程网 – ifeve.com](http://ifeve.com/google-guava/)
[Guava教程™](https://www.yiibai.com/guava)

## maven

```xml
<dependency>
  <groupId>com.google.guava</groupId>
  <artifactId>guava</artifactId>
  <version>28.2-jre</version>
</dependency>
```



## Optional

Optional 被 Java 自带的替代

```java
package guava;

import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

import java.util.Optional;

public class aOptional {
    private static Logger logger = LoggerFactory.getLogger(aOptional.class);
    public static void add(Optional<Integer> a, Optional<Integer> b) {
        logger.info(String.valueOf(a.isPresent()));
        logger.info(String.valueOf(b.isPresent()));
        logger.info(String.valueOf(a.orElse(0) + b.orElse(0)));
        // 获取值
        // a.get()   a 为 null 报错
        // a.orElse(x)   a 为 null 返回 x
    }

    public static void main(String[] args) {
        // 构造
        // Optional.empty()
        // Optional.of(x)
        // Optional.ofNullable(x)  若为 null，返回代表 null 的 Optional : Optional.empty()
        Optional<Integer> a = Optional.ofNullable(10);
        Optional<Integer> b = Optional.of(10);
        add(a, b);
    }
}
```

## Preconditions

```java
package guava;

import com.google.common.base.Preconditions;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

// Preconditions.checkArgument
// Preconditions.checkNotNull
// Preconditions.checkElementIndex

public class bPreconditions {
    private static Logger logger = LoggerFactory.getLogger(bPreconditions.class);

    public static double sqrt(double input) throws IllegalArgumentException {
        Preconditions.checkArgument(input > 0.0,
                "Illegal Argument passed: Negative value %s.", input);
        return Math.sqrt(input);
    }

    public static int sum(Integer a, Integer b) {
        a = Preconditions.checkNotNull(a,
                "Illegal Argument passed: First parameter is Null.");
        b = Preconditions.checkNotNull(b,
                "Illegal Argument passed: Second parameter is Null.");
        return a + b;
    }

    public static int getValue(int input) {
        int[] data = {1, 2, 3, 4, 5};
        Preconditions.checkElementIndex(input, data.length,
                "Illegal Argument passed: Invalid index.");
        return 0;
    }


    public static void main(String[] args) {
        try {
            System.out.println(sqrt(-3.0));
        } catch (IllegalArgumentException e) {
            System.out.println(e.getMessage());
        }
        try {
            System.out.println(sum(null, 3));
        } catch (NullPointerException e) {
            System.out.println(e.getMessage());
        }
        try {
            System.out.println(getValue(6));
        } catch (IndexOutOfBoundsException e) {
            System.out.println(e.getMessage());
        }
    }
}

```

## Guava Ordering类	

```java
package guava;

import com.google.common.collect.Ordering;

import java.util.ArrayList;
import java.util.Collection;
import java.util.Collections;

public class cOrdering {
    public static void main(String[] args) {
        ArrayList<Integer> integers = new ArrayList<>();
        Collections.addAll(integers, 6, 3, 2, 7, 9);

        // sort
        Ordering<Comparable> ordering = Ordering.natural();
        Collections.sort(integers, ordering);
        // reverse sort
        Collections.sort(integers, ordering.reverse());
        // isOrdered
        System.out.println(ordering.reverse().isOrdered(integers));

        System.out.println(integers);
        System.out.println(Collections.max(integers));
        System.out.println(Collections.min(integers));

        // max min
        System.out.println(ordering.max(integers));
        System.out.println(ordering.min(integers));

        integers.add(null);
        // nullsFirst nullsLast
        Collections.sort(integers, ordering.reverse().nullsFirst());
        System.out.println(integers);
    }
}
```



## Guava Objects类

```java
package guava;

import com.google.common.base.Objects;
import com.google.common.collect.ComparisonChain;
import lombok.Data;
import lombok.Getter;
import lombok.Setter;

public class dObjects {

}

@Setter
@Getter
@Data
class Student implements Comparable<Student> {
    private int id;
    private String name;

    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (o == null || getClass() != o.getClass()) return false;
        Student student = (Student) o;
        return id == student.id &&
                Objects.equal(name, student.name);
    }

    @Override
    public int hashCode() {
        // Objects.hashCode(field-1, field-2, …, field-n)
        return Objects.hashCode(id, name);
    }

    @Override
    public int compareTo(Student o) {
        return ComparisonChain.start()
                .compare(this.name, o.name)
                .compare(this.id, o.id)
                .result();
    }
}

```

## Guava集合工具

### 不可变集合

不可变集合可以用如下多种方式创建：

- copyOf方法，如ImmutableSet.copyOf(set);
- of方法，如ImmutableSet.of(“a”, “b”, “c”)或 ImmutableMap.of(“a”, 1, “b”, 2);
- Builder工具

细节：关联可变集合和不可变集合

| **可变集合接口**                                             | **属于****JDK****还是****Guava** | **不可变版本**                                               |
| ------------------------------------------------------------ | -------------------------------- | ------------------------------------------------------------ |
| Collection                                                   | JDK                              | [`ImmutableCollection`](http://docs.guava-libraries.googlecode.com/git-history/release/javadoc/com/google/common/collect/ImmutableCollection.html) |
| List                                                         | JDK                              | [`ImmutableList`](http://docs.guava-libraries.googlecode.com/git-history/release/javadoc/com/google/common/collect/ImmutableList.html) |
| Set                                                          | JDK                              | [`ImmutableSet`](http://docs.guava-libraries.googlecode.com/git-history/release/javadoc/com/google/common/collect/ImmutableSet.html) |
| SortedSet/NavigableSet                                       | JDK                              | [`ImmutableSortedSet`](http://docs.guava-libraries.googlecode.com/git-history/release/javadoc/com/google/common/collect/ImmutableSortedSet.html) |
| Map                                                          | JDK                              | [`ImmutableMap`](http://docs.guava-libraries.googlecode.com/git-history/release/javadoc/com/google/common/collect/ImmutableMap.html) |
| SortedMap                                                    | JDK                              | [`ImmutableSortedMap`](http://docs.guava-libraries.googlecode.com/git-history/release/javadoc/com/google/common/collect/ImmutableSortedMap.html) |
| [Multiset](http://code.google.com/p/guava-libraries/wiki/NewCollectionTypesExplained#Multiset) | Guava                            | [`ImmutableMultiset`](http://docs.guava-libraries.googlecode.com/git-history/release/javadoc/com/google/common/collect/ImmutableMultiset.html) |
| SortedMultiset                                               | Guava                            | [`ImmutableSortedMultiset`](http://docs.guava-libraries.googlecode.com/git-history/release12/javadoc/com/google/common/collect/ImmutableSortedMultiset.html) |
| [Multimap](http://code.google.com/p/guava-libraries/wiki/NewCollectionTypesExplained#Multimap) | Guava                            | [`ImmutableMultimap`](http://docs.guava-libraries.googlecode.com/git-history/release/javadoc/com/google/common/collect/ImmutableMultimap.html) |
| ListMultimap                                                 | Guava                            | [`ImmutableListMultimap`](http://docs.guava-libraries.googlecode.com/git-history/release/javadoc/com/google/common/collect/ImmutableListMultimap.html) |
| SetMultimap                                                  | Guava                            | [`ImmutableSetMultimap`](http://docs.guava-libraries.googlecode.com/git-history/release/javadoc/com/google/common/collect/ImmutableSetMultimap.html) |
| [BiMap](http://code.google.com/p/guava-libraries/wiki/NewCollectionTypesExplained#BiMap) | Guava                            | [`ImmutableBiMap`](http://docs.guava-libraries.googlecode.com/git-history/release/javadoc/com/google/common/collect/ImmutableBiMap.html) |
| [ClassToInstanceMap](http://code.google.com/p/guava-libraries/wiki/NewCollectionTypesExplained#ClassToInstanceMap) | Guava                            | [`ImmutableClassToInstanceMap`](http://docs.guava-libraries.googlecode.com/git-history/release/javadoc/com/google/common/collect/ImmutableClassToInstanceMap.html) |
| [Table](http://code.google.com/p/guava-libraries/wiki/NewCollectionTypesExplained#Table) | Guava                            | [`ImmutableTable`](http://docs.guava-libraries.googlecode.com/git-history/release/javadoc/com/google/common/collect/ImmutableTable.html) |

### Guava Multiset接口

允许 set 重复，并统计每个值的次数

```java
import java.util.Iterator;
import java.util.Set;

import com.google.common.collect.HashMultiset;
import com.google.common.collect.Multiset;

public class GuavaTester {

   public static void main(String args[]){
      // HashMultiset.create()
      Multiset<String> multiset = HashMultiset.create();
      multiset.add("a");
      multiset.add("b");
      multiset.add("c");
      multiset.add("d");
      multiset.add("a");
      multiset.add("b");
      multiset.add("c");
      multiset.add("b");
      multiset.add("b");
      multiset.add("b");
      // multiset.count("b")  multiset.size()   multiset.elementSet()
      System.out.println("Occurrence of 'b' : "+multiset.count("b"));
      System.out.println("Total Size : "+multiset.size());
      Set<String> set = multiset.elementSet();
      System.out.println("Set [");
      for (String s : set) {			
         System.out.println(s);		    
      }
      System.out.println("]");
      //display all the elements of the multiset using iterator
      Iterator<String> iterator  = multiset.iterator();
      System.out.println("MultiSet [");
      while(iterator.hasNext()){
         System.out.println(iterator.next());
      }
      System.out.println("]");		
      //display the distinct elements of the multiset with their occurrence count
      System.out.println("MultiSet [");
      for (Multiset.Entry<String> entry : multiset.entrySet())
      {
         System.out.println("Element: "+entry.getElement() +", Occurrence(s): " + entry.getCount());		    
      }
      System.out.println("]");		

      //remove extra occurrences 
      multiset.remove("b",2);
      //print the occurrence of an element
      System.out.println("Occurence of 'b' : "+multiset.count("b"));
   }	
}
```



### Guava Bimap接口

可以 inverse，获得 value: key 的 map

```java
import com.google.common.collect.BiMap;
import com.google.common.collect.HashBiMap;

public class GuavaTester {

   public static void main(String args[]){
      BiMap<Integer, String> empIDNameMap = HashBiMap.create();

      empIDNameMap.put(new Integer(101), "Mahesh");
      empIDNameMap.put(new Integer(102), "Sohan");
      empIDNameMap.put(new Integer(103), "Ramesh");

      //Emp Id of Employee "Mahesh"
      System.out.println(empIDNameMap.inverse().get("Mahesh"));
   }	
}
```

#### BiMap的各种实现

| **键**–值实现 | **值–**键实现 | 对应BiMap实现                                                |
| ------------- | ------------- | ------------------------------------------------------------ |
| HashMap       | HashMap       | [HashBiMap](http://docs.guava-libraries.googlecode.com/git/javadoc/com/google/common/collect/HashBiMap.html) |
| ImmutableMap  | ImmutableMap  | [ImmutableBiMap](http://docs.guava-libraries.googlecode.com/git/javadoc/com/google/common/collect/ImmutableBiMap.html) |
| EnumMap       | EnumMap       | [EnumBiMap](http://docs.guava-libraries.googlecode.com/git-history/release/javadoc/com/google/common/collect/EnumBiMap.html) |
| EnumMap       | HashMap       | [EnumHashBiMap](http://docs.guava-libraries.googlecode.com/git-history/release/javadoc/com/google/common/collect/EnumHashBiMap.html) |

### Guava Table接口

类似于 map 的 map，table(r, c, v)   (row, col, value)

(r, c) 确定 value

```java
import java.util.Map;
import java.util.Set;

import com.google.common.collect.HashBasedTable;
import com.google.common.collect.Table;

public class GuavaTester {

   public static void main(String args[]){
      //Table<R,C,V> == Map<R,Map<C,V>>
      /*
      *  Company: IBM, Microsoft, TCS
      *  IBM 		-> {101:Mahesh, 102:Ramesh, 103:Suresh}
      *  Microsoft 	-> {101:Sohan, 102:Mohan, 103:Rohan } 
      *  TCS 		-> {101:Ram, 102: Shyam, 103: Sunil } 
      * 
      * */
      //create a table
      Table<String, String, String> employeeTable = HashBasedTable.create();

      //initialize the table with employee details
      employeeTable.put("IBM", "101","Mahesh");
      employeeTable.put("IBM", "102","Ramesh");
      employeeTable.put("IBM", "103","Suresh");

      employeeTable.put("Microsoft", "111","Sohan");
      employeeTable.put("Microsoft", "112","Mohan");
      employeeTable.put("Microsoft", "113","Rohan");

      employeeTable.put("TCS", "121","Ram");
      employeeTable.put("TCS", "122","Shyam");
      employeeTable.put("TCS", "123","Sunil");

      //get Map corresponding to IBM
      Map<String,String> ibmEmployees =  employeeTable.row("IBM");

      System.out.println("List of IBM Employees");
      for(Map.Entry<String, String> entry : ibmEmployees.entrySet()){
         System.out.println("Emp Id: " + entry.getKey() + ", Name: " + entry.getValue());
      }

      //get all the unique keys of the table
      Set<String> employers = employeeTable.rowKeySet();
      System.out.print("Employers: ");
      for(String employer: employers){
         System.out.print(employer + " ");
      }
      System.out.println();

      //get a Map corresponding to 102
      Map<String,String> EmployerMap =  employeeTable.column("102");
      for(Map.Entry<String, String> entry : EmployerMap.entrySet()){
         System.out.println("Employer: " + entry.getKey() + ", Name: " + entry.getValue());
      }		
   }	
}
```

### Multimap

很少会直接使用Multimap接口，更多时候你会用ListMultimap或SetMultimap接口，它们分别把键映射到List或Set。

#### Multimap的各种实现

Multimap提供了多种形式的实现。在大多数要使用Map<K, Collection<V>>的地方，你都可以使用它们：

| **实现**                                                     | **键行为类似** | **值行为类似** |
| ------------------------------------------------------------ | -------------- | -------------- |
| [ArrayListMultimap](http://docs.guava-libraries.googlecode.com/git/javadoc/com/google/common/collect/ArrayListMultimap.html) | HashMap        | ArrayList      |
| [HashMultimap](http://docs.guava-libraries.googlecode.com/git/javadoc/com/google/common/collect/HashMultimap.html) | HashMap        | HashSet        |
| [LinkedListMultimap](http://docs.guava-libraries.googlecode.com/git/javadoc/com/google/common/collect/LinkedListMultimap.html)* | LinkedHashMap* | LinkedList*    |
| [LinkedHashMultimap](http://docs.guava-libraries.googlecode.com/git/javadoc/com/google/common/collect/LinkedHashMultimap.html)** | LinkedHashMap  | LinkedHashMap  |
| [TreeMultimap](http://docs.guava-libraries.googlecode.com/git/javadoc/com/google/common/collect/TreeMultimap.html) | TreeMap        | TreeSet        |
| [`ImmutableListMultimap`](http://docs.guava-libraries.googlecode.com/git/javadoc/com/google/common/collect/ImmutableListMultimap.html) | ImmutableMap   | ImmutableList  |
| [ImmutableSetMultimap](http://docs.guava-libraries.googlecode.com/git/javadoc/com/google/common/collect/ImmutableSetMultimap.html) | ImmutableMap   | ImmutableSet   |

### Guava 集合工具



```java
// 用工厂方法模式，我们可以方便地在初始化时就指定起始元素。
List theseElements = Lists.newArrayList("alpha", "beta", "gamma");

// 此外，通过为工厂方法命名（Effective Java第一条），我们可以提高集合初始化大小的可读性：
List<Type> exactly100 = Lists.newArrayListWithCapacity(100);
List<Type> approx100 = Lists.newArrayListWithExpectedSize(100);
Set<Type> approx100Set = Sets.newHashSetWithExpectedSize(100);

// Iterables
Iterable<Integer> concatenated = Iterables.concat(
        Ints.asList(1, 2, 3),
        Ints.asList(4, 5, 6)); // concatenated包括元素 1, 2, 3, 4, 5, 6
String lastAdded = Iterables.getLast(myLinkedHashSet);
String theElement = Iterables.getOnlyElement(thisSetIsDefinitelyASingleton);

List countUp = Ints.asList(1, 2, 3, 4, 5);
// 注: 如果List是不可变的，考虑改用ImmutableList.reverse()。
List countDown = Lists.reverse(theList); // {5, 4, 3, 2, 1}
List<List> parts = Lists.partition(countUp, 2);//{{1,2}, {3,4}, {5}}

Set<String> wordsWithPrimeLength = ImmutableSet.of("one", "two", "three", "six", "seven", "eight");
Set<String> primes = ImmutableSet.of("two", "three", "five", "seven");
SetView<String> intersection = Sets.intersection(primes,wordsWithPrimeLength);
// intersection包含"two", "three", "seven"

Map<String, Integer> map = ImmutableMap.of("a", 1, "b", 1, "c", 2);
SetMultimap<String, Integer> multimap = Multimaps.forMap(map);
// multimap：["a" => {1}, "b" => {1}, "c" => {2}]
Multimap<Integer, String> inverse = Multimaps.invertFrom(multimap, HashMultimap<Integer, String>.create());
// inverse：[1 => {"a","b"}, 2 => {"c"}]

```

####  Iterables 常规方法

| [`concat(Iterable)`](http://docs.guava-libraries.googlecode.com/git-history/release/javadoc/com/google/common/collect/Iterables.html#concat(java.lang.Iterable)) | 串联多个iterables的懒视图*                                   | [`concat(Iterable...)`](http://docs.guava-libraries.googlecode.com/git-history/release/javadoc/com/google/common/collect/Iterables.html#concat(java.lang.Iterable...)) |
| :----------------------------------------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| [`frequency(Iterable, Object)`](http://docs.guava-libraries.googlecode.com/git-history/release/javadoc/com/google/common/collect/Iterables.html#frequency(java.lang.Iterable, java.lang.Object)) | 返回对象在iterable中出现的次数                               | 与Collections.frequency (Collection,  Object)比较；``[Multiset](http://code.google.com/p/guava-libraries/wiki/NewCollectionTypesExplained#Multiset) |
| [`partition(Iterable, int)`](http://docs.guava-libraries.googlecode.com/git-history/release/javadoc/com/google/common/collect/Iterables.html#partition(java.lang.Iterable, int)) | 把iterable按指定大小分割，得到的子集都不能进行修改操作       | [`Lists.partition(List, int)`](http://docs.guava-libraries.googlecode.com/git-history/release/javadoc/com/google/common/collect/Lists.html#partition(java.util.List, int))；[`paddedPartition(Iterable, int)`](http://docs.guava-libraries.googlecode.com/git-history/release/javadoc/com/google/common/collect/Iterables.html#paddedPartition(java.lang.Iterable, int)) |
| [`getFirst(Iterable, T default)`](http://docs.guava-libraries.googlecode.com/git-history/release/javadoc/com/google/common/collect/Iterables.html#getFirst(java.lang.Iterable, T)) | 返回iterable的第一个元素，若iterable为空则返回默认值         | 与Iterable.iterator(). next()比较;[`FluentIterable.first()`](http://docs.guava-libraries.googlecode.com/git-history/release12/javadoc/com/google/common/collect/FluentIterable.html#first()) |
| [`getLast(Iterable)`](http://docs.guava-libraries.googlecode.com/git-history/release/javadoc/com/google/common/collect/Iterables.html#getLast(java.lang.Iterable)) | 返回iterable的最后一个元素，若iterable为空则抛出NoSuchElementException | [`getLast(Iterable, T default)`](http://docs.guava-libraries.googlecode.com/git-history/release/javadoc/com/google/common/collect/Iterables.html#getLast(java.lang.Iterable, T))； [`FluentIterable.last()`](http://docs.guava-libraries.googlecode.com/git-history/release12/javadoc/com/google/common/collect/FluentIterable.html#last()) |
| [`elementsEqual(Iterable, Iterable)`](http://docs.guava-libraries.googlecode.com/git-history/release/javadoc/com/google/common/collect/Iterables.html#elementsEqual(java.lang.Iterable, java.lang.Iterable)) | 如果两个iterable中的所有元素相等且顺序一致，返回true         | 与List.equals(Object)比较                                    |
| [`unmodifiableIterable(Iterable)`](http://docs.guava-libraries.googlecode.com/git-history/release/javadoc/com/google/common/collect/Iterables.html#unmodifiableIterable(java.lang.Iterable)) | 返回iterable的不可变视图                                     | 与Collections. unmodifiableCollection(Collection)比较        |
| [`limit(Iterable, int)`](http://docs.guava-libraries.googlecode.com/git-history/release/javadoc/com/google/common/collect/Iterables.html#limit(java.lang.Iterable, int)) | 限制iterable的元素个数限制给定值                             | [`FluentIterable.limit(int)`](http://docs.guava-libraries.googlecode.com/git-history/release12/javadoc/com/google/common/collect/FluentIterable.html#limit(int)) |
| [`getOnlyElement(Iterable)`](http://docs.guava-libraries.googlecode.com/git-history/release/javadoc/com/google/common/collect/Iterables.html#getOnlyElement(java.lang.Iterable)) | 获取iterable中唯一的元素，如果iterable为空或有多个元素，则快速失败 | [`getOnlyElement(Iterable, T default)`](http://docs.guava-libraries.googlecode.com/git-history/release/javadoc/com/google/common/collect/Iterables.html#getOnlyElement(java.lang.Iterable, T)) |

#### 与Collection方法相似的工具方法

通常来说，Collection的实现天然支持操作其他Collection，但却不能**操作Iterable**。

下面的方法中，如果传入的Iterable是一个Collection实例，则实际操作将会委托给相应的Collection接口方法。例如，往Iterables.size方法传入是一个Collection实例，它不会真的遍历iterator获取大小，而是直接调用Collection.size。

| **方法**                                                     | **类似的****Collection****方法** | **等价的****FluentIterable****方法**                         |
| ------------------------------------------------------------ | -------------------------------- | ------------------------------------------------------------ |
| [`addAll(Collection addTo,  Iterable toAdd)`](http://docs.guava-libraries.googlecode.com/git-history/release/javadoc/com/google/common/collect/Iterables.html#addAll(java.util.Collection, java.lang.Iterable)) | Collection.addAll(Collection)    |                                                              |
| [`contains(Iterable, Object)`](http://docs.guava-libraries.googlecode.com/git-history/release/javadoc/com/google/common/collect/Iterables.html#contains(java.lang.Iterable, java.lang.Object)) | Collection.contains(Object)      | [`FluentIterable.contains(Object)`](http://docs.guava-libraries.googlecode.com/git-history/release12/javadoc/com/google/common/collect/FluentIterable.html#contains(java.lang.Object)) |
| [`removeAll(Iterable  removeFrom, Collection toRemove)`](http://docs.guava-libraries.googlecode.com/git-history/release/javadoc/com/google/common/collect/Iterables.html#removeAll(java.lang.Iterable, java.util.Collection)) | Collection.removeAll(Collection) |                                                              |
| [`retainAll(Iterable  removeFrom, Collection toRetain)`](http://docs.guava-libraries.googlecode.com/git-history/release/javadoc/com/google/common/collect/Iterables.html#retainAll(java.lang.Iterable, java.util.Collection)) | Collection.retainAll(Collection) |                                                              |
| [`size(Iterable)`](http://docs.guava-libraries.googlecode.com/git-history/release/javadoc/com/google/common/collect/Iterables.html#size(java.lang.Iterable)) | Collection.size()                | [`FluentIterable.size()`](http://docs.guava-libraries.googlecode.com/git-history/release12/javadoc/com/google/common/collect/FluentIterable.html#size()) |
| [`toArray(Iterable, Class)`](http://docs.guava-libraries.googlecode.com/git-history/release/javadoc/com/google/common/collect/Iterables.html#toArray(java.lang.Iterable, java.lang.Class)) | Collection.toArray(T[])          | [`FluentIterable.toArray(Class)`](http://docs.guava-libraries.googlecode.com/git-history/release12/javadoc/com/google/common/collect/FluentIterable.html#toArray(java.lang.Class)) |
| [`isEmpty(Iterable)`](http://docs.guava-libraries.googlecode.com/git-history/release/javadoc/com/google/common/collect/Iterables.html#isEmpty(java.lang.Iterable)) | Collection.isEmpty()             | [`FluentIterable.isEmpty()`](http://docs.guava-libraries.googlecode.com/git-history/release12/javadoc/com/google/common/collect/FluentIterable.html#isEmpty()) |
| [`get(Iterable, int)`](http://docs.guava-libraries.googlecode.com/git-history/release/javadoc/com/google/common/collect/Iterables.html#get(java.lang.Iterable, int)) | List.get(int)                    | [`FluentIterable.get(int)`](http://docs.guava-libraries.googlecode.com/git- history/release12/javadoc/com/google/common/collect/FluentIterable.html#get(int)) |
| [`toString(Iterable)`](http://docs.guava-libraries.googlecode.com/git-history/release/javadoc/com/google/common/collect/Iterables.html#toString(java.lang.Iterable)) | Collection.toString()            | [`FluentIterable.toString()`](http://docs.guava-libraries.googlecode.com/git-history/release12/javadoc/com/google/common/collect/FluentIterable.html#toString()) |

#### Sets

| **方法**                                                     |
| ------------------------------------------------------------ |
| [`union(Set, Set)`](http://docs.guava-libraries.googlecode.com/git-history/release/javadoc/com/google/common/collect/Sets.html#union(java.util.Set, java.util.Set)) |
| [`intersection(Set, Set)`](http://docs.guava-libraries.googlecode.com/git-history/release/javadoc/com/google/common/collect/Sets.html#intersection(java.util.Set, java.util.Set)) |
| [`difference(Set, Set)`](http://docs.guava-libraries.googlecode.com/git-history/release/javadoc/com/google/common/collect/Sets.html#difference(java.util.Set, java.util.Set)) |
| [`symmetricDifference(Set,  Set)`](http://docs.guava-libraries.googlecode.com/git-history/release/javadoc/com/google/common/collect/Sets.html#symmetricDifference(java.util.Set, java.util.Set)) |

## Guava缓存工具

暂时用不到

```java

```



## Guava字符串工具

### Guava Joiner类

Joiner 提供了各种方法来处理字符串加入操作，对象等。

```java
// useForNull(String) 给定某个字符串来替换null
// skipNulls() 直接忽略null
Joiner joiner = Joiner.on("; ").skipNulls();
return joiner.join("Harry", null, "Ron", "Hermione");

```



### Guava Spiltter类

```java
Splitter.on(',')
        .trimResults()
        .omitEmptyStrings()
        .split("foo,bar,,   qux");

```



### Guava CharMatcher类

```java
// only the digits
System.out.println(CharMatcher.DIGIT.retainFrom("mahesh123"));
// trim whitespace at ends, and replace/collapse whitespace into single spaces
System.out.println(CharMatcher.WHITESPACE.trimAndCollapseFrom("     Mahesh     Parashar ", ' '));
// star out all digits
System.out.println(CharMatcher.JAVA_DIGIT.replaceFrom("mahesh123", "*"));
System.out.println(CharMatcher.JAVA_DIGIT.or(CharMatcher.JAVA_LOWER_CASE).retainFrom("mahesh123"));
      


```



### Guava CaseFormat类

```java

```



## Guava原语工具

Java的原生类型就是指基本类型：byte、short、int、long、float、double、char和boolean。

原生类型数组是处理原生类型集合的最有效方式（从内存和性能双方面考虑）。Guava为此提供了许多工具方法。



| **方法签名**                                    | **描述**                                             | **类似方法**                                                 | **可用性** |
| ----------------------------------------------- | ---------------------------------------------------- | ------------------------------------------------------------ | ---------- |
| List<Wrapper> asList(prim… backingArray)        | 把数组转为相应包装类的List                           | [Arrays.asList](http://docs.oracle.com/javase/6/docs/api/java/util/Arrays.html#asList(T...)) | 符号无关*  |
| prim[] toArray(Collection<Wrapper> collection)  | 把集合拷贝为数组，和collection.toArray()一样线程安全 | [Collection.toArray()](http://docs.oracle.com/javase/6/docs/api/java/util/Collection.html#toArray()) | 符号无关   |
| prim[] concat(prim[]… arrays)                   | 串联多个原生类型数组                                 | [Iterables.concat](http://docs.guava-libraries.googlecode.com/git-history/release/javadoc/com/google/common/collect/Iterables.html#concat(java.lang.Iterable...)) | 符号无关   |
| boolean contains(prim[] array, prim target)     | 判断原生类型数组是否包含给定值                       | [Collection.contains](http://docs.oracle.com/javase/6/docs/api/java/util/Collection.html#contains(java.lang.Object)) | 符号无关   |
| int indexOf(prim[] array, prim target)          | 给定值在数组中首次出现处的索引，若不包含此值返回-1   | [List.indexOf](http://docs.oracle.com/javase/6/docs/api/java/util/List.html#indexOf(java.lang.Object)) | 符号无关   |
| int lastIndexOf(prim[] array, prim target)      | 给定值在数组最后出现的索引，若不包含此值返回-1       | [List.lastIndexOf](http://docs.oracle.com/javase/6/docs/api/java/util/List.html#lastIndexOf(java.lang.Object)) | 符号无关   |
| prim min(prim… array)                           | 数组中最小的值                                       | [Collections.min](http://docs.oracle.com/javase/6/docs/api/java/util/Collections.html#min(java.util.Collection)) | 符号相关*  |
| prim max(prim… array)                           | 数组中最大的值                                       | [Collections.max](http://docs.oracle.com/javase/6/docs/api/java/util/Collections.html#max(java.util.Collection)) | 符号相关   |
| String join(String separator, prim… array)      | 把数组用给定分隔符连接为字符串                       | [Joiner.on(separator).join](http://code.google.com/p/guava-libraries/wiki/StringsExplained#Joiner) | 符号相关   |
| Comparator<prim[]>  lexicographicalComparator() | 按字典序比较原生类型数组的Comparator                 | [Ordering.natural().lexicographical()](http://docs.guava-libraries.googlecode.com/git-history/release/javadoc/com/google/common/collect/Ordering.html#lexicographical()) | 符号相关   |

*符号无关方法存在于Bytes, Shorts, Ints, Longs, Floats, Doubles, Chars, Booleans。而UnsignedInts, UnsignedLongs, SignedBytes, 或UnsignedBytes不存在。

*符号相关方法存在于SignedBytes, UnsignedBytes, Shorts, Ints, Longs, Floats, Doubles, Chars, Booleans, UnsignedInts, UnsignedLongs。而Bytes不存在。

### Guava Ints类

| 方法及说明                                                   |
| ------------------------------------------------------------ |
| **static int compare(int a, int b)**  			比较两个指定的int值。 |
| **static boolean contains(int[] array, int target)**  			返回true，如果target是否存在在任何地方数组元素。 |
| **static int[] ensureCapacity(int[] array, int minLength, int padding)**  			返回一个包含相同的值数组的数组，但保证是一个规定的最小长度。 |
| **static String join(String separator, int... array)**  			返回包含由分离器分离所提供的整型值的字符串。 |
| **static int saturatedCast(long value)**  			返回最接近的int值。 |
| **static Converter stringConverter()**  			返回使用字符串和整数之间的一个转换器序列化对象 Integer.decode(java.lang.String) 和 Integer.toString(). |
| **static int[] toArray(Collection collection)**  			返回包含集合的每个值的数组，转换为int值的方式Number.intValue(). |
| **static Integer tryParse(String string)**  			解析指定的字符串作为符号十进制整数。 |

```java

```



## Guava数学工具

Guava还另外提供了一些有用的运算函数

| **运算**    | **IntMath**                                                  | **LongMath**                                                 | **BigIntegerMath*******                                      |
| ----------- | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 最大公约数  | [`gcd(int, int)`](http://docs.guava-libraries.googlecode.com/git-history/release/javadoc/com/google/common/math/IntMath.html#gcd(int, int)) | [`gcd(long, long)`](http://docs.guava-libraries.googlecode.com/git-history/release/javadoc/com/google/common/math/LongMath.html#gcd(long, long)) | [`BigInteger.gcd(BigInteger)`](http://docs.oracle.com/javase/6/docs/api/java/math/BigInteger.html#gcd(java.math.BigInteger)) |
| 取模        | [`mod(int, int)`](http://docs.guava-libraries.googlecode.com/git-history/release/javadoc/com/google/common/math/IntMath.html#mod(int, int)) | [`mod(long, long)`](http://docs.guava-libraries.googlecode.com/git-history/release/javadoc/com/google/common/math/LongMath.html#mod(long, long)) | [`BigInteger.mod(BigInteger)`](http://docs.oracle.com/javase/6/docs/api/java/math/BigInteger.html#mod(java.math.BigInteger)) |
| 取幂        | [`pow(int, int)`](http://docs.guava-libraries.googlecode.com/git-history/release/javadoc/com/google/common/math/IntMath.html#pow(int, int)) | [`pow(long, int)`](http://docs.guava-libraries.googlecode.com/git-history/release/javadoc/com/google/common/math/LongMath.html#pow(long, int)) | [`BigInteger.pow(int)`](http://docs.oracle.com/javase/6/docs/api/java/math/BigInteger.html#pow(int)) |
| 是否2的幂   | [`isPowerOfTwo(int)`](http://docs.guava-libraries.googlecode.com/git-history/release/javadoc/com/google/common/math/IntMath.html#isPowerOfTwo(int)) | [`isPowerOfTwo(long)`](http://docs.guava-libraries.googlecode.com/git-history/release/javadoc/com/google/common/math/LongMath.html#isPowerOfTwo(long)) | [`isPowerOfTwo(BigInteger)`](http://docs.guava-libraries.googlecode.com/git-history/release/javadoc/com/google/common/math/BigIntegerMath.html#isPowerOfTwo(java.math.BigInteger)) |
| 阶乘*       | [`factorial(int)`](http://docs.guava-libraries.googlecode.com/git-history/release/javadoc/com/google/common/math/IntMath.html#factorial(int)) | [`factorial(int)`](http://docs.guava-libraries.googlecode.com/git-history/release/javadoc/com/google/common/math/LongMath.html#factorial(int)) | [`factorial(int)`](http://docs.guava-libraries.googlecode.com/git-history/release/javadoc/com/google/common/math/BigIntegerMath.html#factorial(int)) |
| 二项式系数* | [`binomial(int, int)`](http://docs.guava-libraries.googlecode.com/git-history/release/javadoc/com/google/common/math/IntMath.html#binomial(int, int)) | [`binomial(int, int)`](http://docs.guava-libraries.googlecode.com/git-history/release/javadoc/com/google/common/math/LongMath.html#binomial(int, int)) | [`binomial(int, int)`](http://docs.guava-libraries.googlecode.com/git-history/release/javadoc/com/google/common/math/BigIntegerMath.html#binomial(int, int)) |


​						
