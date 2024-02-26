---
layout: post
title: Win10 22H2启用IE
category: 技术
tags:
  - Windows
keywords: Windows
---
我的两台Windows 10 PC在更新[KB5031355](https://support.microsoft.com/en-us/topic/kb5031355-cumulative-security-update-for-internet-explorer-october-10-2023-c56fd2d7-64c5-4883-8614-b745b7325530)之后，IE再次被禁用了，在此补丁之前用更改注册表的方式启用了IE，但是现在就没法用了，网上找了一下看到了一片讨论，贴在这里：[Force Internet Explorer 11 to open instead of Edge on Windows 10 - Super User](https://superuser.com/questions/1814761/force-internet-explorer-11-to-open-instead-of-edge-on-windows-10)。

这是我现在使用的启动IE的vbs脚本：
```
CreateObject("InternetExplorer.Application").Visible=true
```
保存到本地vbs直接执行就可以拉起IE了。

顺便附上更早之前的版本修复用的reg条目，导入之后就可以直接通过IE的可执行文件来启动IE：
```
Windows Registry Editor Version 5.00



[HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\Browser Helper Objects\{1FD49718-1D00-4B19-AF5F-070AF6D5D54C}]

@="IEToEdge BHO"

"NoInternetExplorer"="1"



[HKEY_LOCAL_MACHINE\SOFTWARE\WOW6432Node\Microsoft\Windows\CurrentVersion\Explorer\Browser Helper Objects\{1FD49718-1D00-4B19-AF5F-070AF6D5D54C}]

@="IEToEdge BHO"

"NoInternetExplorer"="1"
```

