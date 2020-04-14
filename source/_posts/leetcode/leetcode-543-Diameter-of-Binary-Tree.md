---
thumbnail:
title: leetcode 543. Diameter of Binary Tree
date: 2020-04-14 13:30:49
categories:
- leetcode
toc: true
tags:
- Tree
recommend: 1
keywords:
uniqueId: '2020-04-14 13:30:49/"leetcode 543. Diameter of Binary Tree".html'
---

<a href="https://leetcode.com/problems/diameter-of-binary-tree/">leetcode</a>
<a href="https://www.jiuzhang.com/solution/diameter-of-binary-tree/">九章</a>
## 题目描述
Given a binary tree, you need to compute the length of the diameter of the
tree. The diameter of a binary tree is the length of the **longest** path
between any two nodes in a tree. This path may or may not pass through the
root.

**Example:**  
Given a binary tree  
        
              1
             / \
            2   3
           / \     
          4   5    


Return **3** , which is the length of the path [4,2,1,3] or [5,2,1,3].

**Note:** The length of path between two nodes is represented by the number of
edges between them.


**Tags:** Tree

**Difficulty:** Easy

## 答案

思路：

任意一条路径均可以被看作由某个节点为起点，从其左儿子和右儿子向下遍历的路径拼接得到。

心得：

在递归函数的开头判断是否为 None，而不用分别判断 左 右和 root

<!--more-->
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:
    dia = 0
    
    def depth(self, root):
        if not root:
            return 0
        left = self.depth(root.left)
        right = self.depth(root.right)
        
        depth = max(left, right)+1
        self.dia = max(self.dia, left+right)
        return depth
        
    def diameterOfBinaryTree(self, root: TreeNode) -> int:
        self.depth(root)
        return self.dia
```
