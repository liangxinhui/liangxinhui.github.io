---
title: usbip
tags:
---

# Server
- 树莓派

```bash
root@ubuntu:/usr/lib/linux-tools/4.15.0-1031-raspi2# lsusb 
Bus 003 Device 001: ID 1d6b:0003 Linux Foundation 3.0 root hub
Bus 002 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub
Bus 001 Device 004: ID 0471:485d Philips (or NXP) Senselock SenseIV v2.x
Bus 001 Device 003: ID 0424:ec00 Standard Microsystems Corp. SMSC9512/9514 Fast Ethernet Adapter
Bus 001 Device 002: ID 0424:9514 Standard Microsystems Corp. SMC9514 Hub
Bus 001 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub
root@ubuntu:/usr/lib/linux-tools/4.15.0-1031-raspi2# usbip list -p -l
usbip: error: failed to open /usr/share/hwdata//usb.ids
busid=1-1.1#usbid=0424:ec00#
busid=1-1.3#usbid=0471:485d#

root@ubuntu:/usr/lib/linux-tools/4.15.0-1031-raspi2# usbip bind --busid=1-1.3
usbip: info: bind device on busid 1-1.3: complete


```


# Client

- ubuntu vm 

```bash

root@liangxh-dev:~# usbip attach -r 192.168.31.136 -b 1-1.3
libusbip: error: udev_device_new_from_subsystem_sysname failed
usbip: error: open vhci_driver
usbip: error: query
root@liangxh-dev:~# modprobe vhci-hcd



root@liangxh-dev:~# usbip list -r 192.168.31.136
usbip: error: failed to open /usr/share/hwdata//usb.ids
Exportable USB devices
======================
 - 192.168.31.136
      1-1.3: unknown vendor : unknown product (0471:485d)
           : /sys/devices/platform/soc/3f980000.usb/usb1/1-1/1-1.3
           : unknown class / unknown subclass / unknown protocol (ff/00/00)
           :  0 - unknown class / unknown subclass / unknown protocol (ff/00/00)

root@liangxh-dev:~# lsusb 
Bus 004 Device 001: ID 1d6b:0003 Linux Foundation 3.0 root hub
Bus 003 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub
Bus 001 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub
Bus 002 Device 002: ID 80ee:0021 VirtualBox USB Tablet
Bus 002 Device 001: ID 1d6b:0001 Linux Foundation 1.1 root hub
root@liangxh-dev:~# usbip attach -r 192.168.31.136 -b 1-1.3
root@liangxh-dev:~# lsusb 
Bus 004 Device 001: ID 1d6b:0003 Linux Foundation 3.0 root hub
Bus 003 Device 003: ID 0471:485d Philips (or NXP) Senselock SenseIV v2.x
Bus 003 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub
Bus 001 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub
Bus 002 Device 002: ID 80ee:0021 VirtualBox USB Tablet
Bus 002 Device 001: ID 1d6b:0001 Linux Foundation 1.1 root hub
root@liangxh-dev:~# usbip port 
usbip: error: failed to open /usr/share/hwdata//usb.ids
Imported USB devices
====================
Port 00: <Port in Use> at Full Speed(12Mbps)
       unknown vendor : unknown product (0471:485d)
       3-1 -> usbip://192.168.31.136:3240/1-1.3
           -> remote bus/dev 001/004
root@liangxh-dev:~# usbip detach -p 00
root@liangxh-dev:~# lsusb 
Bus 004 Device 001: ID 1d6b:0003 Linux Foundation 3.0 root hub
Bus 003 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub
Bus 001 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub
Bus 002 Device 002: ID 80ee:0021 VirtualBox USB Tablet
Bus 002 Device 001: ID 1d6b:0001 Linux Foundation 1.1 root hub

```