---
title: Python 添加系统路径
categories:
- Notebook
tags:
- python
- sys
---

# 报错'No Module'

没有讲目录添加到`sys path`下，导致python没有扫描到。

```python
import sys
sys.path.append('/home/xxx/xxx')
```

