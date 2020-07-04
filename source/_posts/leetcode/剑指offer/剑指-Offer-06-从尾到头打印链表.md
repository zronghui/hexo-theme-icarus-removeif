---
title: 剑指 Offer 06 从尾到头打印链表
toc: true
recommend: 1
uniqueId: '2020-06-30 13:54:37/"剑指 Offer 06 从尾到头打印链表".html'
date: 2020-06-30 21:54:37
thumbnail:
categories:
- leetcode
- 剑指offer
tags:
keywords:
---

[TOC]

<!--more-->

[剑指 Offer 06. 从尾到头打印链表 - 力扣（LeetCode）](https://leetcode-cn.com/problems/cong-wei-dao-tou-da-yin-lian-biao-lcof/)

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:
    def reversePrint(self, head: ListNode) -> List[int]:
        l = []
        cur = head
        while cur:
            l.append(cur.val)
            cur = cur.next
        return list(reversed(l))
```



[面试题06. 从尾到头打印链表（递归法、辅助栈法，清晰图解） - 从尾到头打印链表 - 力扣（LeetCode）](https://leetcode-cn.com/problems/cong-wei-dao-tou-da-yin-lian-biao-lcof/solution/mian-shi-ti-06-cong-wei-dao-tou-da-yin-lian-biao-d/)

```python
class Solution:
    def reversePrint(self, head: ListNode) -> List[int]:
        return self.reversePrint(head.next) + [head.val] if head else []
      
```

