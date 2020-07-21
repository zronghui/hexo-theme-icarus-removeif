---
thumbnail:
title: leetcode 95. Unique Binary Search Trees II
date: 2020-07-21 10:23:38
categories:
- leetcode
- leetcode-9**
toc: true
tags:
- Dynamic Programming
- Tree
recommend: 1
keywords:
uniqueId: '2020-07-21 10:23:38/"leetcode 95. Unique Binary Search Trees II".html'
---

<a href="https://leetcode.com/problems/unique-binary-search-trees-ii/">leetcode</a>
<a href="https://www.jiuzhang.com/solution/unique-binary-search-trees-ii/">九章</a>
## 题目描述
Given an integer _n_ , generate all structurally unique **BST 's** (binary
search trees) that store values 1 ...  _n_.

**Example:**
        
    Input: 3
    Output:
    [
      [1,null,3,2],
      [3,2,null,1],
      [3,1,null,null,2],
      [2,1,3],
      [1,null,2,null,3]
    ]
    Explanation:
    The above output corresponds to the 5 unique BST's shown below:
    
       1         3     3      2      1
        \       /     /      / \      \
         3     2     1      1   3      2
        /     /       \                 \
       2     1         2                 3



**Tags:** Dynamic Programming, Tree

**Difficulty:** Medium

## 答案
<!--more-->
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def generateTrees(self, n: int) -> List[TreeNode]:
        if not n: return []

        @functools.lru_cache(maxsize=None)
        def helper(start, end):
            res = []
            # 空树，不能直接返回[] 因为另外一边不是空
            if end<start: return [None]
            for i in range(start, end+1):
                for left in helper(start, i-1):
                    for right in helper(i+1, end):
                        node = TreeNode(i)
                        node.left = left
                        node.right = right
                        res.append(node)
            return res
        
        return helper(1, n)

```
