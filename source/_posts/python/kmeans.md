---
title: kmeans
toc: true
recommend: 1
uniqueId: '2020-08-11 09:25:11/"kmeans".html'
date: 2020-08-11 17:25:11
thumbnail:
categories:
- python
tags:
keywords:
---

[TOC]

<!--more-->



## 原理

![image-20200811173514307](https://i.loli.net/2020/08/11/2p6PvxcKr1YhboQ.png)

## 数据准备

```python
import numpy as np

# set the random seed for repeatability
np.random.seed(2) 

# set our constants for the dataset
d = 2    # dimensions
m = 100  # samples

# generate m randomly-distributed d-dimensional samples
X = np.random.random((m, d))

# set the centers
center_1 = [ 0,4]
center_2 = [-1,2]
center_3 = [ 3,0]

# generate 3 clusters each with m normally-distributed, 2-dimensional samples
X1 = np.random.multivariate_normal(center_1, [[1,  0.8], [0.8,  1]],   size=m)
X2 = np.random.multivariate_normal(center_2, [[0.3,0.2], [0.2,0.3]], size=m)
X3 = np.random.multivariate_normal(center_3, [[1,  0],   [0,  0.5]], size=m)

# generate the dataset matrix (3*m rows, d columns)
X = np.append(X1, X2, axis=0)
X = np.append(X,  X3, axis=0)
```



## scikit-learn

```python
# import the scikit-learn model and pyplot

### CODE HERE
import matplotlib.pyplot as plt
from sklearn.cluster import KMeans

# generate and fit the model to the dataset using k clusters (your choice on how many -- I encourage you to do several and explore the performance)
km = KMeans(n_clusters=3, random_state=2).fit_predict(X)

# plot the cluster centers and training examples using different colors for each cluster
plt.figure(figsize=(8,8))
ax = plt.gca()
ax.set_xlabel('x')
ax.set_ylabel('y')
ax.set_title('K-Means')
ax.scatter(X[:,0], X[:,1], c=km)
```

```python
km 是每个点分配的 label id 内容:
array([0, 0, 2, 2, 2, 2, 0, 2, 0, 2, 2, 0, 2, 2, 2, 0, 2, 2, 0, 0, 2, 2,
       2, 2, 2, 0, 2, 2, 2, 0, 0, 2, 2, 2, 2, 2, 2, 2, 2, 2, 0, 2, 2, 2,
       2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 0, 0, 0, 2, 2, 2, 2, 0, 0, 2, 2, 2,
       0, 2, 2, 2, 0, 0, 2, 2, 2, 2, 2, 2, 0, 0, 2, 0, 0, 0, 0, 0, 2, 0,
       0, 2, 2, 2, 0, 0, 2, 2, 0, 2, 2, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0,
       0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0,
       0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0,
       0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0,
       0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 2, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0,
       0, 0, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1,
       1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1,
       1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 0, 1, 1, 1,
       1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1,
       1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1], dtype=int32)
```

结果：

<img src="https://i.loli.net/2020/08/11/xzhR95BHVrFAIZb.png" alt="image-20200811173025348" style="zoom:25%;" />

random_state 的含义，为了设置 random.seed(random_state) 这样每次运行的结果就一致了

## 自己实现

```python
# write your implementation of k-means clustering
import random


def MyKMeans(data, n_clusters=3, max_iter=100, random_state=2):
    random.seed(random_state)
    # 1. Randomly initialize the cluster centers
    centers = random.choices(data, k=n_clusters)
    
    def closestToCenter(x, y):
        # return which center is (x, y) closest to
        return min(range(n_clusters), key=lambda i: (x-centers[i][0])**2+(y-centers[i][1])**2)
    
    # 2. Repeat for `max_iter` iterations:
    for _ in range(max_iter):
        # 3. Assign training examples to the nearest cluster center
        clusters = [[] for _ in range(n_clusters)]
        for x, y in data:
            clusters[closestToCenter(x, y)].append([x, y])
        # 4. Update the cluster centers by computing the mean of the cluster's assigned training examples
        for i in range(n_clusters):
            if len(clusters[i])!=0:
                centers[i][0] = sum(x[0] for x in clusters[i])/len(clusters[i])
                centers[i][1] = sum(x[1] for x in clusters[i])/len(clusters[i])
    return [closestToCenter(x, y) for x, y in data]
# generate and fit the model to the dataset using k clusters (your choice on how many -- I encourage you to do several and explore the performance)
mykm = MyKMeans(data=X, n_clusters=3, max_iter=100, random_state=2)

# plot the cluster centers and training examples using different colors for each cluster
plt.figure(figsize=(8,8))
ax = plt.gca()
ax.set_xlabel('x')
ax.set_ylabel('y')
ax.set_title('My-K-Means')
ax.scatter(X[:,0], X[:,1], c=mykm)
```



<img src="https://i.loli.net/2020/08/11/vWutSNkz3JixjHG.png" alt="image-20200811172705457" style="zoom:25%;" />
