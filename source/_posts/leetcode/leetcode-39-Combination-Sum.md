---
title: leetcode 39. Combination Sum
date: 2019-11-24 14:57:02
categories:
- leetcode
toc: true
tags:
- Array
- Backtracking
---
### 难度：Middle

<a href="https://leetcode.com/problems/combination-sum/">leetcode</a>
<a href="https://www.jiuzhang.com/solution/combination-sum/">九章</a>
## 题目描述
Given a **set** of candidate numbers (`candidates`) **(without duplicates)**
and a target number (`target`), find all unique combinations in `candidates`
where the candidate numbers sums to `target`.

The **same** repeated number may be chosen from `candidates` unlimited number
of times.

**Note:**

  * All numbers (including `target`) will be positive integers.
  * The solution set must not contain duplicate combinations.

**Example 1:**
        
    Input: candidates = [2,3,6,7], target = 7,
    **A solution set is:**
    [
      [7],
      [2,2,3]
    ]
    

**Example 2:**
        
    Input: candidates = [2,3,5], target = 8,
    **A solution set is:**
    [
      [2,2,2,2],
      [2,3,3],
      [3,5]
    ]
    


**Tags:** Array, Backtracking

**Difficulty:** Medium
## 答案
<!--more-->
```java
class Solution {
    public List<List<Integer>> combinationSum(int[] nums, int target) {
        List<List<Integer>> res = new ArrayList<>();
        if(nums==null || nums.length==0) return res;
        Arrays.sort(nums);
        helper(nums, target, res, new ArrayList<Integer>(), 0);
        return res;
    }
    private void helper(int[] nums, int target, List<List<Integer>> res, List<Integer> cur, int start){
        if(target==0) {
            res.add(new ArrayList<>(cur));
        }else if(target>0) {
            // start 去重, 且一个数字可以使用多次，故不是i+1
            for(int i=start; i<nums.length; i++){
                cur.add(nums[i]);
                helper(nums, target-nums[i], res, cur, i);
                cur.remove(cur.size()-1);
            }
        }
    }
}
```
