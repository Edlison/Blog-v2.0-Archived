---
title: logits的含义
categories:
- Paper
---

logits一般是全连接层的输出，不必关心其值在[0, 1]之间，或者相加为1。

但是logits经过Softmax层后又很容易变为表示概率的值。





----

Reference

https://www.zhihu.com/question/60751553