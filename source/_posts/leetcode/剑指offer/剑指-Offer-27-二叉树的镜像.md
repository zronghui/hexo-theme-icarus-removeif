---
title: 剑指 Offer 27 二叉树的镜像
toc: true
recommend: 1
uniqueId: '2020-06-30 13:55:14/"剑指 Offer 27 二叉树的镜像".html'
date: 2020-06-30 21:55:14
thumbnail:
categories:
- leetcode
- 剑指offer
tags:
keywords:
---

[TOC]

<!--more-->

[剑指 Offer 27. 二叉树的镜像 - 力扣（LeetCode）](https://leetcode-cn.com/problems/er-cha-shu-de-jing-xiang-lcof/submissions/)

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:
    def mirrorTree(self, root: TreeNode) -> TreeNode:
        if not root:
            return root
        t = self.mirrorTree(root.left)
        root.left = self.mirrorTree(root.right)
        root.right = t
        return root
      

class Solution:
    def invertTree(self, root: TreeNode) -> TreeNode:
        stack = [root]
        while stack:
            node = stack.pop()
            if not node:
                continue
            t = node.left
            node.left = node.right
            node.right = t
            stack.append(node.left)
            stack.append(node.right)
        return root
```





```java
/*
 * @lc app=leetcode id=226 lang=java
 *
 * [226] Invert Binary Tree
 */
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
    // 递归 简单些
    public TreeNode invertTree1(TreeNode root) {
        if (root==null) {
            return null;
        }
        TreeNode left = root.left;
        root.left = root.right;
        root.right = left;
        invertTree(root.left);
        invertTree(root.right);
        return root;
    }
    // 用stack解决，可应对树层数过多，无法递归的情形
    public TreeNode invertTree(TreeNode root) {
        if (root==null) {
            return null;
        }
        Stack<TreeNode> stack = new Stack();
        stack.push(root);
        while (!stack.isEmpty()) {
            TreeNode top = stack.pop();
            if (top!=null) {
                stack.push(top.left);
                stack.push(top.right);

                TreeNode left = top.left;
                top.left = top.right;
                top.right = left;
            }
        }
        return root;
    }
}

```

