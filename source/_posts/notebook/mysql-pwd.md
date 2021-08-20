---
title: Mysql改密码
catagories:
- Notebook
tags:
- Mysql
---

# 进入Mysql后Set修改

```mysql
set password for root@localhost = password('abc');
```

# 用mysqladmin指令

```shell
mysqladmin -uroot -p123 password abc
```

把root的密码从123 改为 abc

# 用update直接更新

```mysql
use mysql;
update user set password=password('abc') where user='root' and host='localhost';
flush privileges;
```







----

Reference

https://www.w3cschool.cn/mysql/mysql-wauz2owl.html