---
date: 2016-11-15 19:32
status: public
title: 'CentOS 6.5 不升级系统库的情况下运行 高版本gcc编译的程序(c++11)'
---

## 问题背景

目标生产环境
- CentOS 6.5 64bit
- gcc 4.4.7
  
想使用`C++11`的新特性，而该环境默认的`gcc 4.4.7`不支持。

开发环境可以升级`gcc`。

本文讨论在不升级生产环境`gcc`的情况下，如何编译程序，以便在该环境下使用`C++11`新特性编译好的程序。

## 解决方案

**要点**：

- 在同一内核和操作系统，高版本gcc下编译程序
- 编译时将高版本的`libstdc++.so.6`库和可执行程序进行绑定
- 将编译好的程序发布到目标系统运行
 
**编译时，有两种方案**：
- 方案A（动态绑定）：
    - 链接时，指定`rpath`参数
    - 把`libstdc++.so.6`随可执行程序发布
    - 此种方案，运行时会从指定的`rpath`路径加载`libstdc++.so.6`
- 方案B（静态绑定）：
    - 链接时，添加参数`-static-libgcc  -static-libstdc++`
    - 此种方案，运行时不依赖`libstdc++.so.6`


## 走过的弯路

尝试解决此问题时，走了以下弯路，供参考：

**刚开始，没有升级CentOS 6.5 的gcc，而是选在 Ubuntu 14.04 64bit（gcc 4.8.4）上编译。**

- 该环境编译好的程序，如果直接在目标环境上运行，会有如下错误：
```bash
/lib64/libc.so.6: version 'GLIBC_2.17' not found
/lib64/libc.so.6: version 'GLIBC_2.14' not found 
/usr/lib64/libstdc++.so.6: version 'GLIBCXX_3.4.14' not found
/usr/lib64/libstdc++.so.6: version 'GLIBCXX_3.4.19' not found
```

- 尝试使用`-static-libgcc -static-stdlibc++`（静态绑定）方法执行后，依然有`libc.so.6`的版本问题。
```bash
/lib64/libc.so.6: version 'GLIBC_2.17' not found
/lib64/libc.so.6: version 'GLIBC_2.14' not found 
```

- 使用`rpath`（动态绑定）方法，同时将`ubuntu`下的`libc.so.6`和`libstdc++.so.6`都放入生产环境，此时程序直接 **coredump**，挂在`libc.so`中：
```bash
Program received signal SIGSEGV, Segmentation fault.
_dl_close_worker (map=0x7ffff7865132) at dl-close.c:113
113	  --map->l_direct_opencount;
(gdb) bt
#0  _dl_close_worker (map=0x7ffff7865132) at dl-close.c:113
#1  0x00000039c961496e in _dl_close (_map=0x7ffff7865132) at dl-close.c:757
#2  0x00007ffff781f58d in ?? () from /home/esunny/lib/libc.so.6
#3  0x00007ffff770ad67 in ?? () from /home/esunny/lib/libc.so.6
#4  0x00000039c960e705 in call_init (...) at dl-init.c:70
#5  _dl_init (...) at dl-init.c:134
#6  0x00000039c9600b3a in _dl_start_user () from /lib64/ld-linux-x86-64.so.2
#7  0x0000000000000001 in ?? ()
#8  0x00007fffffffe7a2 in ?? ()
#9  0x0000000000000000 in ?? ()

```
- 在同事的提醒下，尝试在CentOS下升级gcc至高版本后测试，程序正常运行。
- 原因分析：
  - CentOS升级完gcc之后，`libc.so`没有变。
  - 这样，编译的时候，还是用的低版本`libc.so`
  - 到生产环境，就没有`libc.so`的兼容性问题了

## 经验教训

对linux系统底层运行库了解不够详细。未考虑到不同平台编译时对`libc.so`的依赖有差别。