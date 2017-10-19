---
date: 2016-11-22 18:11
status: public
title: CentOS打包rpm及其依赖用于离线安装
---

# 使用场景
在某些封闭开发环境或生产环境，可能没有特定的yum源。此时，安装复杂依赖的rpm是一个比较头疼的问题，需要将一个个rpm包进行下载安装。
这里整理了一下可行的两个方案：
- downloadonly插件
- yum cache

# 解决方案
以CentOS 6.5下安装devtoolset-3及其依赖为例。设置好devtoolset-3的repo源之后，按以下方法操作：
## 1.获取离线rpm包
### 方法A：downloadonly插件
该方法利用yum的downloadonly插件，将所需要的依赖仅下载到本地目录，而不安装。
尽在需要安装的目标主机中安装。

1.安装downloadonly插件
```bash
yum install yum-plugin-downloadonly
```
2.下载rpm及其依赖

```bash
yum install --downloadonly  --downloaddir=/root/myrpms/ devtoolset-3-gcc devtoolset-3-gcc-c++
```
**`downloaddir`参数指定了rpm包的输出路径**

### 方法B：开启yum cache
yum默认是不保存下载的rpm包的，如果需要，可以开启。
0. 开启yum的cache功能
修改`/etc/yum.conf`，将`keepcache`设置为`1`.
保存路径在`/etc/yum.conf`中可通过`cachedir`配置，默认为`/var/cache/yum/$basearch/$releasever`
0. 按常规方法yum，安装 
``` bash
yum install devtoolset-3-gcc devtoolset-3-gcc-c++
```

###　获取离线包方法对比
- downloadonly插件
  - 优点：下载主机可以不用安装要下载的包
  - 缺点：需要提前一次性安装yum插件
- yum cache
  - 优点：不需要额外的插件
  - 缺点：修改配置后如果不改回来，占用本地空间；本地需要安装下载的rpm包

## 2.离线安装
将rpm包及其依赖包拷贝到目标主机并进入rpm目录。执行以下命令离线安装：
```bash
yum localinstall *
```
**注意：**
- 这里不能指定具体的rpm包名，否则yum还会从系统配置的repo库中查找依赖，重新下载。
- 建议将需要安装的包及其依赖都放在一个目录下，然后用`yum localinstall *`命令安装，这时，会自动安排依赖，依次安装。

# 参考
- [利用yum下载软件包的三种方法](http://297020555.blog.51cto.com/1396304/530703)
- [解决Linux 软件包的依赖关系](http://blog.csdn.net/wenwenxiong/article/details/51746231)