---
title: leetcode 134. Gas Station
date: 2019-12-04 11:51:30
categories:
- leetcode
- leetcode-1**
toc: true
tags:
- Greedy
---
### 难度：Middle

<a href="https://leetcode.com/problems/gas-station/">leetcode</a>
<a href="https://www.jiuzhang.com/solution/gas-station/">九章</a>
## 题目描述
There are _N_ gas stations along a circular route, where the amount of gas at
station _i_ is `gas[i]`.

You have a car with an unlimited gas tank and it costs `cost[i]` of gas to
travel from station _i_ to its next station ( _i_ +1). You begin the journey
with an empty tank at one of the gas stations.

Return the starting gas station's index if you can travel around the circuit
once in the clockwise direction, otherwise return -1.

**Note:**

  * If there exists a solution, it is guaranteed to be unique.
  * Both input arrays are non-empty and have the same length.
  * Each element in the input arrays is a non-negative integer.

**Example 1:**
        
    Input: 
    gas  = [1,2,3,4,5]
    cost = [3,4,5,1,2]
    
    Output: 3
    
    Explanation: Start at station 3 (index 3) and fill up with 4 unit of gas. Your tank = 0 + 4 = 4
    Travel to station 4. Your tank = 4 - 1 + 5 = 8
    Travel to station 0. Your tank = 8 - 2 + 1 = 7
    Travel to station 1. Your tank = 7 - 3 + 2 = 6
    Travel to station 2. Your tank = 6 - 4 + 3 = 5
    Travel to station 3. The cost is 5. Your gas is just enough to travel back to station 3.
    Therefore, return 3 as the starting index.
    

**Example 2:**
        
    Input: 
    gas  = [2,3,4]
    cost = [3,4,3]
    
    Output: -1
    
    Explanation: You can't start at station 0 or 1, as there is not enough gas to travel to the next station.
    Let's start at station 2 and fill up with 4 unit of gas. Your tank = 0 + 4 = 4
    Travel to station 0. Your tank = 4 - 3 + 2 = 3
    Travel to station 1. Your tank = 3 - 3 + 3 = 3
    You cannot travel back to station 2, as it requires 4 unit of gas but you only have 3.
    Therefore, you can't travel around the circuit once no matter where you start.
    


**Tags:** Greedy

**Difficulty:** Medium
## 答案
<!--more-->
```java

class Solution {
    public int canCompleteCircuit0(int[] gas, int[] cost) {
        // Runtime: 157 ms, faster than 5.13%
        // Memory Usage: 37.8 MB, less than 52.94%
        int n = gas.length;
        int sum = 0;
        for(int i = 0; i<n; i++) {
            gas[i] -= cost[i];
            sum += gas[i];
        }
        if(sum<0) return -1;
        for(int i = 0; i<n; i++) {
            if(helper(gas, i, i, 0, n))
                return i;
        }
        return -1;
    }
    private boolean helper(int[] gas, int start, int index, int remain, int len){
        remain += gas[index];
        if(remain<0) return false;
        index = (index+1)%len;
        if(index==start) return true;
        return helper(gas, start, index, remain, len);
    }
    
    public int canCompleteCircuit(int[] gas, int[] cost) {
        // 一旦遇到第一个无法到达的点 i，直接更换起始点为 i+1。
        // 中间的[1,i-1]一定不是起始点
        // https://leetcode-cn.com/problems/gas-station/solution/java-1ms-xiang-xi-shuo-ming-qi-shi-dian-xuan-qu-gu/
        int start = 0, sum = 0, sumFromStart = 0;
        for(int i=0;i<gas.length; i++) {
            sum += gas[i]-cost[i];
            sumFromStart += gas[i]-cost[i];
            if(sumFromStart<0) {
                start = i+1;
                sumFromStart = 0;
            }
        }
        return sum>=0?start:-1;
    }
}
```
