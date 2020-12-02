---
title: weekly-contest-215
toc: true
recommend: 1
uniqueId: '2020-11-15 04:10:36/"weekly-contest-215".html'
date: 2020-11-15 12:10:36
thumbnail:
categories:
- Leetcode weekly contest
tags:
keywords:
---

[TOC]

- [x] [设计有序流](https://leetcode-cn.com/contest/weekly-contest-215/problems/design-an-ordered-stream/)**3**
- [x] [确定两个字符串是否接近](https://leetcode-cn.com/contest/weekly-contest-215/problems/determine-if-two-strings-are-close/)**4**
- [x] [将 x 减到 0 的最小操作数](https://leetcode-cn.com/contest/weekly-contest-215/problems/minimum-operations-to-reduce-x-to-zero/)**5**
- [ ] [最大化网格幸福感](https://leetcode-cn.com/contest/weekly-contest-215/problems/maximize-grid-happiness/)**7**

![image-20201115121121254](https://i.loli.net/2020/11/15/QnlxpMiLIF315YJ.png)

<!--more-->



# 1

```java
class OrderedStream {
    String[] l;
    int n;
    int ptr = 1;
    
    public OrderedStream(int n) {
        l = new String[n+1];
        this.n = n+1; // ignore index 0
    }
    
    public List<String> insert(int id, String value) {
        l[id] = value;
        List<String> res = new ArrayList<>();
        if(id!=ptr) return res;
        res.add(value);
        int i = id+1;
        while(i<n && l[i]!=null){
            res.add(l[i]);
            i++;
        }
        ptr = i;
        return res;
    }
}

/**
 * Your OrderedStream object will be instantiated and called as such:
 * OrderedStream obj = new OrderedStream(n);
 * List<String> param_1 = obj.insert(id,value);
 */
```

# 2

```java
class Solution {
    public boolean closeStrings(String w1, String w2) {
        if(w1.length()!=w2.length()) return false;
        int[] m1 = new int[26], m2 = new int[26];
        for(char c: w1.toCharArray())
            m1[c-'a'] += 1;
        for(char c: w2.toCharArray())
            m2[c-'a'] += 1;
        List<Integer> l1 = new ArrayList<>(),  l2 = new ArrayList<>();
        for(int i=0; i<26; i++){
            if(m1[i]!=0) l1.add(m1[i]);
            if(m2[i]!=0) l2.add(m2[i]);
            // 你有的字符我也要有
            if(m1[i]==0 && m2[i]!=0) return false;
            if(m2[i]==0 && m1[i]!=0) return false;
        }
        // System.out.println(l1);
        // System.out.println(l2);
        // System.out.println(l1.size()+"|"+l2.size());
        if(l1.size()!=l2.size()) return false;
        Collections.sort(l1);
        Collections.sort(l2);
        for(int i=0; i<l1.size(); i++){
            // Hence the comparison with == only works for numbers between -128 and 127.
            // System.out.println(l1.get(i)+"|"+l2.get(i) +"|"+ (l1.get(i)!=l2.get(i)));
            if((l1.get(i)-l2.get(i))!=0)
                return false;
        }
        return true;
    }
}
```

# 3

postsum -> sufsum 更合适

```java
class Solution {
    public int minOperations(int[] nums, int x) {
        int n = nums.length;
        int[] presum = new int[n+1], postsum = new int[n+1];
        for(int i=1; i<=n; i++){
            presum[i] = presum[i-1]+nums[i-1];
            postsum[i] = postsum[i-1]+nums[n-i];
        }
        // for(int i: presum) System.out.print(i+"|");
        // System.out.println("");
        // for(int i: postsum) System.out.print(i+"|");
        // 0|1|2|6|8|11|
        // 0|3|5|9|10|11|
        // 双指针
        int i=0, j=n;
        int res = Integer.MAX_VALUE;
        while(i<res && i<=n && presum[i]<=x){
            while(n-j>i-1 && postsum[j]+presum[i]>x)
                j--;
            if(n-j>i-1 && (postsum[j]+presum[i]-x==0))
                res = Math.min(res, i+j);
            i++;
        }
        return res==Integer.MAX_VALUE?-1:res;
    }
}
```


# 4

```java

```

