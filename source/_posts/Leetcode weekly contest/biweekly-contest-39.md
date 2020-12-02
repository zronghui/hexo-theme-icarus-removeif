---
title: biweekly-contest-39
toc: true
recommend: 1
uniqueId: '2020-11-14 16:52:55/"biweekly-contest-39".html'
date: 2020-11-15 00:52:55
thumbnail:
categories:
- Leetcode weekly contest
tags:
keywords:
---

[TOC]

- [x] [拆炸弹](https://leetcode-cn.com/contest/biweekly-contest-39/problems/defuse-the-bomb/)**3**
- [x] [使字符串平衡的最少删除次数](https://leetcode-cn.com/contest/biweekly-contest-39/problems/minimum-deletions-to-make-string-balanced/)**4**
- [ ] [到家的最少跳跃次数](https://leetcode-cn.com/contest/biweekly-contest-39/problems/minimum-jumps-to-reach-home/)**5**
- [x] [分配重复整数](https://leetcode-cn.com/contest/biweekly-contest-39/problems/distribute-repeating-integers/)**6**



![image-20201115005339026](https://i.loli.net/2020/11/15/7Aw4iYzBlrh1qaR.png)

<!--more-->



# 1

```java
class Solution {
    public int[] decrypt(int[] code, int k) {
        // 恶心
        int n = code.length;
        int[] res = new int[n];
        if(k==0) return res;
        int d = k>0 ? 1: -1;
        for(int i=0; i<n; i++){
            for(int j=1; j<=Math.abs(k); j++){
                res[i] += code[(i+j*d+n)%n];
            }
        }
        return res;
    }
}
```

# 2

```java
class Solution {
    public int minimumDeletions(String s) {
        int n = s.length();
        // aa 格式  aab 格式
        int[] a = new int[n+1], b = new int[n+1];
        // init
        a[0] = 0;
        b[0] = -1; // 起始不可能 aab 格式
        for(int i=1; i<=n; i++){
            char c = s.charAt(i-1);
            if(c=='a'){
                a[i] = a[i-1];
            }else{
                a[i] = a[i-1]+1;
            }
            
            // 不可能 aab 格式
            if(c=='a' && b[i-1]==-1) b[i]=-1;
            else{
                // 以 a 结尾，a 肯定要删除
                if(c=='a'){
                    b[i] = b[i-1]+1;
                }else{
                    // 可以从 aa 或者 aab 转换过来
                    b[i] = Math.min(a[i-1], b[i-1]==-1 ? n : b[i-1]);
                }
            }
        }
        // aa 格式
        if(b[n]==-1) return a[n];
        return Math.min(a[n], b[n]);
    }
}
```


# 3

```python

```


# 4

```java
class Solution {
    int[] q;
    int m;
    
    public boolean canDistribute(int[] nums, int[] quantity) {
        HashMap<Integer, Integer> map = new HashMap<>();
        m = quantity.length;
        // reverse sort
        Arrays.sort(quantity);
        q = quantity;
        for(int i=0, j=m-1; i<j; i++, j--){
            int t = q[i];
            q[i] = q[j];
            q[j] = t;
        }
        for(int num: nums) 
            map.put(num, map.getOrDefault(num, 0)+1);
        // 暴力
        return dfs(map, 0);
    }
    public boolean dfs(HashMap<Integer, Integer> map, int i){
        if(i==m) return true;
        for(int num: map.keySet()){
            int cnt = map.get(num);
            if(cnt<q[i]) continue;
            map.put(num, cnt-q[i]);
            if(dfs(map, i+1)) return true;
            map.put(num, cnt);
        }
        return false;
    }
}
```

