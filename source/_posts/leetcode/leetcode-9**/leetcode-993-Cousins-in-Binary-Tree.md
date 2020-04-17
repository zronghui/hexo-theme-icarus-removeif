---
title: leetcode 993. Cousins in Binary Tree
date: 2019-12-07 20:05:40
categories:
- leetcode
- leetcode-9**
toc: true
tags:
- Tree
- Breadth-first Search
---
### 难度：Easy

<a href="https://leetcode.com/problems/cousins-in-binary-tree/">leetcode</a>
<a href="https://www.jiuzhang.com/solution/cousins-in-binary-tree/">九章</a>
## 题目描述
In a binary tree, the root node is at depth `0`, and children of each depth
`k` node are at depth `k+1`.

Two nodes of a binary tree are _cousins_ if they have the same depth, but have
**different parents**.

We are given the `root` of a binary tree with unique values, and the values
`x` and `y` of two different nodes in the tree.

Return `true` if and only if the nodes corresponding to the values `x` and `y`
are cousins.



**Example 1:  
![](https://assets.leetcode.com/uploads/2019/02/12/q1248-01.png)**
        
    Input: root = [1,2,3,4], x = 4, y = 3
    Output: false
    

**Example 2:  
![](https://assets.leetcode.com/uploads/2019/02/12/q1248-02.png)**
        
    Input: root = [1,2,3,null,4,null,5], x = 5, y = 4
    Output: true
    

**Example 3:**

**![](https://assets.leetcode.com/uploads/2019/02/13/q1248-03.png)**
        
    Input: root = [1,2,3,null,4], x = 2, y = 3
    Output: false



**Note:**

  1. The number of nodes in the tree will be between `2` and `100`.
  2. Each node has a unique integer value from `1` to `100`.




**Tags:** Tree, Breadth-first Search

**Difficulty:** Easy
## 答案
<!--more-->
```java

class Solution {
    public boolean isCousins(TreeNode root, int x, int y) {
        if(root==null) return false;
        Map<Integer, Integer> level = new HashMap<>();
        Map<Integer, TreeNode> parent = new HashMap<>();
        helper(root, level, parent, 0);
        return level.containsKey(x) && level.containsKey(y) 
            && level.get(x)==level.get(y) && parent.get(x)!=parent.get(y);
    }
    private void helper(TreeNode root, Map<Integer, Integer> level, Map<Integer, TreeNode> parent, int curLevel){
        curLevel++;
        if(root.left!=null){
            level.put(root.left.val, curLevel);
            parent.put(root.left.val, root);
            helper(root.left, level, parent, curLevel);
        }
        if(root.right!=null){
            level.put(root.right.val, curLevel);
            parent.put(root.right.val, root);
            helper(root.right, level, parent, curLevel);
        }
        
    }
}
```
