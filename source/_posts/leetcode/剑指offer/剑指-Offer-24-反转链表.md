---
title: 剑指 Offer 24 反转链表
toc: true
recommend: 1
uniqueId: '2020-06-30 13:55:09/"剑指 Offer 24 反转链表".html'
date: 2020-06-30 21:55:09
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
    def reverseList(self, head: ListNode) -> ListNode:
        # x <- cur next-> x
        cur = None
        next = head
        while next:
            t = next.next
            next.next = cur
            cur = next
            next = t
        return cur
```



```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:
    def reverseList(self, head: ListNode) -> ListNode:
        # x <- cur next-> x
        if not head:
            return head
        cur = head
        next = head.next
        cur.next = None
        while next:
            t = next.next
            next.next = cur
            cur = next
            next = t
        return cur
```

