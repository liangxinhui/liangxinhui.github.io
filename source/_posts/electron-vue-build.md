---
title: electron-vue 构建 windows 应用时的一些问题记录
date: 2019-12-25 09:05:20
tags:
  - electron
---

用 electron-vue 构建 windows 桌面应用，包构建器为 electron-builder 。

在执行 npm run build 时，会出现各种异常，关键点整理如下：

# 不要使用 cnpm 进行安装，改用 yarn

可使用 yarn 国内加速, 配置：

<pre>
yarn config set registry "https://registry.npm.taobao.org"
yarn config set sass_binary_site "https://npm.taobao.org/mirrors/node-sass/"
yarn config set phantomjs_cdnurl "http://cnpmjs.org/downloads"
yarn config set electron_mirror "https://npm.taobao.org/mirrors/electron/"
yarn config set sqlite3_binary_host_mirror "https://foxgis.oss-cn-shanghai.aliyuncs.com/"
yarn config set profiler_binary_host_mirror "https://npm.taobao.org/mirrors/node-inspector/"
yarn config set chromedriver_cdnurl "https://cdn.npm.taobao.org/dist/chromedriver"
</pre>

注意：只添加第一个时，会出现编译 fresh package 卡住的情况，推荐全都添加。

# build 时会出现下载包超时的情况

解决方案：[electron 打包踩过的坑总结](https://segmentfault.com/a/1190000018533945)

大致思路是，根据控制台下载提示，手工下载一些包到对应的目录。

- electron，不需要解压，放在 `AppData\Local\electron\Cache` 下面。
- electron-builder 对应的包，需要解压到 `AppData\Local\electron-builder\cache\<包名>\<包名+版本号>`。

详细目录为：（X.X.X.X 代表版本号）

- AppData\Local\electron\Cache（不需要解压）
  - electron-vX.X.X-win32-x64.zip
  - SHASUMS256.txt-X.X.X
- AppData\Local\electron-builder\cache （有版本号的目录下为解压过，包含 exe 的目录）
  - winCoseSign
    - winCodeSign-X.X.X
  - nsis
    - nsis-X.X.X.X
    - nsis-resources-X.X.X.X
