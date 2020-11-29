---
title: biweekly-contest-40
toc: true
recommend: 1
uniqueId: '2020-11-28 16:01:12/"biweekly-contest-40".html'
date: 2020-11-29 00:01:12
thumbnail:
categories:
tags:
keywords:
---

[TOC]

- [x] [最大重复子字符串](https://leetcode-cn.com/contest/biweekly-contest-40/problems/maximum-repeating-substring/)**3**
- [x] [合并两个链表](https://leetcode-cn.com/contest/biweekly-contest-40/problems/merge-in-between-linked-lists/)**4**
- [x] [设计前中后队列](https://leetcode-cn.com/contest/biweekly-contest-40/problems/design-front-middle-back-queue/)**5**
- [x] [得到山形数组的最少删除次数](https://leetcode-cn.com/contest/biweekly-contest-40/problems/minimum-number-of-removals-to-make-mountain-array/)**6**

前三题半小时，第四题做了一小时，一开始没有思路。最后 23:59:47 做完，很极限了

![image-20201129000232986](https://i.loli.net/2020/11/29/i3vGcAyVq4Ob8Co.png)

<!--more-->



# 1

```java
class Solution {
    public int maxRepeating(String s, String word) {
        int res = 0;
        int n = s.length(), m = word.length();
        int i = 0;
        while(i<n){
            if(s.charAt(i)!=word.charAt(0)){
                i++;
                continue;
            }
            int j = 0;
            while(i+j<n){
                if(s.charAt(i+j)!=word.charAt(j%m))
                    break;
                j++;
            }
            res = Math.max(res, j/m);
            i += 1;
            // System.out.println(j+"|"+m);
        }
        return res;
    }
}
```

# 2

```java
class Solution {
    public ListNode mergeInBetween(ListNode list1, int a, int b, ListNode list2) {
        ListNode a1=list1, b1=list1, tail2=list1;// a-1, b+1, list2 tail
        ListNode cur=list1;
        for(int i=0; i<=b+1; i++){
            if(i-(a-1)==0) a1 = cur;
            if(i-(b+1)==0) b1 = cur;
            cur = cur.next;
        }
        cur = list2;
        while(cur.next!=null) cur = cur.next;
        tail2 = cur;
        a1.next = list2;
        tail2.next = b1;
        return list1;
    }
}
```


# 3

```java
// 2 个 deque
// [a,b,c] [d,e,f]
class FrontMiddleBackQueue {
    private Deque<Integer> d1 = new LinkedList<>();
    private Deque<Integer> d2 = new LinkedList<>();
    public FrontMiddleBackQueue() {}
    
    public void pushFront(int val) {
        d1.offerFirst(val);
    }
    public void pushBack(int val) {
        d2.offerLast(val);
    }
    public int popFront() {
        if(d1.isEmpty() && d2.isEmpty()) return -1;
        if(!d1.isEmpty()) return d1.pollFirst();
        return d2.pollFirst();
    }
    public int popBack() {
        if(d1.isEmpty() && d2.isEmpty()) return -1;
        if(!d2.isEmpty()) return d2.pollLast();
        return d1.pollLast();
    }
    
    public void pushMiddle(int val) {
        while(d1.size()>d2.size())
            d2.offerFirst(d1.pollLast());
        while(d2.size()>d1.size()+1)
            d1.offerLast(d2.pollFirst());
        d1.offerLast(val);
    }
    public int popMiddle() {
        if(d1.isEmpty() && d2.isEmpty()) return -1;
        while(d1.size()>d2.size()+1)
            d2.offerFirst(d1.pollLast());
        while(d2.size()>d1.size())
            d1.offerLast(d2.pollFirst());
        return d1.pollLast();
    }
}
```


# 4

```java
// 最长递增子序列
class Solution {
    public int minimumMountainRemovals(int[] nums) {
        int res = Integer.MIN_VALUE, n=nums.length;
        int[] dp1 = new int[n];
        int[] dp2 = new int[n];
        Arrays.fill(dp1, 1);
        Arrays.fill(dp2, 1);
        for(int i=1; i<n-1; i++){
            for(int j=0; j<i; j++){
                if(nums[j]>=nums[i]) continue;
                dp1[i] = Math.max(dp1[i], dp1[j]+1);
            }
        }
        for(int i=n-2; i>0; i--){
            for(int j=i+1; j<n; j++){
                if(nums[j]>=nums[i]) continue;
                dp2[i] = Math.max(dp2[i], dp2[j]+1);
            }
        }
        // for(int i: dp1) System.out.print(i+"|");
        // System.out.println("");
        // for(int i: dp2) System.out.print(i+"|");
        // System.out.println("");
        for(int i=1; i<n-1; i++){
            res = Math.max(res, dp1[i]+dp2[i]-1);
        }
        return n-res;
    }
}
```

