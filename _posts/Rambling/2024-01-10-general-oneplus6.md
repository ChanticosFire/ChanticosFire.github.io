---
layout: post
title: 一加6从氧OS刷回氢OS
category: 技术
tags:
  - Android
keywords: Android
---
之前的Oneplus6刷到氧OS之后，发现没有云服务支持，手动安装HOS上的云服务包也不生效，从第一部手机备份到当时十来年的短信和通话记录数据都存在云里，没办法下载到本地，只有云服务里的照片找了个教程[一加云相册批量下载](https://kouss.com/2018-10-12)从阿里云OSS挨个拉了回来又拿powershell按文件日期重命名为了原名，一直想着有条件了之后刷回氢OS把这些记录再备出来，一直到现在Pixel6正式服役之后才终于能干了。

因为当前需求是从氧降到氢，而且跨版本要刷最低级的包，干脆省去了双清等步骤直接用MTM线刷，本地存的包历史太过久远，当时并没有做严格的文件备注，所以翻了翻也不是很确认能不能用，于是去了[大侠阿木](https://yun.daxiaamu.com/OnePlus_Roms_2/%E4%B8%80%E5%8A%A06/)的网站下载了5.13原版的线刷包，直接干。


1、安装9008驱动，由于电脑重装过系统，所以原先的9008驱动掉了需要重新安装，驱动文件随附在线刷包中，安装只需要在设备管理器中找到如下设备，右键指定驱动文件位置就可以了。
![设备名](/assets/img/9008devices.png)

2、连上adb，验证
```
adb devices
```

2、关机，打开MsmDownloadTool，点击Start

3、按住音量+和音量-同时连上数据线，等MsmDownloadTool中识别到就正式开始了。

运行之后报了个错误，“Sahara通信失败,请给手机断电”。拔线，开机，插线，用```adb reboot bootloader```进到fastboot按音量键选PowerOff关机，重新第三步流程，等待，完事。
![MSM error](/assets/img/MsmDownloadTool.png)
![进系统](/assets/img/oneplus6-h2os-1.png)

简单配置，登账号，默认的百度输入法敲二十多位的随机字符复杂密码可太难受了，进去系统第一件事就是启用开发者选项，用scrcpy连到Windows上来操作。

打开云服务的时候收到了系统版本的推送更新，停留在氢OS190904，不胜唏嘘。
![系统版本](/assets/img/oneplus6-h2os-2.png)

旧版本的云服务相册功能不太正常，不能下载，用adb install以前备份出来的高版本相册也不管用，干脆把整个系统升到高版本好了，这次就可以直接把包推到本地卡刷了，因为已经忘记了最低版本到高版本的升级顺序，所以卡刷还是选用推送的190904的包，没有用本地另一个210526的包。

```
C:\Users\Administrator>adb push D:\Mobile\6-H2OS\OnePlus6Hydrogen_22_OTA_034_all_1909041314_0edfbb2dfaa34e94.zip /sdcard/
```

进设置-系统升级-本地升级，然后就是等待
![update](/assets/img/oneplus6-h2os-3.png)


齐活，剩下的就是拿备份软件把所有短信和通话记录备出去了，不再赘述。