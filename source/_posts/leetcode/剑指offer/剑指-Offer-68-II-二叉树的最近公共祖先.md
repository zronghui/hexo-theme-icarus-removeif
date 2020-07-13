---
title: 剑指 Offer 68 - II 二叉树的最近公共祖先
toc: true
recommend: 1
uniqueId: '2020-06-30 13:56:49/"剑指 Offer 68 - II 二叉树的最近公共祖先".html'
date: 2020-06-30 21:56:49
thumbnail:
categories:
- leetcode
- 剑指offer
tags:
keywords:
---

[TOC]

<!--more-->

[面试题68 - II. 二叉树的最近公共祖先（后序遍历 DFS ，清晰图解） - 二叉树的最近公共祖先 - 力扣（LeetCode）](https://leetcode-cn.com/problems/er-cha-shu-de-zui-jin-gong-gong-zu-xian-lcof/solution/mian-shi-ti-68-ii-er-cha-shu-de-zui-jin-gong-gon-7/)

把我绕晕了

```python
class Solution:
    def lowestCommonAncestor(self, root, p, q):
        if root in [None, p, q]: return root
        left = self.lowestCommonAncestor(root.left, p, q)
        right = self.lowestCommonAncestor(root.right, p, q)
        if not left: return right
        if not right: return left
        return root

```



```java
class Solution {
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        //找 P 或 Q, 或者没找到
        if(root==null || root==q || root==p) return root;
        //左边查询结果
        TreeNode left = lowestCommonAncestor(root.left, p, q);
        //右边查询结果
        TreeNode right = lowestCommonAncestor(root.right, p, q);
        //因为肯定有这两个节点，所以如果一边是空，另一边查到了 p 或 q 任意一个，那个就是目标节点
        //如果两边都查到了，肯定是一个 q,一个p ,所以直接返回顶部的就行。
        //两边都没查到的话 就不存在这种情况了
        if(left==null) return right;
        else if(right==null) return left;
        return root;
    }
}
```

