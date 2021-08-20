---
title: Base64编码
categories:
- Notebook
tags:
- base64
---

# 作用

可以将二进制的文件转换为文本格式进行传输。

在http协议传输中，传输`bytes`类型的数据会有很多，因此我们可以在传输前对其进行`base64`编码，转化为`str`格式，便于传输。