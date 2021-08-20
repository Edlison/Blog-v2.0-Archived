---
title: Convolutional Neural Network
categories:
- Deeplearning
tags:
- conv
---

# Convolutional Layer

There are many conv_kernels in a Conv layer. In my perspective, one paticular kernel's size is [kernel_size, kernel_size, in_channel]. The output of the calculation computed by the kernel is sum of all channel's results. So you need to define filter's num, which determine the output feature map.



Parameters: filters, kernel_size, stride, padding

# Pooling Layer

Usually a Pooling layer is after a Conv layer. It is a common use to do downsampling by calculate the value in filter of Pooling.

There are 3 method in common use in Pooling layer, MaxPooling, MinPooling, AvgPooling.

