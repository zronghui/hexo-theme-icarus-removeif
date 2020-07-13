---
title: 剑指 Offer 52 两个链表的第一个公共节点
toc: true
recommend: 1
uniqueId: '2020-06-30 13:56:07/"剑指 Offer 52 两个链表的第一个公共节点".html'
date: 2020-06-30 21:56:07
thumbnail:
categories:
- leetcode
- 剑指offer
tags:
keywords:
---

[TOC]

<!--more-->

[Intersection of Two Linked Lists （双指针，链表拼接） - 相交链表 - 力扣（LeetCode）](https://leetcode-cn.com/problems/intersection-of-two-linked-lists/solution/intersection-of-two-linked-lists-shuang-zhi-zhen-l/)

![image-20200713084416620](https://i.loli.net/2020/07/13/x3KGLzIdBO8qYFe.png)

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:
    def getIntersectionNode(self, headA: ListNode, headB: ListNode) -> ListNode:
        pa, pb = headA, headB
        while pa!=pb:
            pa = pa.next if pa else headB
            pb = pb.next if pb else headA
        return pa

```

