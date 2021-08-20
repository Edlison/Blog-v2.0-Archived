---
title: Batch Size
categories:
- Paper
tags:
- batch_size
---

# 对Batch_size与lr的认识

## 大Batch(Batch -> Full)：

好处：

- 更好的代表样本总体，更准确指向极值。
- 不同权重梯度差别大，学习率不统一。

坏处：

- 内存局限，无法一次性载入全部样本。
- 每个Batch，由于**采样性差异**，梯度修正值**相互抵消**。

## 小Batch(Batch -> 1)：

好处：

- 可以在一个epoch上迭代很多次，参数更新多次。

坏处：

- 每次修正都向各自样本的梯度方向修正，**难收敛**。