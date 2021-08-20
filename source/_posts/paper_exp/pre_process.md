---
title: 数据预处理
catagories:
- Paper
tags:
- python
---

# 介绍

由于不同特征的量纲可能不一致，数值差值较大，可能会影响到数据分析的结果

# MinMaxScaler(归一化)

将数据映射到[0, 1]区间

新数据 = (数值 - 最小值) / (最大值 - 最小值)

# Z-Score(标准化)

处理后的数据均值为0，方差为1

新数据 = (数据 - 均值) / 标准差

# 正则化

**正则化是为了防止过拟合**



# Tips

`int`是带符号数

`uint`是不带符号数

----

Reference

正则化 https://www.zhihu.com/question/20924039