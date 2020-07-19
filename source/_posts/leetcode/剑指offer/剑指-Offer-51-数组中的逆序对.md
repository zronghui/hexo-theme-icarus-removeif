---
title: 剑指 Offer 51 数组中的逆序对
toc: true
recommend: 1
uniqueId: '2020-06-30 13:56:05/"剑指 Offer 51 数组中的逆序对".html'
date: 2020-06-30 21:56:05
thumbnail:
categories:
- leetcode
- 剑指offer
tags:
keywords:
---

[TOC]

<!--more-->

[数组中的逆序对 - 数组中的逆序对 - 力扣（LeetCode）](https://leetcode-cn.com/problems/shu-zu-zhong-de-ni-xu-dui-lcof/solution/shu-zu-zhong-de-ni-xu-dui-by-leetcode-solution/)

视频讲解的不错



相较答案里用索引，我的代码写的很垃圾，重新创建了许多新的数组，不过胜在简单

```python
class Solution:
    def reversePairs(self, nums: List[int]) -> int:
        # 归并排序，后半部分元素入队时 加上 前半部分的长度
        def merge(l1, l2):
            l, cnt = [], 0
            while l1 and l2:
                if l2[0]<l1[0]:
                    l.append(l2.pop(0))
                    cnt += len(l1)
                else:
                    l.append(l1.pop(0))
            if l1: l.extend(l1)
            if l2: l.extend(l2)
            return l, cnt
        
        def mergeSort(l):
            if len(l)<2: return l, 0
            l1, cnt1 = mergeSort(l[:len(l)//2])
            l2, cnt2 = mergeSort(l[len(l)//2:])
            l3, cnt3 = merge(l1, l2)
            # print(l3, cnt3)
            return l3, cnt1+cnt2+cnt3
        return mergeSort(nums)[1]
```

人家的代码：

```python
class Solution:
    def reversePairs(self, nums: List[int]) -> int:
        self.cnt = 0
        def merge(nums, start, mid, end, temp):
            i, j = start, mid + 1
            while i <= mid and j <= end:
                if nums[i] <= nums[j]:
                    temp.append(nums[i])
                    i += 1
                else:
                    self.cnt += mid - i + 1
                    temp.append(nums[j])
                    j += 1
            while i <= mid:
                temp.append(nums[i])
                i += 1
            while j <= end:
                temp.append(nums[j])
                j += 1
            
            for i in range(len(temp)):
                nums[start + i] = temp[i]
            temp.clear()
                    

        def mergeSort(nums, start, end, temp):
            if start >= end: return
            mid = (start + end) >> 1
            mergeSort(nums, start, mid, temp)
            mergeSort(nums, mid + 1, end, temp)
            merge(nums, start, mid,  end, temp)
        mergeSort(nums, 0, len(nums) - 1, [])
        return self.cnt

作者：fe-lucifer
链接：https://leetcode-cn.com/problems/shu-zu-zhong-de-ni-xu-dui-lcof/solution/jian-dan-yi-dong-gui-bing-pai-xu-python-by-azl3979/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```

