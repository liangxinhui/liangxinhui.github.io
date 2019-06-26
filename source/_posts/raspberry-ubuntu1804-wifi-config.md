---
title: 树莓派 Ubuntu 18.04 Wifi 配置
date: 2019-06-18 22:01:07
tags:
  - raspberry
---

ubuntu1804，wifi的配置要通过 netplan 进行。
<!-- more -->

初始化的树莓派网络配置文件为
```yaml
# /etc/netplan/50-cloud-init.yaml
network:
    version: 2
    ethernets:
        eth0:
            dhcp4: true
            match:
                macaddress: <eth0 mac>
            set-name: eth0
```

需要调整为如下内容：
- 增加wikis章节
- eth0设置为 optional，否则：
  - 在没有插网线的情况下，开机会卡主
  - 两个网络在同一个路由下，两个default网关，外部无法通过wifi网络访问

```yaml
# /etc/netplan/50-cloud-init.yaml
network:
    version: 2
    ethernets:
        eth0:
            optional: true
            dhcp4: true
            match:
                macaddress: <eth0 mac>
            set-name: eth0
    wifis:
            wlan0:
                    optional: true
                    access-points:
                            "Wifi-Name":
                                    password: "Wifi-Password"
                    dhcp4: true

```