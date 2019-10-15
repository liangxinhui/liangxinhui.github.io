---
title: Ubuntu 普通用户增加 sudo 权限（不修改 sudoers 文件）
date: 2019-10-15 11:20:49
tags: 
  - Ubuntu
---

# 问题

Ubuntu下，默认创建的用户可以通过sudo执行高级权限的动作。

新创建的用户，如果执行sudo的话，会提示：

> username is not in the sudoers file.  This incident will be reported.

# 解决方案

推荐解决办法：**添加用户到sudo组**

```bash
# 用  root 或者有 sudo 权限的用户执行
sudo usermod -aG sudo <username>
```

**注意事项：**
- 需要重新登录用户后生效

# 原因分析

查看 /etc/sudoers 文件，可以看到：

**sudo 用户组内的用户可以执行任何命令**

```bash
# Allow members of group sudo to execute any command
%sudo	ALL=(ALL:ALL) ALL
```

全文：

```
#
# This file MUST be edited with the 'visudo' command as root.
#
# Please consider adding local content in /etc/sudoers.d/ instead of
# directly modifying this file.
#
# See the man page for details on how to write a sudoers file.
#
Defaults	env_reset
Defaults	mail_badpass
Defaults	secure_path="/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/snap/bin"

# Host alias specification

# User alias specification

# Cmnd alias specification

# User privilege specification
root	ALL=(ALL:ALL) ALL

# Members of the admin group may gain root privileges
%admin ALL=(ALL) ALL

# Allow members of group sudo to execute any command
%sudo	ALL=(ALL:ALL) ALL

# See sudoers(5) for more information on "#include" directives:

#includedir /etc/sudoers.d
```



# 扩展阅读
网上常见的方案是：

[Ubuntu报“xxx is not in the sudoers file.This incident will be reported” 错误解决方法](https://www.linuxidc.com/Linux/2016-07/133066.htm)

需要一系列步骤：
> 1.切换到root用户下
> 2.修改 /etc/sudoers权限或者使用visudo
> 3.编辑sudoers文件
> 4.撤销sudoers文件写权限


# 结论

相较于修改 /etc/sudoers 方案，添加到组的方案更简单易用。


