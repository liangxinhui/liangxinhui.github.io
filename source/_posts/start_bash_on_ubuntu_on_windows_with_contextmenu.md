---
date: 2016-11-18 08:03
status: public
title: '右键菜单启动 Bash on Ubuntu on Windows '
tags:
  - Windows
  - 右键菜单
---

Windows 10 在 Red Stone 版本里开始发布的 `Bash on Ubuntu on Windows` 对于开发者来说，是一个很好的工具。
- 可以直接使用很多linux下的命令（grep, awk, tail 等等）
- 可以直接再Windows中安装Linux的各种软件和服务
- 开发阶段可以直接在Windows下编译Linux程序（和VS Studio的新插件绝配）
<!-- more -->
在使用中，感觉不方便的一个地方，就是不能在任意位置启动bash窗口。
如果遇到类似的困扰，可以修改注册表，添加右键菜单来实现。
将以下脚本存为文本文件，后缀改为reg，双击导入即可：
```
Windows Registry Editor Version 5.00
[HKEY_CLASSES_ROOT\Directory\Background\shell\cmdbash]
@="Open Ubuntu Bash here"
[HKEY_CLASSES_ROOT\Directory\Background\shell\cmdbash\command]
@="bash.exe"
```