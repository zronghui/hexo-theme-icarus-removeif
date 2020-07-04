---
title: 剑指 Offer 18 删除链表的节点
toc: true
recommend: 1
uniqueId: '2020-06-30 13:55:00/"剑指 Offer 18 删除链表的节点".html'
date: 2020-06-30 21:55:00
thumbnail:
categories:
- leetcode
- 剑指offer
tags:
keywords:
---

[TOC]

<!--more-->

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None
class Solution:
    def deleteNode(self, head: ListNode, val: int) -> ListNode:
        dummy = ListNode(0)
        dummy.next = head
        cur = head
        pre = dummy
        while cur:
            if cur.val==val:
                pre.next = cur.next
                break
            pre = cur
            cur = cur.next
        return dummy.next
```

