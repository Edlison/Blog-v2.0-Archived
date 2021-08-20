---
title: Docker Nginx
categories:
- Docker
tags:
- linux
- nginx
---

# docker-compose

```
version: '3.1'

services:
  nginx:
    image: nginx
    volumes:
     - ./html:/usr/share/nginx/html
     - ./conf:/etc/nginx
     - ./logs:/var/log/nginx
    ports:
     - 80:80
    environment:
     - NGINX_HOST=edlison.cc
     - NGINX_PORT=80
```

nginx默认的html文件夹在`/usr/share/nginx/html`

不过可以通过指定root来将根目录换到`/etc/www/html`下

# nginx.conf

```
worker_processes  1;

events {
   accept_mutex on;
   multi_accept on;
   worker_connections 1024;
}

http {

server{
  listen 80;
  server_name  edlison.tk;

  location / {
	root	/usr/share/nginx/html;
	index	index.html
    proxy_pass  http://127.0.0.1:80; # 转发规则
    proxy_set_header Host $proxy_host; # 修改转发请求头，让8080端口的应用可以受到真实的请求
    proxy_set_header X-Real-IP $remote_addr;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
  }
}

}
```



# 问题

由于在域名提供商指定的域名是到IP的80端口

容器需要映射到宿主机的80端口，否则需要在网址后面加`:8080`

**域名提供商**或**服务器商**如果在国内的话，需要网站备案！

