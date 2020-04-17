---
title: leetcode 46. Permutations
date: 2019-11-25 09:59:58
categories:
- leetcode
- leetcode-4**
toc: true
tags:
- Backtracking
---
### 难度：Middle

<a href="https://leetcode.com/problems/permutations/">leetcode</a>
<a href="https://www.jiuzhang.com/solution/permutations/">九章</a>
## 题目描述
Given a collection of **distinct** integers, return all possible permutations.

**Example:**
        
    Input: [1,2,3]
    Output:
    [
      [1,2,3],
      [1,3,2],
      [2,1,3],
      [2,3,1],
      [3,1,2],
      [3,2,1]
    ]
    


**Tags:** Backtracking

**Difficulty:** Medium
## 答案
<!--more-->
```java
class Solution {
    public List<List<Integer>> permute(int[] nums) {
        return helper(nums, nums.length);
    }
    
    // 前len个数的组合
    private List<List<Integer>> helper(int[] nums, int len) {
        List<List<Integer>> res = new ArrayList<>();
        if(nums==null || nums.length==0) return res;
        if(len==1) {
            res.add(new ArrayList<>(Arrays.asList(nums[0])));
            return res;
        }
        // 1 2    2 1
        // 插入3
        // 3 1 2   1 3 2   1 2 3
        // 3 2 1   2 3 1   2 1 3 
        for(List<Integer> l: helper(nums, len-1)){
            for(int i=0; i<=len-1; i++){
                List<Integer> temp = new ArrayList<>(l);
                temp.add(i, nums[len-1]);
                res.add(temp);
            }
        }
        return res;        
    }
}
```
