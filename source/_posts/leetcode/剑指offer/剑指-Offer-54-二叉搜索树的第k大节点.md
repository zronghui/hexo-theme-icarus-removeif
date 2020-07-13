---
title: 剑指 Offer 54 二叉搜索树的第k大节点
toc: true
recommend: 1
uniqueId: '2020-06-30 13:56:13/"剑指 Offer 54 二叉搜索树的第k大节点".html'
date: 2020-06-30 21:56:13
thumbnail:
categories:
- leetcode
- 剑指offer
tags:
keywords:
---

[TOC]

<!--more-->

[面试题54. 二叉搜索树的第 k 大节点（中序遍历 + 提前返回，清晰图解） - 二叉搜索树的第k大节点 - 力扣（LeetCode）](https://leetcode-cn.com/problems/er-cha-sou-suo-shu-de-di-kda-jie-dian-lcof/solution/mian-shi-ti-54-er-cha-sou-suo-shu-de-di-k-da-jie-d/)

关键：使用 self.k 以及 self.res

```python
class Solution:
    def kthLargest(self, root: TreeNode, k: int) -> int:
        # "中序遍历"(右 中 左)，取第 k 个元素
        def helper(node):
            if not node: return
            helper(node.right)
            self.k -= 1
            if not self.k: 
                self.res = node.val
                return
            helper(node.left)
        self.k = k
        helper(root)
        return self.res

```

