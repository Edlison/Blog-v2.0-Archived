---
title: Teacher Forcing Mechanism
categories:
- Deep Learning
tags:
- rnn
---

# Drawback

The prediction ability about next word in RNN is weak at beginning.

# Method

By introducing the teacher forcing mechanism, assign the input of the decoder with ground truth under teacher forcing. And feed the prediction of decoder under free running.



----

Reference

https://blog.csdn.net/qq_30219017/article/details/89090690