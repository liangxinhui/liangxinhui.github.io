---
title: Ubuntu 按键映射
date: 2019-04-14 21:41:33
tags:
  - Ubuntu
  - XiaoMi
---

小米笔记本默认的按键 `Insert` 需要 `Fn + F12`，在linux终端中，常用 `Shift + Insert` 进行粘贴操作。

需要修改按键映射，交换 `F12` 与 `Insert`。

<!-- more -->

方法：

打开文件 `/usr/share/X11/xkb/keycodes/evdev`

修改映射表后边的数字。

```
<FK12> = 96;
...
<INS> = 118;
```
改为
```
<FK12> = 118;
...
<INS> = 96;
```

参考：
https://blog.csdn.net/elliott_yoho/article/details/78650838
