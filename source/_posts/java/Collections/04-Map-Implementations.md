---
title: Map Implementations
description: Map Implementations
date: 2020-02-16 16:47:52
categories:
- java
- Collections
toc: true
tags:
---

[toc]



# The Java HashMap Under the Hood

```java

```

# A Guide to TreeMap in Java


```java
TreeMap<Integer, String> treeMap = new TreeMap<>(Comparator.reverseOrder());
treeMap.put(1, "a");
treeMap.put(2, "a");
treeMap.put(3, "a");
treeMap.put(4, "a");
treeMap.put(5, "a");

System.out.println(treeMap.lastKey());
System.out.println(treeMap.firstKey());
System.out.println(treeMap.headMap(3).keySet().toString());
System.out.println(treeMap.tailMap(3).keySet().toString());
//1
//5
//[5, 4]
//[3, 2, 1]
```

# Java TreeMap vs HashMap

2. Differences
2.1. Implementation
   红黑树      hashtable
2.2. Order
2.3. Null Values
   不允许 null 作为 key     允许 null 作为 key
3. Performance Analysis
    3.1. HashMap
    3.2. TreeMap
4. Similarities
    4.1. Unique Elements
  4.2. Concurrent Access
      Both Map implementations aren't synchronized 
      We have to explicitly use Collections.synchronizedMap(mapName) to obtain a synchronized view of a provided map.
    4.3. Fail-Fast Iterators

# Guide to WeakHashMap in Java

WeakHashMap 的用途？


```java
// 2. Strong, Soft, and Weak References
// 2.1. Strong References
Integer prime = 1;

// 2.2. Soft References
// an object that has a SoftReference pointing to it won't be garbage collected until the JVM absolutely needs memory.
Integer prime = 1;  
SoftReference<Integer> soft = new SoftReference<Integer>(prime); 
prime = null;
// The prime object has a strong reference pointing to it.
// Next, we are wrapping prime strong reference into a soft reference. After making that strong reference null, a prime object is eligible for GC but will be collected only when JVM absolutely needs memory.

// 2.3. Weak References
// The objects that are referenced only by weak references are garbage collected eagerly; the GC won't wait until it needs memory in that case.
Integer prime = 1;  
WeakReference<Integer> soft = new WeakReference<Integer>(prime); 
prime = null;
// When we made a prime reference null, the prime object will be garbage collected in the next GC cycle, as there is no other strong reference pointing to it.

// 3. WeakHashMap as an Efficient Memory Cache
// References of a WeakReference type are used as keys in WeakHashMap.
WeakHashMap<UniqueImageName, BigImage> map = new WeakHashMap<>();
BigImage bigImage = new BigImage("image_id");
UniqueImageName imageName = new UniqueImageName("name_of_big_image");
 
map.put(imageName, bigImage);
assertTrue(map.containsKey(imageName));
 
imageName = null;
System.gc();
 
await().atMost(10, TimeUnit.SECONDS).until(map::isEmpty);
```

# A Guide to ConcurrentMap


```java
// 2. ConcurrentMap   是一个 interface
// Several default implementations are overridden, disabling the null key/value support:
getOrDefault
forEach
replaceAll
computeIfAbsent
computeIfPresent
compute
merge
// The following APIs are also overridden to support atomicity, without a default interface implementation:
putIfAbsent
remove
replace(key, oldValue, newValue)
replace(key, value)
  
// 3. ConcurrentHashMap
// 3.1. Thread-Safety
// 3.2. Null Key/Value
// 3.3. Stream Support
// 3.4. Performance
// 3.5. Pitfalls
// 4. ConcurrentNavigableMap
// 5. ConcurrentSkipListMap

```

# Guide to the ConcurrentSkipListMap


```java

```

# An Introduction to Java.util.Hashtable Class


```java

```

# A Guide to LinkedHashMap in Java


```java

```

# A Guide to EnumMap


```java

```

# Immutable Map Implementations in Java


```java

```

# A Guide to Java HashMap


```java

```
