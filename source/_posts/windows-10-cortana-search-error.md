---
title: Windows 10 搜索空白
date: 2019-05-30 21:00:54
tags:
  - Windows
---

Windows 10 开始菜单搜索，会出现空白现象。

主要是在 1803、1809中出现。升级到1903后暂时没有出现。

<!-- more -->

修复方法：

```bash
# powershell 中管理员运行
Get-AppXPackage -Name Microsoft.Windows.Cortana | Foreach {Add-AppxPackage -DisableDevelopmentMode -Register "$($_.InstallLocation)\AppXManifest.xml"}

```