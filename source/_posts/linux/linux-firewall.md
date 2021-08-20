---
title: RedHat 防火墙
catagories:
- Linux
tags:
- firewall
---

# 查看状态

```
systemctl status firewalld
```

- 开启防火墙
- 开机启动

```shell
systemctl start firewalld
systemctl enable firewalld
```

# 端口设置

查看端口状态

```shell
firewall-cmd --zone=public --list-ports
```

## 开放端口

开启22端口

```sh
firewall-cmd --zone=public --add-port=22/tcp --permanent
```

批量开放

```sh
firewall-cmd --zone=public --add-port=100-500/tcp --permanent
```

载入防火墙设置

```sh
firewall-cmd --reload
```

## 关闭端口

关闭22端口

```sh
firewall-cmd --zone=public --remove-port=22/tcp --permanent
```

载入防火墙设置

```sh
firewall-cmd --reload
```

