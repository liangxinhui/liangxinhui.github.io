---
date: 2015-11-05 08:27
status: public
title: 'Linux下准备Node.js环境（Linux Binaries版）'
tags: 
  - node
  - 安装
---

在Node.js的[官方下载网站](https://nodejs.org/en/download/)上，有`Linux Binaries(.tar.gz)`的包可供下载，但是却没有说明文档。
这里整理了`Linux Binaries(.tar.gz)`的一份安装说明。
<!-- more -->
以`Ubuntu`为例，其他Linux环境类似：
# 目标
- 可运行Node.js
- 针对国内网络环境，添加[淘宝cnpm](http://npm.taobao.org/)的支持
- 添加[nodemon]()，以便调整代码后自动重启
- 添加开机自启动

# 基本环境部署
## 获取并解压Node.js二进制文件包
```bash
# 进入安装包目标路径,根据个人喜好放置
cd /home/software
# 获取安装包
wget https://nodejs.org/dist/v4.2.2/node-v4.2.2-linux-x64.tar.gz
# 解压
tar xvzf node-v4.2.2-linux-x64.tar.gz
```

## 设置环境变量
这样，并不能在系统任意位置执行`node`或者`npm`.
需要将路径添加到`PATH`中。
编辑`/etc/profile`，修改`PATH`环境变量：
```bash
export PATH=$PATH:/home/software/node-v4.2.2-linux-x64/bin
```
这样，除了`node`主程序和自带的`npm`，每次通过`npm -g`安装的包，一般都能用非sudo访问了。

添加sudo对npm的访问：
```bash
sudo ln -s /home/software/node-v4.2.2-linux-x64/bin/npm /usr/local/bin/
```

## 安装cnpm
```bash
sudo npm install -g cnpm --registry=https://registry.npm.taobao.org
sudo ln -s /home/software/node-v4.2.2-linux-x64/bin/npm /usr/local/bin/
```
之后就可以用cnpm代替npm进行包管理了，速度会快很多。

## 安装nodemon
```bash
sudo cnpm install -g nodemon
```
由于已经设置过环境变量，此时可以直接运行`nodemon`了。

# 自启动相关
一般会把自启动命令添加到`/etc/rc.local`中，这样，系统重启时就会运行。
但是之前在`/etc/profile`中设置的环境变量，在`/etc/rc.local`中并不起作用。
我的做法是，在`/usr/local/bin`中添加`node`、`nodemon`这些程序的`软链接`:
```bash
sudo ln -s /home/software/node-v4.2.2-linux-x64/bin/node /usr/local/bin/
sudo ln -s /home/software/node-v4.2.2-linux-x64/bin/nodemon /usr/local/bin/
```
然后，在`/etc/rc.local`中便可以设置开机自启动了：
```bash
# 进入要启动程序的目录
cd /home/ubuntu/workspace/test
# 获取当前日期，格式为:20151105
DATE=$(date +%Y%m%d)
# 启动程序，并将日志计入当天日期的文件：log_20151105.txt
sudo nodemon app.js >> log_${DATE}.txt &
# 恢复执行目录
cd -
```