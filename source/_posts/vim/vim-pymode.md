---
title: Vim插件 - Python Mode
categories:
- Vim
tags:
- vim
- python
- linux
---

# 安装

```
Plugin 'python-mode/python-mode'
Plugin 'https://github.com.cnpmjs.org/python-mode/python-mode.git'
```

## 问题

由于repo中有`submodule`导致如果Github被墙的话，其中的包仍然下载不了。

且clone默认是不加载`submodules`的

```
git submodule update --init --recursive
```

## 解决

1. clone时下载

```sh
git clone --recurse-submodules https://github.com/xxx/xxx.git
```

2. 在主项目中执行

```sh
git submodule init
git submodule update
```

