---
title: docker 实践经验
date: 2020-01-15 14:45:44
tags:
  - docker
---

# 安装部署

联网环境下，可使用阿里云镜像，方法参照：
https://liangxinhui.tech/2019/07/18/docker-install-aliyun/

离线安装，可用二进制包，直接解压即可使用。

# Dockerfile

Dockerfile 作为构建 Docker 镜像的描述文件。

<!-- more -->

## 几个概念

### ADD vs COPY

这两个命令都有向镜像中添加文件的功能。

参考：[Dockerfile 中的 COPY 与 ADD 命令](https://www.cnblogs.com/sparkdev/p/9573248.html)

COPY 和 ADD 共同点：

- 不能拷贝上下文之外的本地文件
- 只复制目录中的内容而不包含目录自身
- 目标地址以`/`结尾时，表示目录。否则为文件

区别：

- COPY：拷贝文件或目录到容器中。multistage 构建时可以使用 COPY 命令把前一阶段构建的产物拷贝到另一个镜像中。
- ADD：除了不能用在 multistage 的场景下，ADD 命令可以完成 COPY 命令的所有功能，另外可以：
  - 解压压缩文件并把它们添加到镜像中
  - 从 url 拷贝文件到镜像中（不推荐）

**docker 官方建议我们当需要从远程复制文件时，最好使用 curl 或 wget 命令来代替 ADD 命令。原因是，当使用 ADD 命令时，会创建更多的镜像层，当然镜像的 size 也会更大。**

综合来说，COPY 更简单易懂，需要解压文件时可以使用 ADD。

### CMD vs ENTRYPOINT

这两个参数都可以指定 Docker 容器的启动命令。

参考：[【docker】CMD ENTRYPOINT 区别 终极解读！](https://blog.csdn.net/u010900754/article/details/78526443)

- ENTRYPOINT：容器入口。参数可被 CMD 内容覆盖。
- CMD: 整个命令可被 RUN 启动时很方便的替换

使用建议：

- 最灵活使用：CMD，可以在运行时随意修改
- 限制启动命令，只改参数：ENTRYPOINT + CMD

### WORKDIR

WORKDIR 命令为后续的 RUN、CMD、COPY、ADD 等命令配置工作目录。
也是容器运行的工作目录。

## 示例 Dockerfile

- 基于 Ubuntu 18.04
- 安装 python3
- 阿里云 镜像源
- 阿里云 pip 源
- 常用基础工具（vim、ip、curl 等）
- jupyter notebook

```dockerfile
FROM ubuntu:18.04

RUN sed -i 's/archive\.ubuntu\.com/mirrors.aliyun.com/g' /etc/apt/sources.list

RUN apt-get update && \
	apt-get install  -yq  --no-install-recommends \
	build-essential wget ca-certificates python3 python3-dev default-libmysqlclient-dev libssl-dev && \
	apt-get clean && \
	rm -rf /var/lib/apt/lists/* && \
	ln -sf /usr/bin/python3 /usr/bin/python

RUN wget -O get-pip.py  https://bootstrap.pypa.io/get-pip.py  && python get-pip.py && rm get-pip.py

# pip CN mirror
RUN mkdir -p ~/.pip && \
	echo "[global]" > ~/.pip/pip.conf && \
	echo "trusted-host=mirrors.aliyun.com" >> ~/.pip/pip.conf && \
	echo "index-url=https://mirrors.aliyun.com/pypi/simple/"  >> ~/.pip/pip.conf

# development basic tool
RUN apt-get update && \
	apt-get install  -yq  --no-install-recommends \
	vim curl iproute2 iputils-ping && \
	apt-get clean

# notebook
RUN pip3 install jupyter
COPY .jupyter/ /root/.jupyter/
WORKDIR /notebook
EXPOSE 8888
CMD [ "jupyter", "notebook" ]
```

参考手册：
https://docs.docker.com/reference/
