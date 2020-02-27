---
title: List Implementations
description: List Implementations
date: 2020-02-16 16:47:12
categories:
- java
- Collections
toc: true
tags:
---

[toc]

## [A Guide to the Java LinkedList](https://www.baeldung.com/java-linkedlist)

```java
List list = Collections.synchronizedList(new LinkedList(...));
```



## [Guide to the Java ArrayList](https://www.baeldung.com/java-arraylist)

```java
// Constructor Accepting Initial Capacity**
List list = new ArrayList<>(20);

// Constructor Accepting Collection
Collection<Integer> number = IntStream.range(0, 10).boxed().collect(toSet());
List<Integer> list = new ArrayList<>(numbers);
assertEquals(10, list.size());
assertTrue(numbers.containsAll(list));

// insert a collection or several elements at once:
List<Long> list = new ArrayList<>(Arrays.asList(1L, 2L, 3L));
LongStream.range(4, 10).boxed()
  .collect(collectingAndThen(toCollection(ArrayList::new), ys -> list.addAll(0, ys)));
assertThat(Arrays.asList(4L, 5L, 6L, 7L, 8L, 9L, 1L, 2L, 3L), equalTo(list));

```



## [Immutable ArrayList in Java](https://www.baeldung.com/java-immutable-list)

```java
// With the JDK
//   Collections.unmodifiableList(list);
List<String> list = new ArrayList<>(Arrays.asList("one", "two", "three"));
List<String> unmodifiableList = Collections.unmodifiableList(list);

// With Java 9
//  List.of(E… elements)
final List<String> unmodifiableList = List.of(list.toArray(new String[]{}));

// With Guava
//  ImmutableList.copyOf(list);
List<String> list = new ArrayList<>(Arrays.asList("one", "two", "three"));
List<String> unmodifiableList = ImmutableList.copyOf(list);

// With the Apache Collections Commons
ListUtils.unmodifiableList(list);
```





## [Guide to CopyOnWriteArrayList](https://www.baeldung.com/java-copy-on-write-arraylist)

**Iterating Over CopyOnWriteArrayList While Inserting**

when we create an iterator for the *CopyOnWriteArrayList,* we get an immutable snapshot of the data in the list at the time *iterator()* was called.

```java
assertThat(result).containsOnly(1, 3, 5, 8);  // ?
```



**Removing While Iterating Is Not Allowed**

允许修改元素，但是不允许删除

## [Multi Dimensional ArrayList in Java](https://www.baeldung.com/java-multi-dimensional-arraylist)
