---
title: 05 Apache Commons
date: 2020-02-09 16:45:32
categories:
- java
- module test
toc: true
tags:
---

[toc]

<!--more-->

## maven

```xml

```



## BidiMap

双向映射

```java
BidiMap<String, String> bidi = new TreeBidiMap<>();
bidi.put("One", "1");
bidi.put("Two", "2");
bidi.put("Three", "3");

// get getKey
System.out.println(bidi.get("One")); 
System.out.println(bidi.getKey("1"));
System.out.println("Original Map: " + bidi);

// inverseBidiMap
bidi.removeValue("1"); 
System.out.println("Modified Map: " + bidi);
BidiMap<String, String> inversedMap = bidi.inverseBidiMap();  
System.out.println("Inversed Map: " + inversedMap);
```

## MapIterator

JDK Map接口很难作为迭代在`EntrySet`或`KeySet`对象上迭代。 `MapIterator`提供了对`Map`的简单迭代。下面的例子说明了这一点。

**MapIterator接口示例**

```java
import org.apache.commons.collections4.IterableMap;
import org.apache.commons.collections4.MapIterator;
import org.apache.commons.collections4.map.HashedMap;

public class MapIteratorTester {
   public static void main(String[] args) {
      IterableMap<String, String> map = new HashedMap<>();
      map.put("1", "One");
      map.put("2", "Two");
      map.put("3", "Three");
      map.put("4", "Four");
      map.put("5", "Five");

      MapIterator<String, String> iterator = map.mapIterator();
      while (iterator.hasNext()) {
         Object key = iterator.next();
         Object value = iterator.getValue();
         System.out.println("key: " + key);
         System.out.println("Value: " + value);
         iterator.setValue(value + "_");
      }
      System.out.println(map);
   }
}
```

## OrderedMap接口 				 				 				 			

`OrderedMap`是映射的新接口，用于**保留添加元素的顺序**。 

`LinkedMap`和`ListOrderedMap`是两种可用的实现。 此接口支持`Map`的迭代器，并允许在Map中向前或向后两个方向进行迭代。 下面的例子说明了这一点。

**示例代码**

*OrderedMapTester.java* - 

```java
import org.apache.commons.collections4.OrderedMap;
import org.apache.commons.collections4.map.LinkedMap;

public class OrderedMapTester {
   public static void main(String[] args) {
      OrderedMap<String, String> map = new LinkedMap<String, String>();
      map.put("One", "1");
      map.put("Two", "2");
      map.put("Three", "3");

      System.out.println(map.firstKey());
      System.out.println(map.nextKey("One"));
      System.out.println(map.nextKey("Two"));  
   }
}
```

## CollectionUtils.addIgnoreNull()

**检查是否为空元素**

CollectionUtils的`addIgnoreNull()`方法可用于确保只有非空(`null`)值被添加到集合中。

**声明**

以下是`org.apache.commons.collections4.CollectionUtils.addIgnoreNull()`的声明 - 

```java
public static <T> boolean addIgnoreNull(Collection<T> collection, T object)


Java
```

**参数**

- *collection* - 要添加到的集合，不能为`null`值。 
- *object* - 要添加的对象，如果为`null`，则不会添加。

**返回值**

- 如果集合已更改，则返回为`True`。

**示例**

以下示例显示`org.apache.commons.collections4.CollectionUtils.addIgnoreNull()`方法的用法。在示例中试图添加一个空值和一个非空值。

```java
import java.util.LinkedList;
import java.util.List;

import org.apache.commons.collections4.CollectionUtils;

public class CollectionUtilsTester {
   public static void main(String[] args) {
      List<String> list = new LinkedList<String>();

      CollectionUtils.addIgnoreNull(list, null);
      CollectionUtils.addIgnoreNull(list, "a");

      System.out.println(list);

      if(list.contains(null)) {
         System.out.println("Null value is present");
      } else {
         System.out.println("Null value is not present");
      }
   }
}
```

## CollectionUtils.collate()  合并并排序

CollectionUtils的`collate()`方法可用于合并两个已排序的列表。

**声明**
以下是`org.apache.commons.collections4.CollectionUtils.collate()`方法的声明 - 

```java
public static <O extends Comparable<? super O>> List<O> 
   collate(Iterable<? extends O> a, Iterable<? extends O> b)


Java
```

**参数**

- *a* - 第一个集合，不能为`null`。
- *b* - 第二个集合不能为`null`。

**返回值**

- 一个新的排序列表，其中包含集合`a`和`b`的元素。

**异常**

- `NullPointerException` - 如果其中一个集合为`null`。

**示例**

以下示例显示了用法`org.apache.commons.collections4.CollectionUtils.collate()`方法。

我们将合并两个已排序的列表，然后打印已合并和已排序的列表。

```java
import java.util.Arrays;
import java.util.List;

import org.apache.commons.collections4.CollectionUtils;

public class CollectionUtilsTester {
   public static void main(String[] args) {
      List<String> sortedList1 = Arrays.asList("A","C","E");
      List<String> sortedList2 = Arrays.asList("B","D","F");
      List<String> mergedList = CollectionUtils.collate(sortedList1, sortedList2);
      System.out.println(mergedList); 
   }
}
```



## isEmpty()  isNotEmpty()

```java
// CollectionUtils.isNotEmpty(list)  等价于
static boolean checkNotEmpty1(List<String> list) {
		return !(list == null || list.isEmpty());
}
// isEmpty() 与之相反
```



```java
// 子集
CollectionUtils.isSubCollection(list2, list1);
// 相交
CollectionUtils.intersection(list1, list2);
// 差集
CollectionUtils.subtract(list1, list2);
// 联合
CollectionUtils.union(list1, list2);
```



