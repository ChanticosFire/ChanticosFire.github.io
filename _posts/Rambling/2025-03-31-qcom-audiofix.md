---
layout: post
title: OnePlus 8T音频服务故障深度排查与修复记录
category: 技术
tags:
  - Android
keywords: Android
---

---

## 问题背景

设备：**OnePlus 8T**  
平台：**Qualcomm QCOM**  
系统表现：

- 电量耗尽 → 关机 → 充电重启后出现音频故障
- **系统内所有音频相关部分完全失效**
- **麦克风和扬声器无法使用**
- 某些调用麦克风的应用（如录音）直接 hang 住
- **重启 / 安全模式 / 单清缓存** 无法解决

## 故障日志

在 `adb logcat` 中持续出现如下错误：

```plaintext
F libc    : Fatal signal 6 (SIGABRT), code -1 (SI_QUEUE) in tid XXXX (TimeCheckThread), pid XXXX (audioserver)
Abort message: 'TimeCheck timeout for IAudioFlinger command 22'

AudioManager: updateAudioPortCache: listAudioPorts failed
AudioSystem-JNI: AudioSystem::listAudioPorts error -19
```
### 初步判断

- `IAudioFlinger command 22 timeout`：音频 HAL 无法响应，服务 hang 死
    
- `listAudioPorts error -19`：设备节点或 HAL 无法初始化
    
- 系统表现符合 audio HAL 初始化失败

## 初次尝试解决：更换硬件

由于对 Android 底层调试能力有限，最初怀疑为主板或音频芯片故障，进行了 硬件更换，故障随之消失。

但 两个月后问题再次复现，且在执行`setprop`和`start/stop audioserver`命令后进入自动重启循环状态，说明 **问题并非硬件，而是软件或系统配置异常**。

在查看内核日志时发现如下关键报错：


