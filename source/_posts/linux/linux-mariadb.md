---
title: Linux上的Mariadb
catagories:
- Linux
tags:
- mariadb
---

# Mariadb的使用

Linux上已经摒弃了Mysql，以下是一些Mariadb的基本用法。

## 创建用户

`CREATE USER 'username'@'host' IDENTIFIED BY 'password';`

## 用户权限

`GRANT ALL privileges ON databasename.tablename TO 'username'@'host'`

## 查看权限

`show grants for 'root'@'%';`

例:
`MariaDB [(none)]> grant all on *.* to 'root'@'%';`
`MariaDB [mysql]> flush privileges;`
`MariaDB [(none)]> grant all privileges on *.* to 'root'@'%' identified by 'password';`

## 查看用户

`select user from mysql.user;`

## 查看当前用户

`select current_user;`

## 删除用户

`delete from mysql.user where user='root';`

## 撤销权限

`REVOKE privileges ON databasename.tablename FROM 'username'@'host';`

# 坑

如果创建外网可以登陆的用户

`create user 'xxx'@'%' identified by 'xxx'`

想用这个用户在本地`localhost`登陆的话会被拒绝访问

再新建一个可以`localhost`登陆的用户即可