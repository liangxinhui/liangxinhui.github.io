---
title: Windows10下CorelDRAW9保存窗口文件名输入框完整显示补丁
date: 2019-08-28 22:57:42
tags:
  - CorelDRAW
  - Windows
---

CorelDRAW 9 在 Windows10下运行时，保存窗口被遮盖。

网友提供了详细的解决方法：

[CDR9在WIN10保存完整对话框](https://wenku.baidu.com/view/5f98985bff4733687e21af45b307e87101f6f8cd)

主要思路为：**通过`PE Explorer`修改`drawintl.dll`里的`保存`和`另存为`对话框（Dialog）资源文件。**


附修改过的文件：
> 链接：https://pan.baidu.com/s/1iTEXSpT7m0PL6Us0x_7X9Q 
> 提取码：bf2j 