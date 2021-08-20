---
title: Linux ssh 保持连接
catagories: 
- Notebook
tags: 
- linux
- ssh
---

# Client

From client, you can set this config to hold all the connection with servers.

```
vim ~/.ssh/config
ServerAliveInterval 60
```

# Server

From server, you can hold all the connection with clients.

```
vim /etc/ssh/sshd_config
ClientAliveInterval 60
```

