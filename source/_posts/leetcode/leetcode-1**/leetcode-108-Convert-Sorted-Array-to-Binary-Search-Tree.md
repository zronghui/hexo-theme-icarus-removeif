---
title: leetcode 108. Convert Sorted Array to Binary Search Tree
date: 2019-11-23 14:58:30
categories:
- leetcode
- leetcode-1**
toc: true
tags:
- Tree
- Depth-first Search
---
### 难度：Easy

<a href="https://leetcode.com/problems/convert-sorted-array-to-binary-search-tree/">leetcode</a>
<a href="https://www.jiuzhang.com/solution/convert-sorted-array-to-binary-search-tree/">九章</a>
## 题目描述
Given an array where elements are sorted in ascending order, convert it to a
height balanced BST.

For this problem, a height-balanced binary tree is defined as a binary tree in
which the depth of the two subtrees of _every_ node never differ by more than
1.

**Example:**
        
    Given the sorted array: [-10,-3,0,5,9],
    
    One possible answer is: [0,-3,9,-10,null,5], which represents the following height balanced BST:
    
          0
         / \
       -3   9
       /   /
     -10  5
    


**Tags:** Tree, Depth-first Search

**Difficulty:** Easy
## 答案
<!--more-->
```java
class Solution {
    public TreeNode sortedArrayToBST(int[] nums) {
        if(nums==null || nums.length<1) return null;
        return helper(nums, 0, nums.length-1);
    }
    
    private TreeNode helper(int[] nums, int start, int end){
        if(start>end) return null;
        TreeNode root = new TreeNode(nums[start+(end-start)/2]);
        root.left = helper(nums, start, start+(end-start)/2-1);
        root.right = helper(nums, start+(end-start)/2+1, end);
        return root;
    }
}
```
