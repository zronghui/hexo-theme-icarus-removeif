---
thumbnail:
title: leetcode 1028. Recover a Tree From Preorder Traversal
date: 2020-06-18 10:27:40
categories:
- leetcode
- leetcode-1**
toc: true
tags:
- Tree
- Depth-first Search
recommend: 1
keywords:
uniqueId: '2020-06-18 10:27:40/"leetcode 1028. Recover a Tree From Preorder Traversal".html'
---

<a href="https://leetcode.com/problems/recover-a-tree-from-preorder-traversal/">leetcode</a>
<a href="https://www.jiuzhang.com/solution/recover-a-tree-from-preorder-traversal/">九章</a>
## 题目描述
We run a preorder depth first search on the `root` of a binary tree.

At each node in this traversal, we output `D` dashes (where `D` is the _depth_
of this node), then we output the value of this node.   _(If the depth of a
node is`D`, the depth of its immediate child is `D+1`.  The depth of the root
node is `0`.)_

If a node has only one child, that child is guaranteed to be the left child.

Given the output `S` of this traversal, recover the tree and return its
`root`.



**Example 1:**

**![](https://assets.leetcode.com/uploads/2019/04/08/recover-a-tree-from-
preorder-traversal.png)**
        
    Input: "1-2--3--4-5--6--7"
    Output: [1,2,5,3,4,6,7]


**Example 2:**

**![](https://assets.leetcode.com/uploads/2019/04/11/screen-
shot-2019-04-10-at-114101-pm.png)**
        
    Input: "1-2--3---4-5--6---7"
    Output: [1,2,5,3,null,6,null,4,null,7]



**Example 3:**

![](https://assets.leetcode.com/uploads/2019/04/11/screen-
shot-2019-04-10-at-114955-pm.png)
        
    Input: "1-401--349---90--88"
    Output: [1,401,null,349,88,90]




**Note:**

  * The number of nodes in the original tree is between `1` and `1000`.
  * Each node will have a value between `1` and `10^9`.


**Tags:** Tree, Depth-first Search

**Difficulty:** Hard

## 答案
<!--more-->
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:
    def recoverFromPreorder(self, s: str) -> TreeNode:
        nums = [i for i in s.split('-') if i!='']
        deps = []

        def getDepths(s, deps):
            t = 0
            firstnum = True
            for i in s:
                if i=='-':
                    t += 1
                    firstnum = True
                elif firstnum:
                    deps.append(t)
                    t = 0
                    firstnum = False
        
        getDepths(s, deps)
        data = list(zip(nums, deps))
        print(nums, deps, data)

        def buildTree(data, dep):
            if not data:
                return None
            if data[0][1]>dep:
                val, _ = data.pop(0)
                root = TreeNode(val)
                root.left = buildTree(data, dep+1)
                root.right = buildTree(data, dep+1)
                return root
            else:
                return None


        return buildTree(data, -1)

```
