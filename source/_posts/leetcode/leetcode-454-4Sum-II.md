---
title: leetcode 454. 4Sum II
date: 2019-11-28 15:42:06
categories:
- leetcode
toc: true
tags:
- Hash Table
- Binary Search
---
### 难度：Middle

<a href="https://leetcode.com/problems/4sum-ii/">leetcode</a>
<a href="https://www.jiuzhang.com/solution/4sum-ii/">九章</a>
## 题目描述
Given four lists A, B, C, D of integer values, compute how many tuples `(i, j,
k, l)` there are such that `A[i] + B[j] + C[k] + D[l]` is zero.

To make problem a bit easier, all A, B, C, D have same length of N where 0 ≤ N
≤ 500\. All integers are in the range of -228 to 228 \- 1 and the result is
guaranteed to be at most 231 \- 1.

**Example:**
        
    Input:
    A = [ 1, 2]
    B = [-2,-1]
    C = [-1, 2]
    D = [ 0, 2]
    
    Output:
    2
    
    Explanation:
    The two tuples are:
    1. (0, 0, 0, 1) -> A[0] + B[0] + C[0] + D[1] = 1 + (-2) + (-1) + 2 = 0
    2. (1, 1, 0, 0) -> A[1] + B[1] + C[0] + D[0] = 2 + (-1) + (-1) + 0 = 0
    




**Tags:** Hash Table, Binary Search

**Difficulty:** Medium
## 答案
<!--more-->
```java
class Solution {
    public int fourSumCount(int[] a, int[] b, int[] c, int[] d) {
        HashMap<Integer, Integer> map = new HashMap<>();
        for(int i: a){
            for(int j: b){
                if(map.containsKey(i+j)){
                    map.put(i+j, map.get(i+j)+1);
                }else{
                    map.put(i+j, 1);
                }
            }
        }
        int res = 0;
        for(int i: c){
            for(int j: d){
                int target = 0 - i - j;
                if(map.containsKey(target)){
                    res += map.get(target);
                }
            }
        }
        return res;
    }
}
```
