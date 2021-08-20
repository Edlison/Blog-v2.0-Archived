---
title: Druid连接池加密
catagories:
- Notebook
tags:
- java
- springboot
---

# 数据库连接加密

项目有需求连接数据库的信息需要加密。

刚好Druid可以通过拦截器使用秘文和公钥将密码还原成真实的密码。

# 坑

## 问题

durid过滤器一直没起作用

## 解决方案

由于项目配置文件里有区分主从数据库，因此在对主数据库的加密解密要写在`spring.datasource.druid.master`配置下。不能配置在`spring.datasource.druid`下。





----

Reference

https://www.cnblogs.com/vipstone/p/14467140.html