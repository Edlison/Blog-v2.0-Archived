---
title: Pytorch中的Tensor
categories:
- Paper
tags:
- python
- pytorch
- tensor
---

# 数据类型

## 旧版：

Tensor是基本的数据类型，Variable是对Tensor的封装。只有Variable引入了计算图，实现自动求导`backward()`.

定义是否需要梯度： `Variable(requires_grad=True)`.

## 新版：

Variable与Tensor进行合并了，`torch.Tensor`现在一样可以实现自动求导。

定义是否需要梯度： `Tensor.requires_grad_()`.

## 初始化：

`Tensor([0, 1, 2])`

`Tensor(2, 3).fill_(9)`

`torch.ones(4, 5)`

## 对Tensor内部属性的操作：

Tensor是一个对象，如果对其操作需要通过`.requires_grad_()`等方法对Field进行赋值。

`torch.ones(2, 3)`等，是一个方法，其中可以通过传`requires_grad=True`等字典对Field进行赋值，由于返回的仍是一个Tensor类型的对象，也可以使用`.requires_grad_()`等方法进行赋值。

## 浮点型

可以通过`tensor.to(torch.float64)`将`Tensor`类型的数据转换成`FloatTensor`类型的数据。

# 梯度

Pytorch计算图中有两种元素：数据（Tensor）和运算（可求导运算）。

Tensor分为两类：叶子结点，非叶子结点。

计算Tensor梯度（即grad属性被赋值）的条件：

- 叶子结点
- `requires_grad=True`
- 依赖该Tensor的所有Tensor`requires_grad=True`

进行反向传播时，需要是一个**标量**。

# 梯度下降

就是在求得了grad的叶子结点，使用`Tensor.data + Tensor.grad.data`实现梯度下降。

# Tensor在cuda上的转移

所有需要使用GPU训练的参数以及关联的参数都需要放到cuda上。

```python
cuda = True if torch.cuda.is_available() else False
device_id = 7
if cuda:
    torch.cuda.set_device(device_id)
Tensor = torch.cuda.FloatTensor if cuda else torch.FloatTensor
```

如果已经是在`torch.FloatTensor`中的参数，可以通过`.type(Tensor)`来转移参数。



----

Reference

https://pytorch.org/docs/stable/tensors.html

https://zhuanlan.zhihu.com/p/85506092