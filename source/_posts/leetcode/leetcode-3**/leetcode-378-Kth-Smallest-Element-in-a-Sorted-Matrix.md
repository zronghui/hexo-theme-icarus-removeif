---
title: leetcode 378. Kth Smallest Element in a Sorted Matrix
date: 2019-11-27 21:00:31
categories:
- leetcode
- leetcode-3**
toc: true
tags:
- Binary Search
- Heap
---
### 难度：Middle

<a href="https://leetcode.com/problems/kth-smallest-element-in-a-sorted-matrix/">leetcode</a>
<a href="https://www.jiuzhang.com/solution/kth-smallest-element-in-a-sorted-matrix/">九章</a>
## 题目描述
Given a _n_ x _n_ matrix where each of the rows and columns are sorted in
ascending order, find the kth smallest element in the matrix.

Note that it is the kth smallest element in the sorted order, not the kth
distinct element.

**Example:**
        
    matrix = [
       [ 1,  5,  9],
       [10, 11, 13],
       [12, 13, 15]
    ],
    k = 8,
    
    return 13.


**Note:**  
You may assume k is always valid, 1 ≤ k ≤ n2.


**Tags:** Binary Search, Heap

**Difficulty:** Medium
## 答案
<!--more-->
```java
class Solution {
    // priorityQueue  
    // Runtime: 30 ms, faster than 22.86% of Java online submissions 
    // Memory Usage: 43 MB, less than 83.78% of Java online submissions 
    public int kthSmallest_0(int[][] ma, int k) {
        PriorityQueue<Integer> pq = new PriorityQueue<>(k);
        for(int i=0; i<ma.length; i++){
            for(int j=0; j<ma[0].length; j++){
                pq.add(ma[i][j]);
            }
        }
        for(int i=0; i<k-1; i++) {
            pq.poll();
        }
        return pq.poll();
    }
}
```

```python
class Solution:
    def kthSmallest(self, matrix: List[List[int]], k: int) -> int:
        n = len(matrix)
        pq = [(matrix[i][0], i, 0) for i in range(n)]
        heapq.heapify(pq)
        for i in range(k-1):
            _, x, y = heapq.heappop(pq)
            if y+1!=n:
                heapq.heappush(pq, (matrix[x][y+1], x, y+1))
        return heapq.heappop(pq)[0]

```













