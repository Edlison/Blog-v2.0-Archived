---
catitle: DockerApp - mariadb
categories:
- Docker
tags:
- mariadb
---

# Docker-compose

```yaml
version: '3.1'

services:
  db:
    image: mariadb
    restart: always
    ports:
      - 3306:3306
    environment:
      MYSQL_ROOT_PASSWORD: example
    volumes:
      - db:/var/lib/mysql
    
  adminer:
    image: adminer
    restart: always
    depends_on:
      - db
    ports:
      - 8080:8080
volumes:
  db:
```

# 配置

`MYSQL_ROOT_PASSWORD`: 这个密码将被设置给root用户

`MYSQL_DATABASE(optional)`: 指定一个数据库在镜像创建之初建立（如果后面指定了用户，则授予这个数据库的权限）

`MYSQL_USER`, `MYSQL_PASSWORD`(optional): 如果需要创建用户，这两个变量都需要

`MYSQL_ALLOW_EMPTY_PASSWORD(optional)`: 允许root用户使用空密码

`MYSQL_RANDOM_ROOT_PASSWORD(optional)`: 给root用户随机生成一个密码

`MYSQL_INITDB_SKIP_TZINFO(optional)`: 时区设置









