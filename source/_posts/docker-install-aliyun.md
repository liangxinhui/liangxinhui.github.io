---
title: 国内环境通过阿里云安装Docker
date: 2019-07-18 16:42:23
tags:
  - docker
  - aliyun
  - 镜像
---

国内安装Docker比较慢。可以使用阿里云的镜像。

```bash
curl -fsSL https://get.docker.com | bash -s docker --mirror Aliyun
```

安装好之后，根据提示，添加用户到`docker`组，可以在执行docker指令时不用输入`sudo`.

```bash
sudo usermod -aG docker <your-username>
```

**注意:** 需要重新登录后生效