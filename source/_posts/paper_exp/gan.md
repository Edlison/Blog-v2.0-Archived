---
title: GAN
categories:
- Paper
tags:
- gan
---

# 图片Img数组的取值范围及含义

Loader返回的值形式为[batch, channel, row, col]. (N * C * H * W)

channel：通道数，**通道的维度上就保存了各个通道上的值**，如RGB。

torchvision.transforms中的`.toTensor()`就是将图片的[0, 255]范围转换为了Tensor上的[0.0, 1.0]。因此mnist数据集中每个像素点上的值在[0, 1]区间上。

# G 与 D 进行两次前向传播

**两次的参数均独立生成。**

```python
# G的前向传播
noise = torch.autograd.Variable(torch.randn(imgs_batch, self.latent_dim)).cuda()
gen_imgs = self.G(noise)
d_fake = self.D(gen_imgs)
g_loss = sef.bce_loss(d_fake, real_label)
# 训练G
...
# D的前向传播
gen_imgs = self.G(noise)
d_fake = self.D(gen_imgs)
d_real = self.D(real_imgs)
d_loss_fake = self.bce_loss(d_fake, fake_label)
d_loss_real = self.bce_loss(d_real, real_label)
d_loss = d_loss_fake + d_loss_real
# 训练D
...
```

**注意：**

每一次进行前向计算的参数都是独立的参数，在生成计算图时都是独立的。因此，如果计算后一个网络的时候还传的是一开始的`gen_imgs`其实没有变化，指向的还是同一个参数。但是如果再次让`noise`经过G生成一个`gen_imgs`，因为G已经更新过了，此时的`gen_imgs`也发生了变化，因此再进行前向传播，生成的就是另外一个计算图了。

因此这种情况根本不需要再第一次反向传播的时候设置`retain_graph=True`.

# G 与 D 只进行一次前向传播

**共用一次前向计算的参数。**

```python
# 参数只进行一次前向传播
noise = torch.autograd.Variable(torch.randn(imgs_batch, self.latent_dim)).cuda()
gen_imgs = self.G(noise)
d_fake = self.D(gen_imgs)
d_real = self.D(real_imgs)

g_loss = self.bce_loss(d_fake, real_label)
d_loss_fake = self.bce_loss(d_fake, fake_label)
d_loss_real = self.bce_loss(d_real, real_label)
d_loss = d_loss_fake + d_loss_real
# 同时训练
```



## 方法一（错误的）

两个网络**同时训练**的参数的优化器同时梯度清零->损失反向传播->梯度下降，此时的网络无法得到优化。而且此时因为两个loss的反向传播有重合的部分，必须要`retain_graph=True`。

```python
self.optimizer_G.zero_grad()
self.optimizer_D.zero_grad()
g_loss.backward(retain_graph=True)
d_loss.backward()
self.optimizer_G.step()
self.optimizer_D.step()
```

**注意：**

此时根本不会对网络有优化的作用，并没有产生‘对抗’的效果。

## 方法二

两个网络**先后训练**，先优化G，后优化D。但是此时loss反向传播时，仍然会有参数重复计算梯度且没有更新的部分（网络D中的参数）。必须要在先进行反向传播的loss加上`retain_graph=True`.

```python
self.optimizer_G.zero_grad()
g_loss.backward(retain_graph=True)
self.optimizer_G.step()

self.optimizer_D.zero_grad()
d_loss.backward()
self.optimizer_D.step()
```

**注意：**

这种方法更注重的是**二次**求导，也就是原计算图保存的是`g_loss`对网络D中参数的梯度，然后叠加上`d_loss`对网络D中参数的梯度。一并对网络D的参数进行更新。

# Conv2d

```python
nn.Conv2d(in_channels, out_channels, kernel_size, stride, padding)
```

in_channels: 输入通道数。

out_channels: 输出通道数。

kernel_size: 卷积核大小。（如果长宽不一样，传一个tuple）

stride: 卷积核移动步长。

padding: 图片上下左右的填充。

输入的shape应该是：[batch_size, channels, height, weight]

# 思考

GAN这种结构最大的问题就是虽然**可以生成很相似的东西**，但是如果生成的东西有很强的类别属性，**无法指定生成的类别**。

Optimizer**优化器就是训练网络**，虽然进入网络的样本也被装入了`Variable`计算图中，其实际上不会被更新，如果没有设置`requires_grad=False`本质上还是会被计算梯度的，因此对非网络中的参数指定不计算梯度，可以提升运行效率。

`retain_graph`实质上是**对计算图进行保留**，由于训练基本上是不断生成一个计算图然后反向传播，默认在反向传播后就抛弃掉了这个计算图结构。

只进行一次前向计算就是**同时训练两个网络**（训练网络D并不是在更新了网络G的基础上）；进行两次前向计算就是**分别训练两个网络**（训练网络D是在网络G更新的基础上）。

保留计算图或不保留计算图的本质就在于，对网络D的更新是否**叠加**上了网络G更新时反向传播的`g_loss`对网络D中参数的梯度。（**不是计算二阶导数！**）

网络的最后一层要在[0, 1]上，由于没有用卷积，在数字意外部分多还是噪声的影响大，因此造成了数字以外区域黑白。

batch大小的调整直接影响到了权重的迭代。（问题就出在batch=10000忘记调回来了！！！）：很大的一个作用是可以在一个epoch上迭代很多次！！！

# 问题

shape操作 reshape, view, squeeze, flatten...

共享内存 如(a.view(-1, 1)/ b = a.view(-1, 1)) a内存中的tensor都会变吗，还是只有b拿到了？inplace



----

Reference

https://www.zhihu.com/search?q=batch%20size&utm_content=search_suggestion&type=content

https://blog.csdn.net/weixin_43202635/article/details/84204180



