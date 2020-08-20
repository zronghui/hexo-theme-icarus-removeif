---
title: pca 主成分分析
toc: true
recommend: 1
uniqueId: '2020-08-11 13:07:26/"pca 主成分分析".html'
date: 2020-08-11 21:07:26
thumbnail:
categories:
- python
tags:
keywords:
---

[TOC]

<!--more-->

```python
# use scikit-learn and perform PCA with k
from sklearn import decomposition
import seaborn as sns
import pandas as pd

pca1 = decomposition.PCA(n_components=1)
pc1 = pca1.fit_transform(X1)
df1 = pd.DataFrame({'var':pca1.explained_variance_ratio_, 'PC':['PC1']})

pca2 = decomposition.PCA(n_components=2)
pc2 = pca2.fit_transform(X2)
df2 = pd.DataFrame({'var':pca2.explained_variance_ratio_, 'PC':['PC1','PC2']})

pca3 = decomposition.PCA(n_components=3)
pc3 = pca3.fit_transform(X3)
df3 = pd.DataFrame({'var':pca3.explained_variance_ratio_, 'PC':['PC1','PC2','PC3']})

# make a Scree plot
print('pca1.explained_variance_ratio_', pca1.explained_variance_ratio_)
sns.barplot(x='PC',y="var", 
           data=df1, color="c");

# make a Scree plot
print('pca2.explained_variance_ratio_', pca3.explained_variance_ratio_)
sns.barplot(x='PC',y="var", 
           data=df2, color="c");

# make a Scree plot
print('pca3.explained_variance_ratio_', pca3.explained_variance_ratio_)
sns.barplot(x='PC',y="var", 
           data=df3, color="c");
	
```





[PCA_on_Breast_Cancer_Wisconsin_Data/PCA_on_Breast_Cancer_Wisconsin.ipynb at master · CPotnis/PCA_on_Breast_Cancer_Wisconsin_Data](https://github.com/CPotnis/PCA_on_Breast_Cancer_Wisconsin_Data/blob/master/PCA_on_Breast_Cancer_Wisconsin.ipynb)
[PCA Example in Python with scikit-learn - Python and R Tips](https://cmdlinetips.com/2018/03/pca-example-in-python-with-scikit-learn/)
[Principal Component Analysis (PCA) with Python | DataScience+](https://datascienceplus.com/principal-component-analysis-pca-with-python/)
[PCA on Cancer dataset. Dimensionality reduction for… | by shekhar pandey | Medium](https://medium.com/@skp2707/pca-on-cancer-dataset-4d7a97f5fdb8)
