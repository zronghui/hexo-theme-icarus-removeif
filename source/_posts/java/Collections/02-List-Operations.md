---
title: List Operations
description: List Operations
date: 2020-02-16 16:47:30
categories:
- java
- Collections
toc: true
tags:
---

[toc]

## Converting Iterator to List

```java
// 有时会获得一个 Iterator
Iterator<Integer> iterator = Arrays.asList(1, 2, 3).iterator();
// 怎么转成 List 呢
List<Integer> actualList = new ArrayList<>();
// 1. using a while loop
while (iterator.hasNext()) {
  actualList.add(iterator.next());
}
// 2. using java 8 Iterator.forEachRemaining
iterator.forEachRemaining(actualList::add);
// 3. using java 8 streams API  ?
// 先把 iterator 转成 iterable
Iterable<Integer> iterable = () -> iterator;
List<Integer> actualList = StreamSupport
  .stream(iterable.spliterator(), false)
  .collect(Collectors.toList());
// 4. Guava
// 4.1 Immutable List
List<Integer> actualList = ImmutableList.copyOf(iterator);
// 4.2 mutable list
List<Integer> actualList = Lists.newArrayList(iterator);
// 5. apache Commons
List<Integer> actualList = IteratorUtils.toList(iterator);

assertThat(actualList, containsInAnyOrder(1, 2, 3));
```



## Java – Get Random Item/Element From a List

```java
List<Integer> givenList = Arrays.asList(1, 2, 3);
// 2.1. Single Random Item
Random rand = new Random();
givenList.get(rand.nextInt(givenList.size()));
// 2.2. in Multithread Environment
int randomElementIndex = THreadLocalRandom.current().nextInt(listSize) % givenList.size();
// 2.4. Without Repetitions
// 获取后 remove 元素即可
givenList.remove(randomIndex);
// 2.5. Select Random Series
Collections.shuffle(givenList);
```



## Partition a List in Java

```java
// 将一个 list 划分为指定长度的 sublists
List<Integer> intList = Lists.newArrayList(1, 2, 3, 4, 5, 6, 7, 8, 9); // guava
// 1. Guava
//   Keep in mind that the partitions are sublist views of the original collection
//   which means that changes in the original collection will be reflected in the partitions
// 1.1. Use Guava to Partition the List
Lists.partition(intList, 3);
// 1.2. Use Guava to partition a Collection
//   略
// 2. Use Apache Commons Collections to Partition the List
//    the same caveat applies here as well – 
//    the resulting partition are views of the original List.
ListUtils.partition(intList, 3);
// 3. Use Java8 to Partition the List
// 3.1. Collectors partitioningBy
//    use Collectors.partitioningBy() to split the list into 2 sublists
Map<Boolean, List<Integer>> groups = 
      intList.stream().collect(Collectors.partitioningBy(s -> s > 6));
List<List<Integer>> subSets = new ArrayList<List<Integer>>(groups.values());
// 3.2. Collectors groupingBy
//    use Collectors.groupingBy() to split our list to multiple partitions
Map<Integer, List<Integer>> groups = 
      intList.stream().collect(Collectors.groupingBy(s -> (s - 1) / 3));
List<List<Integer>> subSets = new ArrayList<List<Integer>>(groups.values());
// 3.3. Split the List by Separator
//    use Java8 to split our List by separator
int[] indexes = Stream.of(IntStream.of(-1), IntStream.range(0, intList.size())
  .filter(i -> intList.get(i) == 0), IntStream.of(intList.size()))
  .flatMapToInt(s -> s).toArray();
List<List<Integer>> subSets = IntStream.range(0, indexes.length - 1)
  .mapToObj(i -> intList.subList(indexes[i] + 1, indexes[i + 1]))
  .collect(Collectors.toList());
```



## Removing all nulls from a List in Java

```java
List<Integer> list = Lists.newArrayList(null, 1, null);
// 1. plain java
while (list.remove(null));
// 或者
list.removeAll(Collections.singleton(null)); // ?
// 2. Guava
// 2.1 via predicates (modify original list)
Iterables.removeIf(list, Predicates.isNull());
// 2.2 do not modify original list
List<Integer> listWithoutNulls = Lists.newArrayList(
  Iterables.filter(list, Predicates.notNull()));
assertThat(list, hasSize(1));
// 3. Commons
// (modify original list)
CollectionUtils.filter(list, PredicateUtils.notNullPredicate());
// 4. java8 lambdas
// 4.1 parallel (do not modify original list)
list.parallelStream()
  .filter(Objects::nonNull)
  .collect(Collectors.toList());
// 4.2 serial (do not modify original list)
list.stream()
  .filter(Objects::nonNull)
  .collect(Collectors.toList());
// 4.3 (modify original list)
list.removeIf(Objects::isNull);
```



## Removing all duplicates from a List in Java

```java
List<Integer> list = Lists.newArrayList(0, 1, 2, 3, 0, 0);
// 以下做法均类似于 Python 的 list(set(l))
// 1. plain java
// using set
List<Integer> listWithoutDuplicates = new ArrayList<>(new HashSet<>(list));
// 2. Guava
Lists.newArrayList(Sets.newHashSet(list));
// 3. java 8 stream
list.stream().distinct().collect(Collectors.toList());

assertThat(listWithoutDuplicates, hasSize(4));
```

## Check If Two Lists are Equal in Java

