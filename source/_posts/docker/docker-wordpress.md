---
title: DockerApp - wordpress
categories:
- Notebook
tags:
- docker
- wordpress
---

# Docker-compose


```yaml
version: '3.1'

services:
  wordpress:
    image: wordpress
    restart: always
    depends_on:
      - db
    ports:
      - 80:80
    environment:
      WORDPRESS_DB_HOST: db
      WORDPRESS_DB_USER: user
      WORDPRESS_DB_PASSWORD: password
      WORDPRESS_DB_NAME: wordpress
    volumes:
      - wordpress:/var/www/html

  db:
    image: mysql:5.7
    restart: always
    environment:
      MYSQL_DATABASE: wordpress
      MYSQL_USER: user
      MYSQL_PASSWORD: password
      MYSQL_RANDOM_ROOT_PASSWORD: '1'
    volumes:
      - db:/var/lib/mysql

volumes:
  wordpress:
  db:
```

# 配置

## volumes

挂载形式`host:container`

compose最后声明的volumes**如果不指定目录**会自动创建在`/etc/lib/docker/volumes/example/_data`下

## 注意

`example`=当前文件夹名_compose中声明的volumes的名字

# 启动

```sh
sudo docker-compose -f wordpress.yml up -d
```