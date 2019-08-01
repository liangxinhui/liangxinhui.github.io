---
title: Github 修改hosts加速国内访问
date: 2019-07-31 08:35:20
tags:
  - Github
  - 加速
---

在国内访问`Github`网站，特别是`git clone`时，非常慢。

如果有梯子，会好一些。

这里介绍一个不需要梯子的办法: 通过修改本地 `hosts` 文件，达到加速的目的。

实测，可以明显提升下载速度（单位网络限速 10 Mbps，`git clone` 能满速 `900KB/s` 以上)。

<!-- more -->


打开`hosts`文件：

- Windows: `C:\Windows\System32\drivers\etc\hosts` （需要管理员权限）
- Linux: `/etc/hosts`

添加以下内容：

```
192.30.253.112 github.com
192.30.253.113 github.com
151.101.184.133 assets-cdn.github.com
151.101.185.194 github.global.ssl.fastly.net
```

添加完毕之后，刷新dns缓存：

- Windows: `ipconfig /flushdns`
- Linux: 不需要



上边域名对应的IP地址，可通过 [www.ipaddress.com](https://www.ipaddress.com/) 查询得到。