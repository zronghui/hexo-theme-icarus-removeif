---
title: leetcode 78. Subsets
date: 2019-11-26 09:26:14
categories:
- leetcode
- leetcode-7**
toc: true
tags:
- Array
- Backtracking
- Bit Manipulation
---
### 难度：Middle

<a href="https://leetcode.com/problems/subsets/">leetcode</a>
<a href="https://www.jiuzhang.com/solution/subsets/">九章</a>
## 题目描述
Given a set of **distinct** integers, _nums_ , return all possible subsets
(the power set).

**Note:** The solution set must not contain duplicate subsets.

**Example:**
        
    Input: nums = [1,2,3]
    Output:
    [
      [3],
      [1],
      [2],
      [1,2,3],
      [1,3],
      [2,3],
      [1,2],
      []
    ]


**Tags:** Array, Backtracking, Bit Manipulation

**Difficulty:** Medium
## 答案
<!--more-->
```java
// 开始时间-写完-调完错
// 31-43-46
class Solution {
    public List<List<Integer>> subsets(int[] nums) {
        return helper(nums, nums.length);
    }
    // 1 2 3
    // [[]] len=0
    // [[], [1]] len=1
    // [[], [1], [2], [1, 2]] len=2
    // [[], [1], [2], [1, 2], [3], [1, 3], [2, 3], [1, 2, 3]] len=3
    private List<List<Integer>> helper(int[] nums, int len){
        List<List<Integer>> res = new ArrayList<>();
        if(nums==null || len==0) {
            res.add(new ArrayList<>());
            return res;
        }
        int num = nums[len-1];
        // 在len-1的res中的每一个list加上num
        for(List<Integer> l: helper(nums, len-1)){
            res.add(l);
            List<Integer> temp = new ArrayList<>(l);
            temp.add(num);
            res.add(temp);
        }
        return res;
    }
}
```
