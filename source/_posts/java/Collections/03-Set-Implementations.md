---
title: Set Implementations
description: Set Implementations
date: 2020-02-16 16:47:42
categories:
- java
- Collections
toc: true
tags:
---

[toc]

## A Guide to TreeSet in Java

```java
// 2. Intro to TreeSet
// 2.1. TreeSet With a Constructor Comparator Param
Set<String> treeSet = new TreeSet<>(Comparator.comparing(String::length));
// 2.2 synchronized treeSet
Set<String> syncTreeSet = Collections.synchronizedSet(treeSet);
// 3. TreeSet add()
//   返回 boolean, 源码如下，可以看到内部用的是 treemap
public boolean add(E e) {
    return m.put(e, PRESENT) == null;
}
// 4. TreeSet contains()
// 5. TreeSet remove()  返回 boolean
// 6. TreeSet clear()
// 7. TreeSet size()
// 8. TreeSet isEmpty()
// 9. TreeSet iterator()--升序 descendingIterator()--降序
// 若迭代时用treeSet 的 remove 方法，会触发 fail-fast 机制，抛出 ConcurrentModificationException 异常
while (itr.hasNext()) {
  itr.next();
  treeSet.remove("Second");
}
// 但是若用 Iterator 的 remove 方法就没问题
while (itr.hasNext()) {
  String element = itr.next();
  if (element.equals("Second"))
    itr.remove();
}

// 10. TreeSet first()
//   size() 为 0 的话抛出异常
// 11. TreeSet last()
// 12. TreeSet subSet()
//   return the elements ranging from fromElement to toElement
// 13. TreeSet headSet()
//   return elements of TreeSet which are smaller than the specified element:
// 14. TreeSet tailSet()
//   return the elements of a TreeSet which are greater than or equal to the specified element
// 15. Storing Null Elements
//    不可添加 null

```



## A Guide to HashSet in Java

内部维护了一个 HashMap

#### How HashSet Maintains Uniqueness?

When we put an object into a *HashSet*, it uses the object's ***hashcode*** value to determine if an element is not in the set already.

Each hash code value corresponds to a certain bucket location which can contain various elements, for which the calculated hash value is the same. **But two objects with the same \*hashCode\* might not be equal**.

So, objects within the same bucket will be compared using the *equals()* method.
