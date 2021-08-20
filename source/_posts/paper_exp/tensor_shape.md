---
title: Tensor的Shape操作
categories:
- Paper
tags:
- python
- pytorch
---

# 改变Tensor形状

改变Tensor形状时，原Tensor中的数据是如何重构的。

## Tensor数据形状的变化

### 同一维度

每一行往前或往后拼接，直到满足这个维度要求的值。

```python
x = torch.autograd.Variable(torch.Tensor([[1, 2], [3, 4], [5, 6]]), requires_grad=True)
tensor([[1., 2.],
        [3., 4.],
        [5., 6.]], requires_grad=True)
x = x.view(2, 3)
tensor([[1., 2., 3.],
        [4., 5., 6.]], grad_fn=<ViewBackward>)
```

### 跨维度

从最低维上的数值一直到高纬进行拼接，不同维度上不够的会跨维度进行拼接。

```python
x = torch.autograd.Variable(torch.Tensor([[[1, 2], [3, 4], [5, 6]], [[7, 8], [9, 10], [11, 12]]]), requires_grad=True)
tensor([[[ 1.,  2.],
         [ 3.,  4.],
         [ 5.,  6.]],

        [[ 7.,  8.],
         [ 9., 10.],
         [11., 12.]]], requires_grad=True)
x = x.view(3, 4)
tensor([[ 1.,  2.,  3.,  4.],
        [ 5.,  6.,  7.,  8.],
        [ 9., 10., 11., 12.]], grad_fn=<ViewBackward>)
```

## 查看形状的方式

### 1. shape

通过`Tensor.shape`可以获取该Tensor的形状信息。这里的`shape`属于Field，它是一个数组。

使用方法：想要获取某个维度的大小只需要`.shape[0]`就可以了。

### 2. size

通过`Tensor.size()`可以获取该Tensor**某个维度上**的信息。这里的`size`属于Method，它是一个方法，需要传dim信息。

使用方法：`.size(0)`

## 改变形状的方式

### 1. reshape

- 不受该Tensor是否是连续的影响。

- 返回的**可能是**Tensor的copy，**也可能不是**。

### 2. view

- 只能改变连续的Tensor（没有调用过`transpose`, `permute`等）。
- 与原Tensor共享存储器（不是内存）。

### 3. squeeze/unsqueeze

对Tensor维度值为1的维度进行删除或增加。

```python
a = Tensor(2, 1, 3, 1)
# torch.Size([2, 1, 3, 1])
a = a.squeeze(1)
# torch.Size([2, 3, 1])
a = a.unsqueeze(0)
# torch.Size([1, 2, 3, 1])
a = a.squeeze()
# torch.Size([2, 3])
```

**注意：**

- 不传参删除所有维度值为1的维度。
- 不在原本的存储空间上操作，**只是显示改变样子**，想要获取需要赋值。

# Tensor的拼接

将多个Tensor进行拼接，使用`torch.cat(inputs, dim=0)`.

将输入在dim维度上进行拼接。

```python
x = Tensor([[1, 2, 3], [4, 5, 6]])
# x:  tensor([[1., 2., 3.],
#         [4., 5., 6.]], device='cuda:7')
y = Tensor([[4, 5, 6], [7, 8, 9]])
# y:  tensor([[4., 5., 6.],
#         [7., 8., 9.]], device='cuda:7')
z = torch.cat([x, y], dim=0)
# tensor([[1., 2., 3.],
#         [4., 5., 6.],
#         [4., 5., 6.],
#         [7., 8., 9.]], device='cuda:7') torch.Size([4, 3])
z = torch.cat([x, y], dim=1)
# tensor([[1., 2., 3., 4., 5., 6.],
#         [4., 5., 6., 7., 8., 9.]], device='cuda:7') torch.Size([2, 6])
```

**注意：**

- 传入的inputs是一个序列，`list`或`tuple`.
- 传入的inputs在需要拼接的dim上shape必须一致。

# Tensor的合并

`torch.flatten(input, start_dim, end_dim)`

将`start_dim`到`end_dim`维度之间的值合并。

**好像**torch下的方法，都可以通过`Tensor.flatten()`来调用。

# Tensor的分解

## 1. torch.split()

`torch.split(tensor, split_size, dim)`

`split_size`: 间隔大小。

```python
a = Tensor(3, 2)
x, y, z = a.split(1, 0)
```

**注意**：

- 最后不足的也会被分成一个块。
- 注意类型，返回的是一个tuple，可以用多个变量来接收，tuple里装的是Tensor。

## 2. torch.chunk()

`torch.chunk(tensor, chunks, dim)`

`chunks`: 分块数。

```python
a = Tensor(3, 2)
x, y, z = a.chunk(3, 0)
```

**注意**：最后不足的也会被分成一个块。



----

Reference

https://www.cnblogs.com/dilthey/p/12376179.html