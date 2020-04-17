---
title: leetcode 61. Rotate List
date: 2019-11-24 15:35:14
categories:
- leetcode
- leetcode-6**
toc: true
tags:
- Linked List
- Two Pointers
---
### 难度：Middle

<a href="https://leetcode.com/problems/rotate-list/">leetcode</a>
<a href="https://www.jiuzhang.com/solution/rotate-list/">九章</a>
## 题目描述
Given a linked list, rotate the list to the right by _k_ places, where _k_ is
non-negative.

**Example 1:**
        
    Input: 1->2->3->4->5->NULL, k = 2
    Output: 4->5->1->2->3->NULL
    Explanation:
    rotate 1 steps to the right: 5->1->2->3->4->NULL
    rotate 2 steps to the right: 4->5->1->2->3->NULL
    

**Example 2:**
        
    Input: 0->1->2->NULL, k = 4
    Output: 2->0->1->NULL
    Explanation:
    rotate 1 steps to the right: 2->0->1->NULL
    rotate 2 steps to the right: 1->2->0->NULL
    rotate 3 steps to the right: 0->1->2->NULL
    rotate 4 steps to the right: 2->0->1->NULL


**Tags:** Linked List, Two Pointers

**Difficulty:** Medium
## 答案
<!--more-->
```java
class Solution {
    public ListNode rotateRight(ListNode head, int k) {
        if(head==null) return null;
        // 遍历一次，确定链表长度和链尾
        int count = 1;
        ListNode cur = head, end;
        while(cur.next!=null) {
            count ++;
            cur = cur.next;
        }
        end = cur;
        cur = head;
        System.out.println(count);
        k = k%count;
        // 找到第 count-k 个节点（是新链表的链尾）
        int index = 1;
        while(index!=(count-k)){
            cur = cur.next;
            index++;
        }
        end.next = head;
        head = cur.next;
        cur.next = null;
        return head;
    }
}
```
