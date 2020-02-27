---
title: leetcode 98. Validate Binary Search Tree
date: 2019-11-22 13:31:22
categories:
- leetcode
toc: true
tags:
- Tree
- Depth-first Search
---
### 难度：Middle

<a href="https://leetcode.com/problems/validate-binary-search-tree/">leetcode</a>
<a href="https://www.jiuzhang.com/solution/validate-binary-search-tree/">九章</a>
## 题目描述
Given a binary tree, determine if it is a valid binary search tree (BST).

Assume a BST is defined as follows:

  * The left subtree of a node contains only nodes with keys **less than** the node's key.
  * The right subtree of a node contains only nodes with keys **greater than** the node's key.
  * Both the left and right subtrees must also be binary search trees.



**Example 1:**
        
        2
       / \
      1   3
    
    Input:  [2,1,3]
    Output: true
    

**Example 2:**
        
        5
       / \
      1   4
         / \
        3   6
    
    Input: [5,1,4,null,null,3,6]
    Output: false
    Explanation: The root node's value is 5 but its right child's value is 4.
    


**Tags:** Tree, Depth-first Search

**Difficulty:** Medium
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
    public boolean isValidBST(TreeNode root) {
        // 要求所有右子树的元素大于root
        // if(root==null) return true;
        // else if(root.left!=null && root.left.val>=root.val)
        //     return false;
        // else if(root.right!=null && root.right.val<=root.val)
        //     return false;
        // return isValidBST(root.left) && isValidBST(root.right);
        return divConq(root, Long.MIN_VALUE, Long.MAX_VALUE);
    }
    private boolean divConq(TreeNode root, long min, long max){
        // min~max 是root应该处于的范围
        if(root==null) return true;
        else if(root.val<=min || root.val>=max) return false;
        else {
            // root.left < root  && root.right > root
            return divConq(root.left, min, Math.min(root.val, max)) 
                && divConq(root.right, Math.max(root.val, min), max) ;
        }
    }
}
```