```
avc: denied { getattr } for comm="mount" path="/vendor/dsp" scontext=u:r:system_app:s0 tcontext=u:object_r:adsprpcd_file:s0 tclass=dir permissive=0`
```
*SELinux 拦截了 audio 服务访问 `/vendor/dsp`（ADSP 固件目录）的权限，导致音频 DSP 无法加载 → HAL 初始化失败 → `audioserver` 崩溃重启 → 触发Watchdog，系统陷入重启循环或完全丧失音频功能。*

尝试使用 `setenforce 0` 放宽权限限制

无效 —— 原因是：这只能修改“当前运行时”的 SELinux 模式，**无法影响系统 early-init 阶段的权限策略执行**。

## 应急处理（可恢复进入系统）

暂时禁用音频 HAL，避免无限重启

用 adb 快速连接并执行：

```
adb shell setprop persist.vendor.audio.hal.disable 1 setprop persist.vendor.audio.init_disabled 1
```

然后立即`reboot`重启设备（防止触发下一轮 watchdog）

### 对 `setprop` 的修正说明

此前多次参考资料使用：
`setprop persist.vendor.audio.hal.disable 1`
来避免音频服务死循环重启。

实际上这会完全禁用 audio HAL，导致系统彻底无法发声

正确理解如下：

setprop persist.vendor.audio.hal.disable 0 这类命令是通过设置系统属性的方式来启用或关闭某些功能，当
`setprop persist.vendor.audio.hal.disable 0`
时，通常表示**不禁用（启用）**这个音频 HAL；而如果设置成
`setprop persist.vendor.audio.hal.disable 1`
则表示禁用这个音频 HAL。”
所以：
- `setprop persist.vendor.audio.hal.disable 0` → **启用音频 HAL**
    
- `setprop persist.vendor.audio.hal.disable 1` → **禁用音频 HAL**
    

音频服务即使恢复成功，但若 disable 参数仍为 1，仍将保持静音状态”


## 初步解决方案：Magisk SELinux Permissive 模块

安装以下 Magisk 模块，使系统 **在引导阶段即处于 Permissive 模式**：
**注意，此操作会降低系统安全性。**
 https://github.com/evdenis/selinux_permissive
安装完成后，可使用 `getenforce`命令 验证模块是否生效，需在 early-init 阶段生效才具备作用。

宇宙安全声明：**将系统 SELinux 全局设为 Permissive 并非长期可取**，最佳实践是**最小化策略放行**。这一点在调试阶段可以接受，但是如要做长期稳定的修复方案，最好是编写自定义的 `magisk policy.cil`，只放行所需访问，而非将系统整体放宽，如果是官方ROM且 bootloader 已解，可以考虑在 `vendor/etc/selinux/vendor_file_contexts` 或`vendor/etc/selinux/nonplat_vendor.cil` 中添加针对 `/vendor/dsp` 及音频进程的规则；但这也需要更高门槛的 ROM 定制或补丁能力。

### 确定音频架构
查看` /data/vendor/audio/acdbdata/delta/`发现

```
OnePlus8T:/ # ls /data/vendor/audio/acdbdata/delta/
MTP_Bluetooth_cal.acdbdelta  MTP_Handset_cal.acdbdelta  MTP_Speaker_cal.acdbdelta
MTP_General_cal.acdbdelta    MTP_Hdmi_cal.acdbdelta     MTP_workspaceFile.qwspdelta
MTP_Global_cal.acdbdelta     MTP_Headset_cal.acdbdelta  adsp_avs_config.acdbdelta
```
这说明 OnePlus 8T 使用的是 **QTI 音频 HAL + acdbdelta 动态校准数据结构**，而不依赖传统 `/persist/audio` 路径。

## 实际修复流程（已验证有效）

### 清除 audio 状态

```
setprop persist.vendor.audio.hal.disable 0
setprop persist.vendor.audio.init_disabled 0
setprop persist.vendor.audio.freshstart true
```

### 设置 acdb 相关路径变量

```
setprop persist.vendor.audio.calfile0 /data/vendor/audio/acdbdata/delta/MTP_Handset_cal.acdbdelta
setprop persist.vendor.audio.calfile1 /data/vendor/audio/acdbdata/delta/MTP_Headset_cal.acdbdelta
setprop persist.vendor.audio.calfile2 /data/vendor/audio/acdbdata/delta/MTP_Speaker_cal.acdbdelta
setprop persist.vendor.audio.calfile3 /data/vendor/audio/acdbdata/delta/MTP_Bluetooth_cal.acdbdelta
```

### 构造 HAL fallback 所依赖的 persist 路径，避免初始化失败”
*此步骤或许并非必须*

为避免系统在初始化时尝试访问不存在的旧路径 `/mnt/vendor/persist/audio` 导致 HAL 初始化失败，手动创建该目录并设置权限：
```
mkdir -p /mnt/vendor/persist/audio
chmod 755 /mnt/vendor/persist/audio
chown system:system /mnt/vendor/persist/audio
restorecon -Rv /mnt/vendor/persist/audio
```
系统随后会识别此路径为合法 `vendor_persist_audio_file` 类型，**不再报 SELinux 错误**。
 也有一些机型不会强制访问 `/mnt/vendor/persist/audio`，对缺失的目录只发一个 `-ENOENT` 日志，然后正常转到下一路径。这就意味着**是否“必须”创建该目录**，需要在具体机型/vendor 代码中确认。可以在 `vendor` 分区下的 `audio_platform_info.xml` 或者“音频 HAL 源码”（如果能拿到的话）看到它对 fallback 路径的处理方式。


### 设置音频 HAL 与校准参数

```
setprop persist.vendor.audio.hal.disable 0
setprop persist.vendor.audio.init_disabled 0
setprop persist.vendor.audio.freshstart true
setprop vendor.audio.hal.debug_level 1
```
`setprop` 会在系统运行期间生效” ，这类 `persist.*` 属性在 reboot 后可保留，适合做长期配置；但必须在 HAL 初始化前设定，重启后自动应用。
### 指定校准数据

```
setprop persist.vendor.audio.calfile0 /data/vendor/audio/acdbdata/delta/MTP_Handset_cal.acdbdelta
setprop persist.vendor.audio.calfile1 /data/vendor/audio/acdbdata/delta/MTP_Headset_cal.acdbdelta
setprop persist.vendor.audio.calfile2 /data/vendor/audio/acdbdata/delta/MTP_Speaker_cal.acdbdelta
setprop persist.vendor.audio.calfile3 /data/vendor/audio/acdbdata/delta/MTP_Bluetooth_cal.acdbdelta
```

### 重启音频服务链

```
stop vendor.audio-hal
stop audioserver
start vendor.audio-hal
start audioserver
```

### 最后重启

```
reboot
```



重启后，音频服务正常启动、无报错、所有功能恢复正常。多次尝试重启/挂起唤醒等操作，问题**均未再复现**。



---

## 总结和分析

- **音频异常并非硬件问题，而是 SELinux 策略拦截导致**，通过 Magisk 模块设为 Permissive 状态，即可恢复 `/vendor/dsp` 权限。
- OnePlus 8T 使用基于 **QTI** 平台的音频架构，其校准数据位于 `/data/vendor/audio/acdbdata/delta/`，并非传统 `/mnt/vendor/persist/audio/`，当系统缺失该目录时（或 HAL 启动期尝试访问该路径失败），可能仍然触发 fallback 错误，因此建议**构造一个合法空目录**防止异常。
- 所有 `setprop persist.vendor.audio.calfileX` 都是传递给 HAL 和 DSP 设备的路径参数，必须在 HAL 初始化前设置。
- `persist.vendor.audio.freshstart=true` 可强制系统跳过缓存，重新加载校准配置。

本案例可为未来类似 QCOM 平台音频崩溃问题提供参考，尤其在日志提示 `audioserver crash + listAudioPorts error -19` 时，建议遇到类似 QCOM 音频初始化失败的情况时，优先从 SELinux 拦截、校准数据路径、HAL 属性配置几个角度逐步排查，避免盲目刷机或硬件更换。
