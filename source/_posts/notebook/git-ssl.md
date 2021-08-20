---
title: Github push 发生SSL错误
categories:
- Notebook
tags:
- git
---

# 问题描述

`LibreSSL SSL_connect: SSL_ERROR_SYSCALL in connection to github.com:443`

# 解决方案

`git config --global http.sslBackend "openssl" `

配置`~/.gitconfig`

```
[http]
        sslVerify = false
[https]
        sslVerify = false
```

或者直接输入指令

```shell
git config --global http.sslVerify false
git config --global https.sslVerify false
```

