---
title: pytorch模型的保存与加载
categories:
- Paper
tags:
- python
- pytorch
---

# 只保存网络的参数

## Save

```python
torch.save(model.state_dict(), PATH)
```

## Load

```python
model.load_state_dict(torch.load(PATH))
```

**注意：**

- 模型的加载需要使用`torch.load`加载路径下的文件，逆序列化dict

# 保存全部的信息

## Save

```python
torch.save({
  'epoch': epoch,
  'model_state_dict': model.state_dict(),
  ...
}, PATH)
```

## Load

```python
checkpoint = torch.load(PATH)
epoch = checkpoint['epoch']
model.load_state_dict(checkpoint['model_state_dict'])
```







----

Reference

https://blog.csdn.net/dss_dssssd/article/details/89409183