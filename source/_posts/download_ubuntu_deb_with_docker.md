---
title: 通过Docker下载任意版本ubuntu包及完整依赖
date: 2019-07-18 16:59:02
tags:
  - Ubuntu
  - docker
---

# 背景

对于没有互联网环境的研发网，Ubuntu 软件包的离线安装比较麻烦。

虽然可以使用`apt install --download-only`来下载，但是在这种情况下会有问题：

- 安装包有很多依赖
- 外网机器已经安装过其中一些依赖包

此方法不会重新下载已安装的依赖文件。

# 方法
借助docker镜像，解决依赖包下载不全的问题。

# 详细步骤

以 ubuntu 18.04 为例：
<!-- more -->

## 创建并进入工作目录

```bash
mkdir ubuntu_downloader
cd ubuntu_downloader
```

## 创建Dockerfile

文件：`ubuntu_downloader/Dockerfile`

```
# Dockerfile
FROM ubuntu:18.04

RUN sed -i 's/archive\.ubuntu\.com/mirrors.aliyun.com/g' /etc/apt/sources.list && \
    grep aliyun /etc/apt/sources.list > /tmp/sources.list && \
    mv -f /tmp/sources.list /etc/apt/sources.list
```

## 创建下载脚本

文件：`ubuntu_downloader/ubuntu_download.sh`
```bash
#!/bin/bash
base_download_dir=/tmp/ubuntu_download
package_list="$@"
package_dir_name=${package_list//" "/"_"} 
package_dir_full=$base_download_dir/$package_dir_name

mkdir -p $package_dir_full

docker run -v $package_dir_full:/var/cache/apt/archives ubuntu1804_downloader sh -c "apt update && apt install --download-only -y $package_list "
```

## 下载文件

```bash
./ubuntu_download.sh yourpackage1 yourkackage2 ...
```
下载后，文件保存在 `/tmp/ubuntu_download/yourpackage1_yourkackage2` 目录。

## 离线安装

对于依赖关系少的安装包，可以用 `dpkg -i ` 手动安装deb包。

如果依赖关系较多，可自定义本地安装源。

详细可参考：[ubuntu18.04 本地源制作](https://www.cnblogs.com/longchang/p/11088411.html)
