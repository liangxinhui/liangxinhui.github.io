---
title: 虚拟机磁盘空间压缩
date: 2019-07-18 22:07:42
tags:
 - Virtualbox
 - 压缩
---

# 问题

虚拟机的磁盘类型，一般为了节省空间，会设置为动态增长。随着写入文件的增多，磁盘文件（vdi/vmdk等)会越来越大。即便虚拟机内部文件删除了，宿主机中原来开辟出来的空间还是不会释放。 

VMWare、Virtualbox提供了相应的工具来压缩磁盘空间。

以Virtualbox为例，该命令为 `VBoxManage modifymedium <diskfile> --compact`.


<!-- more -->

# 官方说明

[链接](https://docs.oracle.com/cd/E97728_01/E97727/html/vboxmanage-modifyvdi.html)

> For compatibility with earlier versions of Oracle VM VirtualBox, the modifyvdi and modifyhd commands are also supported and mapped internally to the modifymedium command.

> The --compact option can be used to compact disk images. Compacting removes blocks that only contains zeroes. Using this option will shrink a dynamically allocated image. It will reduce the physical size of the image without affecting the logical size of the virtual disk. Compaction works both for base images and for differencing images created as part of a snapshot.
> 
> For this operation to be effective, it is required that free space in the guest system first be zeroed out using a suitable software tool. For Windows guests, you can use the sdelete tool provided by Microsoft. Run sdelete -z in the guest to zero the free disk space, before compressing the virtual disk image. For Linux, use the zerofree utility which supports ext2/ext3 filesystems. For Mac OS X guests, use the diskutil secureErase freespace 0 / command from an elevated Terminal.
> 
> Please note that compacting is currently only available for VDI images. A similar effect can be achieved by zeroing out free blocks and then cloning the disk to any other dynamically allocated format. You can use this workaround until compacting is also supported for disk formats other than VDI.

# 关键点

**使用`VBoxManage modifymedium`前，必须先将磁盘未使用空间填为`0`**
- Windows 用 `sdelete -z`
- Linux 用 zerofree 或 dd (`dd if=/dev/zero of=/tmp/zerofile bs=10M`)

# 补充说明

- 除了使用`VBoxManage modifymedium`，也可以用克隆磁盘的方法来进行。前提都是先将未使用空间重置为`0`
- 有些教程提到 `VBoxManage modifyhd` 或 `VBoxManage modifyvdi`，新版本统一为 `VBoxManage modifymedium`
- `dd` 操作写文件时，可放心操作，即便宿主机可用空间不足，虚拟机内部也能写满。可以理解为`dd`持续写0，不会增加宿主机中虚拟磁盘文件大小。


