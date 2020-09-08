---
title: weekly-contest-190
toc: true
recommend: 1
uniqueId: '2020-08-21 06:22:10/"weekly-contest-190".html'
date: 2020-08-21 14:22:10
thumbnail:
categories:
- Leetcode weekly contest
tags:
keywords:
---

[TOC]

- [x] [检查单词是否为句中其他单词的前缀](https://leetcode-cn.com/contest/weekly-contest-190/problems/check-if-a-word-occurs-as-a-prefix-of-any-word-in-a-sentence/)**3**
- [x] [定长子串中元音的最大数目](https://leetcode-cn.com/contest/weekly-contest-190/problems/maximum-number-of-vowels-in-a-substring-of-given-length/)**4**
- [x] [二叉树中的伪回文路径](https://leetcode-cn.com/contest/weekly-contest-190/problems/pseudo-palindromic-paths-in-a-binary-tree/)**5**
- [ ] [两个子序列的最大点积](https://leetcode-cn.com/contest/weekly-contest-190/problems/max-dot-product-of-two-subsequences/)**6**

<!--more-->



# 1

```python
class Solution:
    def isPrefixOfWord(self, ss: str, s: str) -> int:
        for i, si in enumerate(ss.split()):
            if si.startswith(s):
                return i+1
        return -1
```

# 2

```python
class Solution:
    def maxVowels(self, s: str, k: int) -> int:
        
        def vowel(c):
            if c in 'aeiou': return 1
            return 0
        
        l, r = 0, k-1
        cur = sum(vowel(s[i]) for i in range(l, r+1))
        res = cur
        while r+1<len(s):
            r += 1
            l += 1
            if vowel(s[r]): cur += 1
            if vowel(s[l-1]): cur -= 1
            res = max(res, cur)
        return res
```


# 3

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def pseudoPalindromicPaths (self, root: TreeNode) -> int:
        # dfs + 记录所有的奇数节点
        self.res = 0
        odd = set()
        
        def flip(odd, n):
            if n in odd: odd.remove(n)
            else: odd.add(n)
                
        def dfs(root):
            if not root: return
            flip(odd, root.val)
            if not root.left and not root.right and len(odd)<2:
                self.res += 1
            dfs(root.left)
            dfs(root.right)
            flip(odd, root.val)
        dfs(root)
        return self.res
        
```

# 4

注意 num1 nums2 中都至少取 1 个数

nums1[i]*nums2[j] 表示只拿 i j 2个数相乘

```python
dp[i][j] = max(dp[i-1][j-1], dp[i-1][j], dp[i][j-1], nums1[i]*nums2[j]+dp[i-1][j-1], nums1[i]*nums2[j])
```



```python
class Solution:
    def maxDotProduct(self, nums1: List[int], nums2: List[int]) -> int:
        n, m = map(len, [nums1, nums2])
        # dp[i][j]: nums1[:i+1] 与 nums2[:j+1] 的点积最大值
        # 约束：i<n, j<m
        # 初始化
        dp = [[float('-inf')]*m for i in range(n)]
        # 至少相乘一次
        dp[0][0] = nums1[0]*nums2[0]
        for i in range(1, n):
            dp[i][0] = max(dp[i-1][0], nums1[i]*nums2[0])
        for j in range(1, m):
            dp[0][j] = max(dp[0][j-1], nums1[0]*nums2[j])
        
        for i in range(1, n):
            for j in range(1, m):
                dp[i][j] = max(dp[i-1][j-1], dp[i-1][j], dp[i][j-1], nums1[i]*nums2[j]+dp[i-1][j-1], nums1[i]*nums2[j])
        return dp[-1][-1]

```

