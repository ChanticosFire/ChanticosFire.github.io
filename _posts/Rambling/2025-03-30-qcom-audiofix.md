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
> **解释**：SELinux 拦截了 audio 服务访问 `/vendor/dsp`（ADSP 固件目录）的权限，导致音频 DSP 无法加载 → HAL 初始化失败 → `audioserver` 崩溃重启 → 触发Watchdog，系统进入无限重启或完全无声音状态。

尝试：
`setenforce 0`

无效 —— 原因是：这只能修改“当前运行时”的 SELinux 模式，**无法影响系统 early-init 阶段的权限策略执行**。

## 应急处理（可恢复进入系统）

暂时禁用音频 HAL，避免无限重启

用 adb 快速连接并执行：

```
adb shell setprop persist.vendor.audio.hal.disable 1 setprop persist.vendor.audio.init_disabled 1
```

然后立即`reboot`重启设备（防止触发下一轮 watchdog）



## 解决方案：Magisk SELinux Permissive 模块

安装以下 Magisk 模块，使系统 **在引导阶段即处于 Permissive 模式**：
**注意，此操作会降低系统安全性。**
 https://github.com/evdenis/selinux_permissive
重启后，音频服务正常启动、无报错、所有功能恢复正常。多次尝试重启/挂起唤醒等操作，问题**均未再复现**。

## 对 `setprop` 的修正说明

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
    

如果音频服务功能恢复之后还保留 `disable = 1`，系统会一直没有声音，**这是错误的设置方式**。

## 正确的音频修复步骤总结

```
# 启用 HAL
setprop persist.vendor.audio.hal.disable 0
setprop persist.vendor.audio.init_disabled 0
setprop persist.vendor.audio.freshstart true

# 重启音频服务
stop audioserver
start audioserver
```

## 踩过的坑（高危操作）

以下操作会导致桌面丢失配置、黑屏或系统功能异常，操作前需备份：

`rm -rf /mnt/vendor/persist/audio`
该路径包含设备的 **音频校准参数、驱动配置、部分系统持久性数据**，误删会触发系统级回退行为。

正确清理方式，只使用：
```
rm -rf /data/vendor/misc/audio 
rm -rf /data/vendor/audiohal
```
或清理完成后恢复备份的`/mnt/vendor/persist/audio`数据

---

## 总结

- **音频异常并非硬件问题，而是 SELinux 策略拦截导致**，通过 Magisk 模块设为 Permissive 状态，即可恢复 `/vendor/dsp` 权限

本案例可为未来类似 QCOM 平台音频崩溃问题提供参考，尤其在日志提示 `audioserver crash + listAudioPorts error -19` 时，优先考虑 **SELinux 或 HAL 权限问题**。