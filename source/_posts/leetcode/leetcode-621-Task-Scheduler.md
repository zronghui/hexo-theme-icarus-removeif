---
title: leetcode 621. Task Scheduler
date: 2019-12-06 19:15:01
categories:
- leetcode
toc: true
tags:
- Array
- Greedy
- Queue
---
### 难度：Middle

<a href="https://leetcode.com/problems/task-scheduler/">leetcode</a>
<a href="https://www.jiuzhang.com/solution/task-scheduler/">九章</a>
## 题目描述
Given a char array representing tasks CPU need to do. It contains capital
letters A to Z where different letters represent different tasks. Tasks could
be done without original order. Each task could be done in one interval. For
each interval, CPU could finish one task or just be idle.

However, there is a non-negative cooling interval **n** that means between two
**same tasks** , there must be at least n intervals that CPU are doing
different tasks or just be idle.

You need to return the **least** number of intervals the CPU will take to
finish all the given tasks.



**Example:**
        
    Input: tasks = ["A","A","A","B","B","B"], n = 2
    Output: 8
    Explanation: A -> B -> idle -> A -> B -> idle -> A -> B.
    



**Note:**

  1. The number of tasks is in the range [1, 10000].
  2. The integer n is in the range [0, 100].


**Tags:** Array, Greedy, Queue

**Difficulty:** Medium
## 答案
<!--more-->
```java
// 代码：https://leetcode-cn.com/problems/task-scheduler/solution/ren-wu-diao-du-qi-by-leetcode/
// 讲解：https://www.youtube.com/watch?v=YCD_iYxyXoo
class Solution {
    public int leastInterval(char[] tasks, int n) {
        int[] map = new int[26];
        for(char c: tasks) {
            map[c-'A']++;
        }
        Arrays.sort(map);
        int max_val = map[25]-1, idle_slots = max_val*n;
        for(int i=0; i<25; i++) {
            idle_slots -= Math.min(max_val, map[i]);
        }
        return idle_slots>0?tasks.length+idle_slots:tasks.length;
    }
}
```
