---
title: 在GAN上实现Multi-View
categories:
- Paper
tags:
- python
- pytorch
- gan
- multi_view
---

# 遇到的坑

## 网络的组合

用列表的数据结构，存储Module。

使用 Python 的 list 添加的全连接层和它们的 parameters **并没有自动注册**到我们的网络中。

**解决方案：**如果想要继承了nn.Module的网络组合多个子网络，需要通过`nn.Sequential`, `nn.ModuleList`等**对网络参数进行注册**。

## 计算Loss的两个Tensor形状不一致

由于通过网络经过各种**矩阵相乘**，最后得到的结果是一个二维的Tensor。自己定义的real_label是一个一维的Tensor。`torch.Size([256])` 不等于 `torch.Size([256, 1])`.

```python
g_loss = self.losser.get_loss(d_fake, real_label)
```

**解决方案：**在网络D最后的输出使用`.squeeze()`对维度值为1的维度进行挤压。

# 注意

对在每个View上的数据，使用list或tuple的数据结构进行存储，不必再转到Tensor上。

使用Processor再训练(train)过程中对数据进行处理，虽然可能会影响效率，但是是最方便的实现方法，在Pytorch的DataLoader只能返回Batch_size的数据，无法再加一个维度，保存View。









----

Reference

https://discuss.pytorch.org/t/when-should-i-use-nn-modulelist-and-when-should-i-use-nn-sequential/5463