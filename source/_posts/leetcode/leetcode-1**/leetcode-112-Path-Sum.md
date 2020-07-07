---
title: leetcode 112. Path Sum
date: 2019-11-23 15:47:36
categories:
- leetcode
- leetcode-1**
toc: true
tags:
- Tree
- Depth-first Search
---
### 难度：Easy

<a href="https://leetcode.com/problems/path-sum/">leetcode</a>
<a href="https://www.jiuzhang.com/solution/path-sum/">九章</a>
## 题目描述
Given a binary tree and a sum, determine if the tree has a root-to-leaf path
such that adding up all the values along the path equals the given sum.

**Note:**  A leaf is a node with no children.

**Example:**

Given the below binary tree and `sum = 22`,
        
          **5**
         **/** \
        **4**   8
       **/**   / \
      **11**  13  4
     /  **\**      \
    7    **2**      1


return true, as there exist a root-to-leaf path `5->4->11->2` which sum is 22.


**Tags:** Tree, Depth-first Search

**Difficulty:** Easy
## 答案
<!--more-->

正确代码：

```java
class Solution {
    public boolean hasPathSum(TreeNode root, int sum) {
        if(root==null) return false;
        if(root.left==null && root.right==null && root.val==sum)
            return true;
        return hasPathSum(root.left, sum-root.val) || hasPathSum(root.right, sum-root.val);
    }
}
```



烂代码：

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:
    def hasPathSum(self, root: TreeNode, target: int) -> bool:
        if not root:
            return False
        def helper(root, cursum):
            cursum += root.val
            if not root.left and not root.right:
                print(cursum)
                return cursum==target
            # if else 需要用括号括起来否则不生效
            return (helper(root.left, cursum) if root.left else False) \
              or (helper(root.right, cursum) if root.right else False)
        return helper(root, 0)
```

