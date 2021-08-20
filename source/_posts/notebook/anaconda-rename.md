---
title: Conda create/delete/rename environments
categories:
- Notebook
tags:
- python
- conda
---

# Create

```shell
conda create -n tensorflow20 python=3.6
```

# Delete

```shell
conda remove -n tensorflow20 --all
```

# Rename

clone and delete

```shell
conda create -n tf20 --clone tensorflow20
conda remove -n tensorflow20 --all
```







---

Reference

https://www.jianshu.com/p/7265011ba3f2