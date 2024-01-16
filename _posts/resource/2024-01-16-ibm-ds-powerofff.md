---
layout: post
title: DS8800下电操作-通过HMC
category: 技术
tags:
  - 存储
keywords: IBM
---

客户要一篇DS8870的关机操作文档，找到官方文档和架构手册简单摘抄了一下，本文档只包含从HMC操作的部分，不涉及CLI和WUI下的关机操作。


本文部分内容来自IBM DS8000官方操作手册(微码 64.xx或以上版本)及IBM DS8870架构手册。

1．  首先到机器的后面，确认 Local/Remote 开关处于正常的“Remote”（向上）位置。如果 不是，则拨到 Remote 位置。 (本地开机/本地强制断电开关：当本地/远端开关处于本地模式时，本地开机/本地强制断电开关可以手动上电或强制断电给整个系统。当本地/远程开关处于远程方式时，HMC 控制电源开/关。)

2．  打开 Storage Facility Management，选择相应的 Storage Facility

3．  在“Service Utilities” 菜单中选择“Storage Facility Power Control…”
![HMC](/assets/img/ds8870poweroff(2).png)


4．  在弹出的窗口中，首先确认当前的 Power Control Mode 是“Manual”，然后确认 “Current State”是“On”，然后选中 Power OFF Storage Facility，点按钮 Apply
![power control](/assets/img/ds8870poweroff(3).png)**注意：**

1）关机将持续 5 至 10 分钟，直到所有硬盘的灯熄灭为止

**<font color="#ff0000">2）除非火灾或地震等紧急情况，否则千万不要通过红色的 UEPO 开关来关机，会 导致数据丢失！！！ （此开关为红色，位于前门后面。仅当前门打开时可见。该开关仅在以下极端情况下立即断开DS8870框架的所有电源：</font>**

**<font color="#ff0000">DS8870出现故障，使环境处于危险之中，例如火灾。</font>**

**<font color="#ff0000">DS8870使人的生命处于危险之中，例如触电。）</font>**
![UEPO](/assets/img/ds8870poweroff(1).png)

除如上所述或其他紧急事件外，切勿激活 UEPO 开关。激活 UEPO 开关时，将绕过允许 FHD 的电池保护。通常，如果线路断电，DS8870可以使用其内部电池将数据从NVS存储器转储到CPC中的内部磁盘驱动器，以便保留数据，直到电源恢复。但是，UEPO 开关不允许发生此转储过程，并且所有 NVS 数据都会立即丢失。此事件很可能导致数据丢失。

3）如果需要完全下电，先从 HMC 菜单关闭 HMC

从导航区域，单击 HMC 管理。在右侧工作区域中，转到“操作”部分，然后单击Shut Down or Restart.将打开Shutdown or Restart 窗口。选择 Shutdown HMC。最后打下 PPS 后面的黄色空气开关（拨到“OFF”位置）。

Notes： 如果机器是彻底下电，请将 PPS 上的供电电源线拔掉！如果在关机过程中遇到任何问题，请立即开 PMH 申请技术支持。如果机器在关机过程中，因硬件问题，可能会导致数据不能完全写到硬盘上，不正确的操作将会导致客户数据丢失！！

**<font color="#ff0000">4）在整个关机和加电过程中，都不应该去触动 P570 的控制面板的白色电源开关。</font>