```java
List<String> list1 = Arrays.asList("1", "2", "3", "4");
List<String> list2 = Arrays.asList("1", "2", "3", "4");
List<String> list3 = Arrays.asList("1", "2", "4", "3");
// 1. JUnit
Assert.assertEquals(list1, list2);
Assert.assertNotSame(list1, list2);
Assert.assertNotEquals(list1, list3);
// 2. TestNG (用法类似)
Assert.assertEquals(list1, list2);
Assert.assertNotSame(list1, list2);
Assert.assertNotEquals(list1, list3);
// 3. AssertJ
assertThat(list1)
  .isEqualTo(list2)
  .isNotEqualTo(list3);
assertThat(list1.equals(list2)).isTrue;
assertThat(list1.equals(list3)).isFalse;
```



## How to Find an Element in a List with Java

```java
// contains indexOf loop
// 略
// java 8 stream api
// invoke stream() on the list
// call the filter() method with a proper Predicate
// call the findAny() construct which returns the first element 
// that matches the filter predicate wrapped in an Optional if such an element exists
customers.stream()
  .filter(customer -> "James".equals(customer.getName()))
  .findAny()
  .orElse(null);
// guava / commons
// 写的很晦涩，略
```



## Java List UnsupportedOperationException

```java
List<String> flowerList = Arrays.asList(flowers);
// Arrays.asList() 返回的 list 大小固定，不可增删（修改呢？）
// 改进, 若要修改
new ArrayList(Arrays.asList(flowers));
```



## Copy a List to Another List in Java

```java
// 1. 利用构造函数

List<Integer> copy = new ArrayList<>(list);
// 2. addAll
List<Integer> copy = new ArrayList<>();
copy.addAll(list);
// 但是: 这 2 种方法只是复制了引用，修改一个会影响另一个

// 3. Collections.copy
//   src dest 长度一样，dest 会被覆盖
List<Integer> source = Arrays.asList(1,2,3);
List<Integer> dest = Arrays.asList(4,5,6);
Collections.copy(dest, source);
//   dest 留出前三个位置
List<Integer> source = Arrays.asList(1, 2, 3);
List<Integer> dest = Arrays.asList(5, 6, 7, 8, 9, 10);
Collections.copy(dest, source);

// 4. java 8 stream
list.stream().collect(Collectors.toList());
```





## Remove All Occurrences of a Specific Value from a List

```java
// list.remove(1) 若 1 是 int, 会被当成索引，不是 value，是 integer 才行
// void removeAll(List<Integer> list, int element) {
void removeAll(List<Integer> list, Integer element) {
    while (list.contains(element)) {
        list.remove(element);
    }
}
// 或者
void removeAll(List<Integer> list, Integer element) {
    int index;
    while ((index = list.indexOf(element)) >= 0) {
        list.remove(index);
    }
}

// 但以上方法都很慢
// for loop, 但需要考虑到同时删除相邻元素时的问题
// 因此，因为 for-each loop 没有解决同时删除相邻元素时的问题，test failed
void removeAll(List<Integer> list, int element) {
    for (int i = 0; i < list.size();) {
        if (Objects.equals(element, list.get(i))) {
            list.remove(i);
        } else {
            i++;
        }
    }
}
// using an Iterator
void removeAll(List<Integer> list, int element) {
    for (Iterator<Integer> i = list.iterator(); i.hasNext();) {
        Integer number = i.next();
        if (Objects.equals(number, element)) {
            i.remove();
        }
    }
}
// 建立一个新 list
List<Integer> removeAll(List<Integer> list, int element) {
    List<Integer> remainingElements = new ArrayList<>();
    for (Integer number : list) {
        if (!Objects.equals(number, element)) {
            remainingElements.add(number);
        }
    }
    return remainingElements;
}
// 或者返回 void
void removeAll(List<Integer> list, int element) {
    List<Integer> remainingElements = new ArrayList<>();
    for (Integer number : list) {
        if (!Objects.equals(number, element)) {
            remainingElements.add(number);
        }
    }
 
    list.clear();
    list.addAll(remainingElements);
}
// stream api
list.stream().filter(e -> !Object.equals(e, element))
  .collect(Collectors.toList());

// 最简单的方法（你不早说）：
// removeIf
list.removeIf(n -> Objects.equals(n, element));
```



## Add Multiple Items to an Java ArrayList

```java
// list1.addAll(list2)   
// Collections.addAll(list1, list2)
// Collections.addAll(list1, 1, 2, 3)
// 以上 2 种方法添加的都是引用

// using stream 更简便，还能用 skip filter
source.stream().forEachOrdered(target::add);
// 不能用collect(target::add) ，因为 collect 将所有元素看为一个整体

```



## Remove the First Element from a List

```java
// ArrayList
list.remove(0);
// linkedList
list.removeFirst()
```



## Ways to Iterate Over a List in Java

```java
// loop foreach loop Iterator
// list.forEach
countries.forEach(System.out::println);
// Stream.forEach()
countries.stream().forEach((c) -> System.out.println(c));
```



## Intersection of Two Lists in Java

```java
List<String> list = Arrays.asList("red", "blue", "blue", "green", "red");
List<String> otherList = Arrays.asList("red", "green", "green", "yellow");

Set<String> result = list.stream().filter(otherList::contains)
  .collect(Collectors.toSet());
```



