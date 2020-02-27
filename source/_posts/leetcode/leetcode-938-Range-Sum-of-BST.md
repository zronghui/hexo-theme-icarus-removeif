---
title: leetcode 938. Range Sum of BST
date: 2019-11-22 17:07:36
categories:
- leetcode
toc: true
tags:
- Tree
- Recursion
---
### 难度：Easy

<a href="https://leetcode.com/problems/range-sum-of-bst/">leetcode</a>
<a href="https://www.jiuzhang.com/solution/range-sum-of-bst/">九章</a>
## 题目描述
Given the `root` node of a binary search tree, return the sum of values of all
nodes with value between `L` and `R` (inclusive).

The binary search tree is guaranteed to have unique values.



**Example 1:**
        
    Input: root = [10,5,15,3,7,null,18], L = 7, R = 15
    Output: 32
    

**Example 2:**
        
    Input: root = [10,5,15,3,7,13,18,1,null,6], L = 6, R = 10
    Output: 23
    



**Note:**

  1. The number of nodes in the tree is at most `10000`.
  2. The final answer is guaranteed to be less than `2^31`.


**Tags:** Tree, Recursion

**Difficulty:** Easy
## 答案
<!--more-->
```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
class Solution {
    int sum;
    public int rangeSumBST(TreeNode root, int L, int R) {
        sum = 0;
        helper(root, L, R);
        return sum;
    }
    private void helper(TreeNode root, int l, int r){
        // 如果 node.val 小于等于 L，那么只需要继续搜索它的右子树；如果 node.val 大于等于 R，那么只需要继续搜索它的左子树；如果 node.val 在区间 (L, R) 中，则需要搜索它的所有子树
        if(root==null) return;
        if(root.val>=l && root.val<=r){
            sum += root.val;
            helper(root.left, l, r);
            helper(root.right, l ,r);
        }else if(root.val<l){
            helper(root.right, l ,r);
        }else{
            helper(root.left, l, r);
        }        
    }
}
```
