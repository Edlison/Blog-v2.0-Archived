---
title: Python的__init__.py文件
categories:
- Notebook
tags:
- python
---

# init.py文件的主要作用

用来管理当前目录下的`.py`文件。

1. `__init__.py`是package的标识。
2. 其中`__all__`可以用来进行模糊导入(`import *`)。

使用`import`导入包的时候，就相当于导入了这个包的`__init__.py`。

# import的理解

可以导入**包**或**文件**或**变量**。

# from的理解

只能**包**或**文件**。



----

Reference

https://www.cnblogs.com/BlueSkyyj/p/9415087.html