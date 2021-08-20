---
title: 搭建MVADG网络
categories:
- Paper
tags:
- python
- pytorch
- multi_view
- anomaly
- gan
---

# 遇到的坑

## 1. np.prod()

对传入的list计算乘积，可以指定dim。

## 2. nn.ConvTranspose2d()

反卷积操作。

## 3. argument after * must be an iterable

如果在函数内部使用了*, 对传入的参数进行unpack, 需要注意需要传入一个可以迭代的对象。

```python
def Reshape(input, shape):
  return input.view(input.shape[0], *shape)
```

如果传的是一个标量，可以将它变为一个tuple传入。

```python
shape = np.prod([1, 2, 3])
Reshape(input, (shape,))
```

在传入的shape后加一个`,`就相当于是一个tuple了。

## 4. reduce failed to synchronize: device-side assert triggered.

这是神经网络最后一层没有normalize导致的，可以加一层Sigmoid()解决。

## 5. d_fake的batch_size一直是torch.Size([256, 1])

在计算`bce_loss(d_fake, real_label)`时，生成的d_fake没有与最后一个batch_size匹配。原因在于Net中d_fake是由gen_imgs输入D生成的，而一开始生成的噪声的batch_size决定了gen_imgs的第一维度的大小。

问题出在一开始生成噪声的时候输入的是self.batch_size而不是imgs_batch.

## 6. std evaluated to zero after conversion to torch.int64, leading to division by zero.

dataset在返回数据时通过`transform.Normalize()`时出现的问题。

dataset在读取自制的数据集时，类型转换的问题。需要将文本的`str`格式转成`float`,而不是`int`.

## 7. output with shape [1, 28, 28] doesn't match the broadcast shape [3, 28, 28].

由于MNIST数据集通道的问题。

```python
transforms.Normalize((0.5, 0.5, 0.5), (0.5, 0.5, 0.5))
# 改为
transforms.Normalize((0.5,), (0.5,))
```

## 8. _pickle.UnpicklingError: A load persistent id instruction was encountered, but no persistent_load...

由于pytorch版本所致，`torch.save()`, `torch.load()`里面保存了版本信息，不一样的话会报错。

## 9. Only one class present in y_true. ROC AUC score is not defined in that case.

在计算ROC AUC时，如果一个batch中的类别都是同样的，就会报错。

解决方案

- 整理0，1标签数据较为均衡
- 使用try except然后pass

## 10. one of the variables needed for gradient computation has been modified by an inplace operation 由于Pytorch版本的大坑

由于pytorch版本不一样，因此报错！！！

都是Python3.7

Pytorch = 1.7.1 (报错)

Pytorch = 1.0.0 (不报错)

## 11. Pytorch自带的mnist数据集读取库 由于Pytorch版本的坑

Pytorch = 1.7.1 (指定到有mnist的目录)

Pytorch = 1.0.0 (指定到有raw的目录)

