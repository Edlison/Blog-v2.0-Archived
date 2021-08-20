---
title: 一些数学概念
categories:
- Paper
tags:
- math
---

# 范数

## 向量范数

### 1-norm

$$
\parallel x\parallel_1 = \sum_{i=1}^N\vert x_i\vert
$$

元素绝对值之和。

### 2-norm

$$
\parallel x\parallel = \sqrt{\sum_{i=1}^Nx_i^2}
$$

欧几里得范数，元素绝对值的平方再开方。
$$
\parallel x\parallel_\infin = \max_i\vert x_i\vert
$$
$\infin$范数，所有元素中绝对值的最大值。
$$
\parallel x\parallel_{-\infin} = \min_i\vert x_i\vert
$$
$-\infin$范数，所有元素中绝对值最小的值。

## p-norm

$$
\parallel x\parallel_p = {(\sum_{i=1}^N\vert x_i\vert^p)}^{1/p}
$$

元素的p次方和1/p次幂。

## 矩阵范数

...

# 距离

## 欧氏距离

$$
D(X,Y) = \sqrt{\sum_{i=1}^n(x_i-y_i)^2}
$$

## 明可夫斯基距离

$$
D(X,Y) = (\sum_{i=1}^n\vert x_i-y_i\vert^p)^{1/p}
$$

当p=2时，就是欧氏距离。

## 曼哈顿距离

$$
D(X,Y) = \sum_{i=1}^n\vert x_i-y_i\vert
$$







----

Reference

https://www.jianshu.com/p/6e5dff42a77e

https://blog.csdn.net/susanzhang1231/article/details/52127011

https://zhuanlan.zhihu.com/p/137073968

