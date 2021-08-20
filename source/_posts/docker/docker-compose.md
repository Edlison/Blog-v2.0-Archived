---
title: Docker compose
catagories:
- Docker
tags:
- compose
---

# Docker-Compose

# 安装

```sh
sudo curl -L "https://github.com/docker/compose/releases/download/1.28.4/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
```

有可能会遇到权限不足的问题

```sh
chmod +x /usr/local/bin/docker-compose
```

如果不在环境路径下 创建软连接

```sh
ln -s /usr/bin /usr/local/bin/docker-compose
```

# 运行指令

`docker-compose`指令**默认执行**当前文件夹下docker-compose.yml文件

## 项目运行 

```sh
sudo docker-compose -f app.yml up -d
```

## 项目停止 

```sh
sudo docker-compose -f app.yml down
```

## 挂载

`host:container`

volumes声明过的名称才能用在镜像的volumes挂载 
镜像中volumes挂载地址也可以使用相对路径

## 更多指令

https://www.jianshu.com/p/658911a8cff3
