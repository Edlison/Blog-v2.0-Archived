---
title: Github慢
catagories:
- Notebook
tags:
- github
---

# Ubuntu上修改Hosts有问题

`service networking restart`

修改hosts的方法在腾讯云上不行，没有network服务？

# MacOS修改Host

去[DNS](http://tool.chinaz.com/dns)查找最小TTL的响应IP

修改`/etc/hosts`

刷新DNS缓存

```
dscacheutil -flushcache
```

# 备用方案

将仓库的地址手动改为github的镜像地址再进行`clone`.

`https://github.com.cnpmjs.org/`

