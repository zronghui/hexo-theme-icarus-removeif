---
title: leetcode 380. Insert Delete GetRandom O(1)
date: 2019-11-30 18:57:30
categories:
- leetcode
- leetcode-3**
toc: true
tags:
- Array
- Hash Table
- Design
---
### 难度：Middle

<a href="https://leetcode.com/problems/insert-delete-getrandom-o1/">leetcode</a>
<a href="https://www.jiuzhang.com/solution/insert-delete-getrandom-o1/">九章</a>
## 题目描述
Design a data structure that supports all following operations in _average_
**O(1)** time.

  1. `insert(val)`: Inserts an item val to the set if not already present.
  2. `remove(val)`: Removes an item val from the set if present.
  3. `getRandom`: Returns a random element from current set of elements. Each element must have the **same probability** of being returned.

**Example:**
        
    // Init an empty set.
    RandomizedSet randomSet = new RandomizedSet();
    
    // Inserts 1 to the set. Returns true as 1 was inserted successfully.
    randomSet.insert(1);
    
    // Returns false as 2 does not exist in the set.
    randomSet.remove(2);
    
    // Inserts 2 to the set, returns true. Set now contains [1,2].
    randomSet.insert(2);
    
    // getRandom should return either 1 or 2 randomly.
    randomSet.getRandom();
    
    // Removes 1 from the set, returns true. Set now contains [2].
    randomSet.remove(1);
    
    // 2 was already in the set, so return false.
    randomSet.insert(2);
    
    // Since 2 is the only number in the set, getRandom always return 2.
    randomSet.getRandom();
    


**Tags:** Array, Hash Table, Design

**Difficulty:** Medium
## 答案
<!--more-->
```java
class RandomizedSet {

    List<Integer> l;  // val
    Map<Integer, Integer> map; // val to index
    Random r;
    
    public RandomizedSet() {
        l = new ArrayList<>();
        map = new HashMap<>();
        r = new Random();
    }
    
    
    public boolean insert(int val) {
        if(map.containsKey(val)){
            return false;
        }else{
            l.add(val);
            map.put(val, l.size()-1);
            return true;
        }
    }
    
    
    public boolean remove(int val) {
        if(map.containsKey(val)){
            int index = map.get(val);
            if(index!=l.size()-1){
                int end = l.get(l.size()-1);
                l.set(index, end);
                map.put(end, index);
            }
            l.remove(l.size()-1);
            map.remove(val);
            return true;
        }else{
            return false;
        }
    }
    
    
    public int getRandom() {
        return l.get(r.nextInt(l.size()));
    }
}


```
