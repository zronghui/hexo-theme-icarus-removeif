---
title: leetcode 82. Remove Duplicates from Sorted List II
date: 2019-11-24 15:48:21
categories:
- leetcode
toc: true
tags:
- Linked List
---
### 难度：Middle

<a href="https://leetcode.com/problems/remove-duplicates-from-sorted-list-ii/">leetcode</a>
<a href="https://www.jiuzhang.com/solution/remove-duplicates-from-sorted-list-ii/">九章</a>
## 题目描述
Given a sorted linked list, delete all nodes that have duplicate numbers,
leaving only _distinct_ numbers from the original list.

**Example 1:**
        
    Input: 1->2->3->3->4->4->5
    Output: 1->2->5
    

**Example 2:**
        
    Input: 1->1->1->2->3
    Output: 2->3
    


**Tags:** Linked List

**Difficulty:** Medium
## 答案
<!--more-->
```java
class Solution {
    public ListNode deleteDuplicates(ListNode head) {
        if(head==null) return null;
        ListNode dummy = new ListNode(0), cur=head;
        dummy.next = head;
        ListNode pre = dummy;
        while(cur!=null){
            if(cur.next!=null && cur.next.val==cur.val){
                while(cur.next!=null && cur.val==cur.next.val){
                    cur = cur.next;
                }
                pre.next = cur.next;
                cur = cur.next;
            }else{
                pre = cur;
                cur = cur.next;
            }
        }
        return dummy.next;
    }
}
```
