---
title: matplotlib
toc: true
recommend: 1
uniqueId: '2020-08-11 06:28:08/"matplotlib".html'
date: 2020-08-11 14:28:08
thumbnail:
categories:
- python
tags:
keywords:
---

[TOC]

<!--more-->

### 引入包

```python
import matplotlib.pyplot as plt
```



### 设置 xlabel ylabel title 显示网格线

```python
fig = plt.figure(figsize=(6, 6)) #
ax = plt.gca() # get current axes
ax.set_xlabel('theta')
ax.set_ylabel('omega')
ax.set_title('theta_0=1/2, omega_0=0, n_end=200')
ax.scatter(t1, t2)


plt.grid(True, linestyle = "-.", color = "r", linewidth = "3")
```

### 设置 x y 轴范围



```python
plt.xlim((-1.1, 1.1))
plt.ylim((-1.1, 1.1))
```



### 画点、画线

#### 画线：

ax.plot(lx, ly, 'r-') # 画红色实线

#### 画点:

ax.plot(x, y, marker='.', markersize=40, markerfacecolor='red')

更多 marker：

[matplotlib.axes.Axes.plot — Matplotlib 3.1.2 documentation](https://matplotlib.org/3.1.1/api/_as_gen/matplotlib.axes.Axes.plot.html)

**Markers**

| character | description           |
| --------- | --------------------- |
| `'.'`     | point marker          |
| `','`     | pixel marker          |
| `'o'`     | circle marker         |
| `'v'`     | triangle_down marker  |
| `'^'`     | triangle_up marker    |
| `'<'`     | triangle_left marker  |
| `'>'`     | triangle_right marker |
| `'1'`     | tri_down marker       |
| `'2'`     | tri_up marker         |
| `'3'`     | tri_left marker       |
| `'4'`     | tri_right marker      |
| `'s'`     | square marker         |
| `'p'`     | pentagon marker       |
| `'*'`     | star marker           |
| `'h'`     | hexagon1 marker       |
| `'H'`     | hexagon2 marker       |
| `'+'`     | plus marker           |
| `'x'`     | x marker              |
| `'D'`     | diamond marker        |
| `'d'`     | thin_diamond marker   |
| `'|'`     | vline marker          |
| `'_'`     | hline marker          |



```python
t1, t2 = run_pendulum(0.5, 0, 20)
xs = list(map(lambda i: -sin(i), t1))
ys = list(map(lambda i: cos(i), t1))

fig = plt.figure(figsize=(6,6)) #
ax = plt.gca() # get current axes
for i in [0, 5, 10, 15]:
    x, y = xs[i], ys[i]
    # 画线 前一个列表是 x 后一个列表是 y 
    ax.plot([0,x], [0, y], "k") # plot the black line
    # 画一个圆点，颜色是红，大小 40, 坐标 (x, y)
    ax.plot(x, y, marker='.', markersize=40, markerfacecolor='red') # plot the red square

# set the limits
plt.xlim((-1.1, 1.1))
plt.ylim((-1.1, 1.1))
```



**Line Styles**

| character | description         |
| --------- | ------------------- |
| `'-'`     | solid line style    |
| `'--'`    | dashed line style   |
| `'-.'`    | dash-dot line style |
| `':'`     | dotted line style   |

Example format strings:

```
'b'    # blue markers with default shape
'or'   # red circles
'-g'   # green solid line
'--'   # dashed line with default color
'^k:'  # black triangle_up markers connected by a dotted line
```

**Colors**

The supported color abbreviations are the single letter codes

| character | color   |
| --------- | ------- |
| `'b'`     | blue    |
| `'g'`     | green   |
| `'r'`     | red     |
| `'c'`     | cyan    |
| `'m'`     | magenta |
| `'y'`     | yellow  |
| `'k'`     | black   |
| `'w'`     | white   |

### 动画

如果只有一个 plot 需要更新，直接返回就好

否则， updateALL 函数返回 plot 列表

```python
t1, t2 = run_pendulum(0.5, 0, 200)
xs = list(map(lambda i: -sin(i), t1))
ys = list(map(lambda i: cos(i), t1))

fig = plt.figure(figsize=(6,6)) #
ax = plt.gca() # get current axes
plot0, = ax.plot([0,xs[0]], [0, ys[0]], "k") # plot the black line
plot1, = ax.plot(xs[0], ys[0], marker='.', markersize=40, markerfacecolor='red') # plot the red square

# set the limits
plt.xlim((-1.1, 1.1))
plt.ylim((-1.1, 1.1))


from mpl_toolkits.mplot3d import Axes3D
from matplotlib.animation import FuncAnimation
from IPython.display import HTML

def updateALL(i):
    plot1.set_xdata(xs[i])
    plot1.set_ydata(ys[i])
    plot0.set_xdata([0, xs[i]])
    plot0.set_ydata([0, ys[i]])
    return [plot0, plot1]
    
anim = FuncAnimation(fig, updateALL, frames=range(0, 200), interval=100, repeat=True)
HTML(anim.to_html5_video())
```





### subplot 子图

![image-20200811162558483](https://i.loli.net/2020/08/11/ehdNzQDfJ96CF7s.png)

```python
plt.figure(figsize=(15,8))
plt.subplot(121, title='"Neat" K-Means')
plt.scatter(x_neat[:,0], x_neat[:,1], c=km_neat)
plt.subplot(122, title='"Messy" K-Means')
plt.scatter(x_messy[:,0], x_messy[:,1], c=km_messy)
```



### 设置 legend

legend 是啥：

[Matplotlib 系列之「Legend 图例」 - 知乎](https://zhuanlan.zhihu.com/p/41781440)

<img src="https://i.loli.net/2020/08/11/xRawbpZmgPCt7UT.jpg" alt="img" style="zoom:50%;" />

可以同时设置多条线的 legend 

plt.legend(handles=[s1],labels=['theta_0=1/2, omega_0=0, n_end=200'],loc='best')

```python
fig = plt.figure(figsize=(6, 6)) #
ax = plt.gca() # get current axes
ax.set_xlabel('theta')
ax.set_ylabel('omega')
# ax.set_title('theta_0=1/2, omega_0=0, n_end=200')
s1 = ax.scatter(t1, t2)
plt.legend(handles=[s1],labels=['theta_0=1/2, omega_0=0, n_end=200'],loc='best')
ax
```







