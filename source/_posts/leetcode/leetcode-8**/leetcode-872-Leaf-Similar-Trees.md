---
title: leetcode 872. Leaf-Similar Trees
date: 2019-12-07 18:56:11
categories:
- leetcode
- leetcode-8**
toc: true
tags:
- Tree
- Depth-first Search
---
### 难度：Easy

<a href="https://leetcode.com/problems/leaf-similar-trees/">leetcode</a>
<a href="https://www.jiuzhang.com/solution/leaf-similar-trees/">九章</a>
## 题目描述
Consider all the leaves of a binary tree.  From left to right order, the
values of those leaves form a _leaf value sequence._

![](https://s3-lc-upload.s3.amazonaws.com/uploads/2018/07/16/tree.png)

For example, in the given tree above, the leaf value sequence is `(6, 7, 4, 9,
8)`.

Two binary trees are considered _leaf-similar_  if their leaf value sequence
is the same.

Return `true` if and only if the two given trees with head nodes `root1` and
`root2` are leaf-similar.



**Note:**

  * Both of the given trees will have between `1` and `100` nodes.


**Tags:** Tree, Depth-first Search

**Difficulty:** Easy
## 答案
<!--more-->
```java

// 前序遍历应该用stack
// deque remove==removeFirst  add==addLast
class Solution {
    public boolean leafSimilar(TreeNode root1, TreeNode root2) {
        List<Integer> l1 = new ArrayList<>();
        List<Integer> l2 = new ArrayList<>();
        preOrder(root1, l1);
        preOrder(root2, l2);
        return l1.equals(l2);
    }
    private void preOrder(TreeNode root, List<Integer> l){
        if(root==null) return;
        if(root.left==null && root.right==null){
            l.add(root.val);
        }
        preOrder(root.left, l);
        preOrder(root.right, l);
    }
}
```
