---
title: Windows 下的 which 命令 (Get-Command)
date: 2019-07-25 09:39:26
tags:
  - Windows
  - Linux
  - which
  - Get-Command
---


Linux 下的很多工具用起来很方便，比如 cat、grep、awk、sed、which 等等。

其中 which 可以用来查找当前环境中的某个程序的具体路径在哪里。

Windows 下安装过 `Git for Windows` 后，也可以使用这些常用的linux命令。

其实，还有另外一种方法，可以使用 `Powershell` 的 `Get-Command`指令。

例如：
```powershell
PS C:\Users\liangxinhui\Desktop> Get-Command ssh

CommandType     Name     Version    Source
-----------     ----     -------    ------
Application     ssh.exe  0.0.0.0    C:\Program Files\Git\usr\bin\ssh.exe


PS C:\Users\liangxinhui\Desktop> which ssh
/usr/bin/ssh

```
