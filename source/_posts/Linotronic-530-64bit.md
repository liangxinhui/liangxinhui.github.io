---
title: Windows10 下安装 Linotronic 530 虚拟打印机（64位）
date: 2019-08-28 22:27:02
tags:
  - Linotronic
  - prn
  - CorelDRAW
---

在印刷行业，一些老的用户还是习惯用`CorelDRAW 9`。而`CorelDRAW 9`的`pdf导入`功能太弱，很多不兼容。

在`XP`年代，当需要将`Word文档`或者`方正书版`生成的文件在`CorelDRAW 9`中进行排版时，一般的步骤为：

- 添加虚拟打印机`Linotronic 530-RIP 30 v52.3`
- 在`Word`或者`方正书版`中，通过虚拟打印机`Linotronic 530-RIP 30 v52.3`生成`prn`文件。
- 在`CorelDRAW 9`中导入`prn`文件。


当系统升级到`Win7`或者`Win10`时，系统的打印机驱动列表中，去掉了`Linotronic`系列的打印机。

<!--more-->

---

网上一些很好的教程会指导安装 `Win7 32位` 下的 `Linotronic 530-RIP 30 v52.3`。

主要流程为：
- 导出或下载`XP 32位`下的`Linotronic 530-RIP 30 v52.3`驱动安装文件到 Win7 32位 系统
  - `C:\WINDOWS\inf\ntprint.inf`文件
  - `C:\WINDOWS\system32\spool\drivers\w32x86\3`下的驱动文件
- 添加虚拟打印机，手工选择`ntprint.inf`文件
- 点击下一步，按提示选择缺失的驱动文件

参考：[你好，这个问题，windows7 64位添加Linotronic 530 v52.3打印机是怎么解决的？非常感激！](https://zhidao.baidu.com/question/1992083853597810387.html)

---

到了`Win10` 年代，系统硬件升级很快, 很多电脑也都用上了大内存，升级为`64位`。

按照网上的很多说法，`Linotronic 530-RIP 30 v52.3` 在`64位`下不可用。

经测试，从`XP 64位原版系统` 中提取的打印机驱动，可在`Win10 64位`下正常使用。

路径为：
- `C:\WINDOWS\inf\ntprint.inf`
- `C:\WINDOWS\system32\spool\drivers\x64\3`

安装流程同上。

附百度网盘链接：

> 链接：https://pan.baidu.com/s/18cTe7iTvAbiteWwjYI1ynw 
> 提取码：s66w 