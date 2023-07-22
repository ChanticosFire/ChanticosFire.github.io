---
layout: post
title: Linux引导出现Bluetooth: hci0: Malformed MSFT vendor event: 0x02故障的解决方案及wayland配置
category: 技术
tags: Linux
keywords: Linux
description: 
---

# Kali Linux（debian系）引导出现Bluetooth: hci0: Malformed MSFT vendor event: 0x02故障的解决方案及wayland配置 

关键词：Linux 引导 Bluetooth: hci0: Malformed MSFT vendor event: 0x02
环境描述：
OS:Kali Linux 2023
Kernel:Linux 6.4.2-surface

遇到此故障是在Surface设备安装Kali Linux+Windows双系统并配置[Linux-Surface内核](https://github.com/linux-surface/linux-surface "Linux-Surface内核")之后，想要实现原生触摸以用来滑动和缩放网页/阅读器的功能，于是按照[Installation-and-Setup](https://github.com/linux-surface/linux-surface/wiki/Installation-and-Setup "Installation-and-Setup")最后部分的内容开始安装Wayland 及 KDE，在安装过程中选择了SDDM作为默认的显示管理器以支持KDE，而它成为了问题的根源。
在进行完安装之后，我遇到了一些故障，比如在Wayland +KDE下输入用户名密码之后系统hang住，但选择X11和其他桌面环境则可以正常登录，以及遇到了本次的重点：引导报错。

报错具体呈现为开机引导显示系统LOGO后，出现Bluetooth: hci0: Malformed MSFT vendor event: 0x02报错，此时无法跳过和输入其他命令，只能通过Ctrl+Alt+F2切换到tty2或其他tty，我第一反应就是蓝牙出现了问题，于是尝试重新启动蓝牙然后重启
  ```shell
systemctl status bluetooth
systemctl restart bluetooth
systemctl status bluetooth
reboot
```
然而本次重新引导仍然复现了故障，我开始上网搜索此问题，找到的几乎都是从蓝牙服务入手来解决问题的，例如[ArchLinux社区的讨论](https://bbs.archlinux.org/viewtopic.php?id=276815 "ArchLinux社区的讨论")，在尝试过可能的办法无法解决后，我开始回溯自己做过的操作，并决定卸载KDE和Wayland。

    #移除KDE桌面环境
    sudo apt-get purge kde-plasma-desktop
    #移除Wayland
    sudo rm /etc/systemd/system/gdm.service.d/disable-wayland.conf
    #安装默认桌面环境
    sudo apt-get update
    sudo apt-get install xfce4
    sudo apt-get install xorg
    sudo dpkg-reconfigure xfce4
经过如上操作之后，我突然意识到我还需要使用X11+XFCE
    #查看是否安装了X11
    sudo dpkg -l |grep xserver-xorg
    #如果输出包含xserver-xorg，那么代表安装了X11
	
确认当前系统已安装X11后，执行`reboot`重新引导


好事不会那么容易发生，重新启动后tty1仍然卡在报错，再次切到tty2尝试排障。
如果使用xfce4的话，默认的显示管理器可能是LightDM，我决定再次确认一下
    echo $DESKTOP_SESSION
然而此命令输出了plasma，这代表我使用的不是LightDM而是之前安装KDE的时候切换的SDDM，那么接下来就应该安装并切换到LightDM。
    sudo apt-get install lightdm
    dpkg-reconfigure lightdm
在弹框中选择LightDM作为默认的显示管理器，然后`reboot`重启生效。

神奇的事情发生了，从SDDM切换到LightDM之后，故障消失了，并且我可以在登录的时候选择plasmawayland作为桌面环境，是KDE Plasma桌面环境在Wayland显示服务器下的实现，多次重启后无故障复现，蓝牙正常使用。

结论：
Bluetooth: hci0: Malformed MSFT vendor event: 0x02报错是有可能由SDDM引起的，此时不妨尝试切换到其他显示管理器尝试一下。


