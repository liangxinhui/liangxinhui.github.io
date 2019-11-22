---
title: Docker 镜像压缩导出
date: 2019-11-19 19:20:37
tags:
  - docker
  - gzip
---

Docker默认的`image save` 功能是`tar`格式的，没有经过压缩。

如果需要在多个节点或网络环境传输的话，可以用`gzip`压缩一下。具体操作方法为：

```bash
# 保存
docker image save <your_image> | gzip > image_file.tar.gz

# 导入(可直接导入tar.gz的包，不需要先解压)
docker image load < image_file.tar.gz
```

