---
title: gitignore不起作用
catagories:
- Notebook
tags:
- git
---

# 原因

如果项目不是在一开始添加了`.gitignore`文件来管理仓库的忽略规则的话，就会出现不起作用的情况。

# 解决

1. 清除本地当前的Git缓存

```
git rm -r --cached .
```

2. 应用.gitignore等本地配置文件重新建立Git索引

```
git add .
```

3. 提交当前Git版本

```
git cm -m 'update'
```







----

Reference

https://zhuanlan.zhihu.com/p/334908553