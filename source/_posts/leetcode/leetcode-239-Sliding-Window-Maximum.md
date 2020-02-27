---
title: leetcode 239. Sliding Window Maximum
date: 2019-12-04 13:52:48
categories:
- leetcode
toc: true
tags:
- Heap
- Sliding Window
---
### 难度：Hard

<a href="https://leetcode.com/problems/sliding-window-maximum/">leetcode</a>
<a href="https://www.jiuzhang.com/solution/sliding-window-maximum/">九章</a>
## 题目描述
Given an array _nums_ , there is a sliding window of size _k_ which is moving
from the very left of the array to the very right. You can only see the _k_
numbers in the window. Each time the sliding window moves right by one
position. Return the max sliding window.

**Example:**
        
    Input: _nums_ = [1,3,-1,-3,5,3,6,7], and _k_ = 3
    Output:[3,3,5,5,6,7] 
    Explanation:
    Window position                Max
    ---------------               -----
    [1  3  -1] -3  5  3  6  7       **3**
     1 [3  -1  -3] 5  3  6  7       **3**
     1  3 [-1  -3  5] 3  6  7      **5**
     1  3  -1 [-3  5  3] 6  7       **5**
     1  3  -1  -3 [5  3  6] 7       **6**
     1  3  -1  -3  5 [3  6  7]      **7**
    

**Note:**  
You may assume _k_ is always valid, 1 ≤ k ≤ input array's size for non-empty
array.

**Follow up:**  
Could you solve it in linear time?


**Tags:** Heap, Sliding Window

**Difficulty:** Hard
## 答案
<!--more-->
```java
// /Volumes/My Passport/data/ut下载/03     算法/高频算法面试题精讲/3. 3栈和队列面试题精讲.mp4 例6
// julyedu.com
class Solution {
    public int[] maxSlidingWindow(int[] nums, int k) {
        if(k<=1) return nums;
        // 存放索引
        Deque<Integer> deque = new ArrayDeque<>();
        int n = nums.length;
        int[] res = new int[n-(k-1)];
        // 添加前k-1个数
        for(int i=0; i<k-1; i++) {
            // 若即将插入的值比之前的数大，删除旧的数字，因为新数更晚被删除，旧数永远不会被选中
            while(!deque.isEmpty()&&nums[deque.getLast()]<=nums[i]) deque.pollLast();
            deque.add(i);
        }
        
        for(int i=k-1; i<n; i++) {
            while(!deque.isEmpty()&&nums[deque.getLast()]<=nums[i]) deque.pollLast();
            // 0 <-> k-1
            // 第0个数在i=k-1时保留
            // 删除窗口之前的数字
            while(!deque.isEmpty()&&deque.getFirst()<i-(k-1)) deque.pollFirst();
            deque.add(i);
            res[i-(k-1)] = nums[deque.getFirst()];
        }
        return res;
    }
}
```
