---
title: weekly-contest-212
toc: true
recommend: 1
uniqueId: '2020-10-25 10:39:45/"weekly-contest-212".html'
date: 2020-10-25 18:39:45
thumbnail:
categories:
tags:
keywords:
---

[TOC]



![image-20201025184334845](/Users/zhangronghui/Library/Application Support/typora-user-images/image-20201025184334845.png)

<!--more-->



# 1

```python
class Solution {
    public char slowestKey(int[] l, String s) {
        int mi = 0, m=l[0];
        for(int i=1; i<l.length; i++){
            int duration = l[i]-l[i-1];
            if(duration>m){
                m = duration;
                mi = i;
            }else if(duration==m && s.charAt(i)>s.charAt(mi)){
                mi = i;
            }
        }
        return s.charAt(mi);
    }
}
```

# 2

```python
class Solution {
    public List<Boolean> checkArithmeticSubarrays(int[] nums, int[] l, int[] r) {
        int n=nums.length, m=l.length;
        List<Boolean> res = new ArrayList<>();
        for(int i=0; i<m; i++){
            res.add(check(nums, l[i], r[i]));
        }
        return res;
    }
    public boolean check(int[] nums, int l, int r){
        if(r==l+1) return true;
        int[] arr = new int[r-l+1];
        for(int i=l; i<=r; i++){
            arr[i-l] = nums[i];
        }
        Arrays.sort(arr);
        int gap = arr[1]-arr[0];
        for(int i=1; i<r-l+1; i++){
            if(arr[i]-arr[i-1]!=gap) return false;
        }
        return true;
    }
}
```

# 3

一开始自己瞎jb写，也能过

```python
class Solution {
    boolean changed = true;
    public int minimumEffortPath(int[][] heights) {
        int n=heights.length, m=heights[0].length;
        int[][] dp = new int[n][m];
        for(int i=0; i<n; i++){
            for(int j=0; j<m; j++){
                dp[i][j] = Integer.MAX_VALUE;
            }
        }
        dp[0][0] = 0;
        while(changed){
            changed = false;
            for(int i=0; i<n; i++){
                for(int j=0; j<m; j++){
                    update(heights, dp, n, m, i, j, i-1, j);
                    update(heights, dp, n, m, i, j, i, j-1);
                }
            }
            // right to left
            for(int i=0; i<n; i++){
                for(int j=m-1; j>=0; j--){
                    update(heights, dp, n, m, i, j, i, j+1);
                }
            }
            // down to up
            for(int j=m-1; j>=0; j--){
                for(int i=0; i<n; i++){
                    update(heights, dp, n, m, i, j, i+1, j);
                }
            }
        }
        return dp[n-1][m-1];
    }
    
    public void update(int[][] heights, int[][] dp, int n, int m, int i, int j, int ii, int jj){
        if(ii>=0 && ii<n && jj>=0 && jj<m){
            int t = Math.max(Math.abs(heights[i][j]-heights[ii][jj]), dp[ii][jj]);
            if(t<dp[i][j]){
                changed = true;
                dp[i][j] = t;
            }
        }
    }
}
```

[最小体力消耗路径 - 最小体力消耗路径 - 力扣（LeetCode）](https://leetcode-cn.com/problems/path-with-minimum-effort/solution/zui-xiao-ti-li-xiao-hao-lu-jing-by-zerotrac2/)

### 并查集

```java
class DSU{
    int[] p;
    DSU(int n){
        p = new int[n];
        for(int i=0; i<n; i++){
            p[i] = i;
        }
    }
    public int find(int x){
        if(p[x]!=x)
            p[x] = find(p[x]);
        return p[x];
    }
    public void union(int x, int y){
        int u = find(x), v=find(y);
        if(u!=v)
            p[u] = v;
    }
    public boolean isSame(int x, int y){
        return find(x)==find(y);
    }
}

class Edge{
    int x;
    int y;
    int dist;
    Edge(int x, int y, int dist){
        this.x = x;
        this.y = y;
        this.dist = dist;
    }
}
class Solution {
    public int minimumEffortPath(int[][] heights) {
        int n=heights.length, m=heights[0].length;
        List<Edge> edges = new ArrayList<>();
        for(int i=0; i<n; i++){
            for(int j=0; j<m; j++){
                int cur=i*m+j;
                if(j>0) edges.add(new Edge(cur, cur-1, Math.abs(heights[i][j]-heights[i][j-1])));
                if(i>0) edges.add(new Edge(cur, cur-m, Math.abs(heights[i][j]-heights[i-1][j])));
            }
        }
        Collections.sort(edges, (a,b) -> a.dist-b.dist);
        DSU dsu = new DSU(n*m);
        for(Edge edge: edges){
            dsu.union(edge.x, edge.y);
            if(dsu.isSame(0, n*m-1)) return edge.dist;
        }
        return 0;
    }
}
```



### 二分

```java
class Solution {
    int n, m;
    public int minimumEffortPath(int[][] heights) {
        int l=0, r=999999, mid, ans=0;
        n=heights.length;
        m=heights[0].length;
        boolean[] visited;
        while(l<=r){
            mid = l+(r-l)/2;
            visited = new boolean[n*m];
            // mid >= ans
            if(check(heights, visited, mid, 0, 0, heights[0][0])){
                ans = mid;
                r = mid-1;
            }else{
                l = mid+1;
            }
        }
        return ans;
    }
    public boolean check(int[][] heights, boolean[] visited, int dist, int i, int j, int pre){
        if(i<0 || j<0 || i>=n || j>=m || visited[i*m+j] || Math.abs(pre-heights[i][j])>dist) return false;
        if(i==n-1 && j==m-1) return true;
        visited[i*m+j] = true;
        pre = heights[i][j];
        return check(heights, visited, dist, i-1, j, pre)
            || check(heights, visited, dist, i+1, j, pre)
            || check(heights, visited, dist, i, j-1, pre)
            || check(heights, visited, dist, i, j+1, pre);


    }
}
```



# 4

```python

```

