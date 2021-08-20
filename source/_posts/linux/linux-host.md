---
title: Linux hosts
catagories:
- Linux
tags:
- ubuntu
- host
---

# 用途

hosts文件在目录`/etc`，文件负责**IP地址与域名快速解析**。

优先级：dns缓存 > hosts > dns服务

# 配置

## 格式

网络IP地址  主机名或域名  主机名别名

## 例子

```
127.0.0.1 localhost.localdomain localhost
```

# Hostname

不能直接更改hostname

https://blog.csdn.net/zhengchaooo/article/details/83177335







----

Reference

https://www.linuxidc.com/Linux/2016-10/135886.htm