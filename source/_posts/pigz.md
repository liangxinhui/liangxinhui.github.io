---
title: 使用 pigz 并行加速 Linux 压缩与解压
date: 2019-11-13 09:12:05
tags:
  - linux
  - pigz
  - 效率
---

Linux下压缩或解压一个文件，通常使用`tar.gz`格式，对应的工具为 `tar`(打包工具)以及由`-z`参数指定的`gzip`（压缩解压工具）。

而默认的`gzip`工具是单线程工作的，在处理大文件时非常慢。

可以通过`-I`参数指定自定义的`并行（多进程、多核）`压缩解压工具 `pigz` 来进行加速。

`pigz`的[官方](https://zlib.net/pigz/)自我介绍：
> A parallel implementation of gzip for modern multi-processor, multi-core machines

# 使用方法

- 安装 `pigz` 工具包（yum 或 apt)
- 将常规参数中的`-z`去掉，换上`-I`

``` bash
# 压缩
tar -I pigz -cvf package.tar.gz /path/to/file_or_folder
# 解压 (中括号内为可选，用于指定解压缩目标地址)
tar -I pigz -xvf package.tar.gz [-C  /path/to/extracted]
```

# 原理解析
<!--more-->
tar 通常的用法为：

``` bash
# 压缩
tar -czvf package.tar.gz /path/to/file_or_folder
# 解压 (中括号内为可选，用于指定解压缩目标地址)
tar -xzvf package.tar.gz [-C  /path/to/extracted]
```

我们来看一下`tar --help`返回的帮助信息中这几个参数的含义：
<pre>
  -c, --create                     create a new archive
  -x, --extract, --get             extract files from an archive
  -z, --gzip, --gunzip, --ungzip   filter the archive through gzip
  -f, --file=ARCHIVE               use archive file or device ARCHIVE
  -C, --directory=DIR              change to directory DIR
</pre>

以及另外一个我们今天使用的参数：
<pre>
  -I, --use-compress-program=PROG  filter through PROG (must accept -d)
</pre>

通过以上信息，我们可以知道：
- `-z` 的意思是使用 gzip 进行压缩或解压
- `-I` 可以自定义你使用的压缩解压工具

# 相关信息
网上也有其他一些方案，例如管道组合之类的,不太好记。这里推荐`-I`参数的方法。

其他用法及对应的性能测试的效果，参考：
- [unpigz-and-untar-to-a-specific-directory](https://unix.stackexchange.com/questions/198958/unpigz-and-untar-to-a-specific-directory)
- [Linux并行gzip压缩工具pigz-冰封飞飞](https://www.jianshu.com/p/4e69716804a5)
- [tar 使用pigz多线程打包-明天的明abc](https://www.jianshu.com/p/7d956a21f0ab)