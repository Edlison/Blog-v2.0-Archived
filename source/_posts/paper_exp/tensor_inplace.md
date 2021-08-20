---
title: Pytorch中的inplace
categories:
- Paper
tags:
- python
- pytorch
---

# 简介

Inplace操作就是将操作后的**新值赋值到原变量地址**上的操作。如`x += 1`.

# Pytorch中的操作

带有`_`后缀对Tensor的操作都是Inplace操作。

`.squeeze()`不是Inplace操作，`.squeeze_()`是Inplace操作。

`x += 1`是Inplace操作，`x = x + 1`不是Inplace操作。

# 注意

**在 pytorch 中, 有两种情况不能使用 inplace operation:**

1. 对于 requires_grad=True 的 **叶子张量(leaf tensor)** 不能使用 inplace operation
2. 对于在 **求梯度阶段需要用到的张量** 不能使用 inplace operation



----

Reference

https://zhuanlan.zhihu.com/p/38475183