---
title: leetcode 111. Minimum Depth of Binary Tree
date: 2019-11-23 15:33:12
categories:
- leetcode
- leetcode-1**
toc: true
tags:
- Tree
- Depth-first Search
- Breadth-first Search
---
### 难度：Easy

<a href="https://leetcode.com/problems/minimum-depth-of-binary-tree/">leetcode</a>
<a href="https://www.jiuzhang.com/solution/minimum-depth-of-binary-tree/">九章</a>
## 题目描述
Given a binary tree, find its minimum depth.

The minimum depth is the number of nodes along the shortest path from the root
node down to the nearest leaf node.

**Note:**  A leaf is a node with no children.

**Example:**

Given binary tree `[3,9,20,null,null,15,7]`,
        
        3
       / \
      9  20
        /  \
       15   7

return its minimum depth = 2.


**Tags:** Tree, Depth-first Search, Breadth-first Search

**Difficulty:** Easy
## 答案
<!--more-->
```java
class Solution {
    public int minDepth(TreeNode root) {
        if(root==null) return 0;
        int left = minDepth(root.left);
        int right = minDepth(root.right);
        // left 或 right 为0 表示 有1边没有节点，只能选另一边，然后加一
        // 就是 left+right+1
        return (left==0 || right==0)?left+right+1:Math.min(left, right)+1;
    }
}
```
