---
title: leetcode 113. Path Sum II
date: 2019-11-24 10:13:58
categories:
- leetcode
- leetcode-1**
toc: true
tags:
- Tree
- Depth-first Search
---
### 难度：Middle

<a href="https://leetcode.com/problems/path-sum-ii/">leetcode</a>
<a href="https://www.jiuzhang.com/solution/path-sum-ii/">九章</a>
## 题目描述
Given a binary tree and a sum, find all root-to-leaf paths where each path's
sum equals the given sum.

**Note:**  A leaf is a node with no children.

**Example:**

Given the below binary tree and `sum = 22`,
        
          **5**
         **/ \**
        **4   8**
       **/**   / **\**
      **11**  13  **4**
     /  **\**    **/** \
    7    **2**  **5**   1


Return:
        
    [
       [5,4,11,2],
       [5,8,4,5]
    ]



**Tags:** Tree, Depth-first Search

**Difficulty:** Medium
## 答案
<!--more-->
```java
class Solution {
    public List<List<Integer>> pathSum(TreeNode root, int sum) {
        List<List<Integer>> res = new ArrayList<>();
        if(root==null) return res;
        helper(root, sum, res, null);
        return res;
    }
    
    private void helper(TreeNode root, int sum, List<List<Integer>> res, List<Integer> curr){
        List<Integer> cur;
        if(curr==null) cur = new ArrayList<Integer>();
        else cur = new ArrayList<Integer>(curr);
        cur.add(root.val);
        if(root.left==null && root.right==null && root.val==sum) res.add(cur);
        sum -= root.val;
        if(root.left!=null) helper(root.left, sum, res, cur);
        if(root.right!=null) helper(root.right, sum, res, cur);
    }
}
```



```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:
    def pathSum(self, root: TreeNode, target: int) -> List[List[int]]:
        res, l = [], []
        if not root:
            return []
        def helper(root, cursum):
            cursum += root.val
            l.append(root.val)
            if not root.left and not root.right and cursum==target:
                res.append(l.copy())
            if root.left: helper(root.left, cursum)
            if root.right: helper(root.right, cursum)
            l.pop()
        helper(root, 0)
        return res

```

