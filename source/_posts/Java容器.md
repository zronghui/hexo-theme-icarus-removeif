---
title: Java容器
toc: true
recommend: 1
uniqueId: '2020-09-08 15:37:00/"Java容器".html'
date: 2020-09-08 23:37:00
thumbnail:
categories:
tags:
keywords:
---

[TOC]

<!--more-->



```java

// size > loadFactor * capacity 时需要扩充 capacity(默认*2)
// accessOrder 默认为 false，维护 插入顺序
//             为 true 时，在调用 afterNodeAccess() 方法时，会将当前访问的节点移到链表尾部
// 所以，链表头部是最旧的元素，尾部是最新的


// public LinkedHashMap(int initialCapacity, float loadFactor, boolean accessOrder) {
//     super(initialCapacity, loadFactor);
//     this.accessOrder = accessOrder;
// }

class LRUCache<K, V> extends LinkedHashMap<K, V>{
    private int cacheSize;
    LRUCache(int cacheSize){
        super(16, 0.75, true);
        this.cacheSize = cacheSize;
    }
    protected boolean removeEldestEntry(){
        // 此方法在 afterNodeInsertion 中调用，所以此时已经插入新节点
        return this.size()>cacheSize;
    }
}
```

