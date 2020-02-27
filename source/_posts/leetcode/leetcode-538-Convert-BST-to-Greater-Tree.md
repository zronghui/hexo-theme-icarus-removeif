---
title: leetcode 538. Convert BST to Greater Tree
date: 2019-12-07 19:47:55
categories:
- leetcode
toc: true
tags:
- Tree
---
### 难度：Easy

<a href="https://leetcode.com/problems/convert-bst-to-greater-tree/">leetcode</a>
<a href="https://www.jiuzhang.com/solution/convert-bst-to-greater-tree/">九章</a>
## 题目描述
Given a Binary Search Tree (BST), convert it to a Greater Tree such that every
key of the original BST is changed to the original key plus sum of all keys
greater than the original key in BST.

**Example:**
        
    Input: The root of a Binary Search Tree like this:
                  5
                /   \
               2     13
    
    Output: The root of a Greater Tree like this:
                 18
                /   \
              20     13
    


**Tags:** Tree

**Difficulty:** Easy
## 答案
<!--more-->
```java

// https://leetcode-cn.com/problems/convert-bst-to-greater-tree/solution/ba-er-cha-sou-suo-shu-zhuan-huan-wei-lei-jia-shu-3/
class Solution {
    int sum = 0;
    public TreeNode convertBST(TreeNode root) {
        if(root==null) return root;
        convertBST(root.right);
        sum += root.val;
        root.val = sum;
        convertBST(root.left);
        return root;
    }
}
```
