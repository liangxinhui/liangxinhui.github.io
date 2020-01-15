---
title: Linux Shell 编程速查表
date: 2019-12-26 11:27:52
tags:
  - linux
  - shell
  - cheatsheet
---

持续更新..

整理记录常用 shell 代码段。

# 基础变量

```bash
# 获取当前脚本所在目录
SHELL_FOLDER=$(dirname $(readlink -f "$0"))

# 获取当前时间 20200123112233
DATETIME=$(date +%Y%m%d%H%M%S)
# 获取当前时间 2020-01-23 11:22:33
DATETIME=$(date +%Y-%m-%d %H:%M:%S)

```

# 语法示例

```bash
# 判断文件夹是否存在
if [ -d /path/to/folder ]; then
    # do somthing if the folder exists
fi
# 判断文件是否存在
if [ -f /path/to/file ]; then
    # do somthing if the file exists
fi
```

# 实用函数封装

```bash
function is_cmd_exist() {
    return $(type $1 &>/dev/null)
}


```
