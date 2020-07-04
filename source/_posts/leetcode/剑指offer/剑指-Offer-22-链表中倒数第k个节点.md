---
title: 剑指 Offer 22 链表中倒数第k个节点
toc: true
recommend: 1
uniqueId: '2020-06-30 13:55:07/"剑指 Offer 22 链表中倒数第k个节点".html'
date: 2020-06-30 21:55:07
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
    def getKthFromEnd(self, head: ListNode, k: int) -> ListNode:
        slow = fast = head
        for _ in range(k):
            fast = fast.next
        while fast:
            fast = fast.next
            slow = slow.next
        return slow
```

