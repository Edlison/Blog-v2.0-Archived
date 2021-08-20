---
title: Python原生高阶用法
catagories:
- Paper
tags:
- python
- buildin
---

# new

静态方法，传入`cls`类，返回`self`实例。在`__init__`之前调用。

# init

接收`__new__`返回的实例，对实例进行初始化。

# call

使实例化后的对象变成了`callable`可以调用的对象（）。同时

```python
class Hello:
	def __call__(self, arg):
  	print('hello' + arg)
    
  def hello():
    print('hello word')
h = Hello()
h('world')
# hello world
h.hello.__call__()
# hello world
```

# getitem

`getitem`可以直接对对象进行取值。

```python
def __getitem__(self, idx):
  return self.data[idx]
```

# 反射

Python也有**反射**，即可以通过字符串的形式操作实例的属性。

`__hasattr__`, `__getattr__`, `__setattr__`, `__delattr__`

# 类变量

```python
class C:
  var_class = 0
  def __init__(self):
    self.var_instance = 1
```

类似静态成员。

----

Reference

https://www.jianshu.com/p/3aca78a84def