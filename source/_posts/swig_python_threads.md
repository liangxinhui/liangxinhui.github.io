---
title: swig转python 多线程问题
date: 2016-11-03 19:39
tags:
  - SWIG
  - Python
---

SWIG (Simplified Wrapper and Interface Generator) 

当使用wsig将c++转换为python时，可以使用`-threads` 参数。
它可以帮你处理GIL的问题。

例如：
```
swig -c++ -python -threads  example.i
```

参考链接：
http://blog.audio-tk.com/2007/11/23/enabling-thread-support-in-swig-and-python/

解决的问题：
http://stackoverflow.com/questions/9569372/swig-c-python-polymorphism-and-multi-threading