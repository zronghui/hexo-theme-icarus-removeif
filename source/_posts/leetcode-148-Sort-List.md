---
thumbnail:
title: leetcode 148. Sort List
date: 2020-08-01 21:26:55
categories:
toc: true
tags:
- Linked List
- Sort
recommend: 1
keywords:
uniqueId: '2020-08-01 21:26:55/"leetcode 148. Sort List".html'
---

<a href="https://leetcode.com/problems/sort-list/">leetcode</a>
<a href="https://www.jiuzhang.com/solution/sort-list/">九章</a>
## 题目描述
Sort a linked list in _O_ ( _n_ log _n_ ) time using constant space
complexity.

**Example 1:**
        
    Input: 4->2->1->3
    Output: 1->2->3->4


**Example 2:**
        
    Input: -1->5->3->4->0
    Output: -1->0->3->4->5


**Tags:** Linked List, Sort

**Difficulty:** Medium

## 答案
<!--more-->

## 快排

### 超时写法

根据[86. 分隔链表 - 力扣（LeetCode）](https://leetcode-cn.com/problems/partition-list/)的 partition 写的

86题代码：

```python
class Solution:
    def partition(self, head: ListNode, x: int) -> ListNode:
        dummy1, dummy2 = ListNode(0), ListNode(0)
        # dummy1 存 小于 x 的值; dummy >=
        cur1, cur2 = dummy1, dummy2
        while head:
            if head.val<x:
                cur1.next = head
                cur1 = cur1.next
            else:
                cur2.next = head
                cur2 = cur2.next
            head = head.next
        cur2.next = None
        cur1.next = dummy2.next
        return dummy1.next
```



一开始写的死板解法，但是遇到一个 testcase 全是  1，2，3 的链表，超时了

```python
class Solution:
    def sortList(self, head: ListNode) -> ListNode:
        # 快速排序
        # return head tail
        def quickSort(head):
            # return head1,tail1,head2,tail2
            def partition(head, x):
                dummy1, dummy2 = ListNode(0), ListNode(0)
                # cur 指向那条链最后一个非 None 节点
                cur1, cur2 = dummy1, dummy2
                while head:
                    if head.val<x:
                        cur1.next = head
                        cur1 = cur1.next
                    else:
                        cur2.next = head
                        cur2 = cur2.next
                    head = head.next
                cur1.next = cur2.next = None
                # head1, head2 可能为 None, tail1, tail2 一定非 None 
                return dummy1.next, cur1, dummy2.next, cur2
            
            tail = head
            if not head or not head.next: return head, tail
            head1, tail1, head2, tail2 = partition(head.next, head.val)
            head.next = None
            # print(head1, tail1, head2, tail2)
            if head1:
                head1, tail1 = quickSort(head1)
                tail1.next = head
            if head2:
                head2, tail2 = quickSort(head2)
                head.next = head2
            return head1 if head1 else head , tail2 if head2 else head

        return quickSort(head)[0]
```



### 优化后的快排

划分为 small equal large 3个链表，对 small large 递归, 再合并 3 个链表

```python
class Solution:
    def sortList(self, head: ListNode) -> ListNode:
        # 快速排序
        if not head: return None
        # small equal large 的缩写
        # 都指向相应链表的 head
        s = e = l = None
        target = head.val
        while head:
            nxt = head.next
            if head.val>target:
                head.next = l
                l = head
            elif head.val==target:
                head.next = e
                e = head
            else:
                head.next = s
                s = head
            head = nxt
        
        s = self.sortList(s)
        l = self.sortList(l)
        # 合并 3 个链表
        dummy = ListNode(0)
        cur = dummy # cur: 非 None 的尾节点
        # p: 下一个需要连接的节点
        for p in [s, e, l]:
            while p:
                cur.next = p
                p = p.next
                cur = cur.next
        return dummy.next


```





## 归并排序

写归并排序是主流



```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:
    def sortList(self, head: ListNode) -> ListNode:
        # 归并排序
        def split(h):
            slow = h
            fast = h.next
            while fast and fast.next:
                fast = fast.next.next
                slow = slow.next
            t = slow.next
            slow.next = None
            # print(h, t)
            return h, t

        def merge(h1, h2):
            dummy = ListNode(0)
            cur = dummy
            while h1 or h2:
                v1 = h1.val if h1 else float('inf')
                v2 = h2.val if h2 else float('inf')
                if v1<=v2:
                    cur.next = h1
                    cur = cur.next
                    h1 = h1.next
                else:
                    cur.next = h2
                    cur = cur.next
                    h2 = h2.next
            return dummy.next

        if not head or not head.next: return head
        # 把链划分 2 半，递归排序
        h1, h2 = map(self.sortList, split(head))
        # merge 2 条链
        return merge(h1, h2)

```

优化合并

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:
    def sortList(self, head: ListNode) -> ListNode:
        # 归并排序
        def split(h):
            slow = h
            fast = h.next
            while fast and fast.next:
                fast = fast.next.next
                slow = slow.next
            t = slow.next
            slow.next = None
            # print(h, t)
            return h, t

        def merge(h1, h2):
            dummy = ListNode(0)
            cur = dummy
            while h1 and h2:
                if h1.val<h2.val:
                    cur.next = h1
                    cur = cur.next
                    h1 = h1.next
                else:
                    cur.next = h2
                    cur = cur.next
                    h2 = h2.next
            if h1: cur.next = h1
            if h2: cur.next = h2
            return dummy.next

        if not head or not head.next: return head
        # 把链划分 2 半，递归排序
        h1, h2 = map(self.sortList, split(head))
        # merge 2 条链
        return merge(h1, h2)

```





## 速度对比

归并慢些

![image-20200802095356739](https://i.loli.net/2020/08/02/jsFn69BiDbMo87c.png)



















