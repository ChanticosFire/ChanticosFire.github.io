---
layout: post
title: Moto Edge 60 Pro Root & 隐藏方案（KSU-NEXT + Play Integrity FULL PASS）
category: 技术
tags:
  - Android
  - Google
  - root
  - 玩机
keywords: Android
---
# Moto Edge 60 Pro Root & 隐藏方案（KSU-NEXT + Play Integrity FULL PASS）

## 1. 设备与前提
我的环境：

- 机型：Moto Edge 60 Pro（RETCN）
- 系统版本：Android 15
- 内核版本：`6.1.115-android14-11-g32fd20834ef3-ab13251582`
- **目标**：解锁 Bootloader，获取 Root、使用 LSPosed 模块、隐藏 Root，并通过 Play Integrity **Strong** 检测
    
---

## 2. 解锁和 Root 方案（KSU-NEXT / LKM）

### 前置准备

1. 安装 **Moto 驱动**（供 PC 通过 ADB 连接手机）  
    [Motorola 官方 USB 驱动下载](https://en-us.support.motorola.com/app/usb-drivers)
    
2. 安装 **ADB & Fastboot 环境**（此处不赘述）
    
3. 可选：下载 [Tiny Fastboot Script v1.10.6](https://bbs.ixmoe.com/t/topic/17646) 方便操作
    
4. 获取对应系统版本固件
    - 我的设备：`XT2507-5_CYBERT_RETCN`
    - 系统版本：`15_V2VVC35.58`
    - 固件下载地址：[RETCN 官方固件镜像](https://mirrors.lolinet.com/firmware/lenomola/2025/cybert_retcn/official/RETCN/)
        
5. 下载所需模块，我使用的版本如下（建议使用最新版）：
    
    - **Zygisk Next**： [Dr-TSNG/ZygiskNext](https://github.com/Dr-TSNG/ZygiskNext)  
        `Zygisk-Next-1.2.9.1-534-b8e7e21-release`
        
    - **LSPosed（Zygisk 版）**： [JingMatrix/LSPosed](https://github.com/JingMatrix/LSPosed)  
        `LSPosed-v1.10.2-7182-zygisk-release`
        
    - **Shamiko**： [LSPosed/Shamiko](https://github.com/LSPosed/LSPosed.github.io/releases)  
        `Shamiko-v1.2.5-414-release`
        
    - **Play Integrity Fix (PIF)**： [KOWX712/PlayIntegrityFix](https://github.com/KOWX712/PlayIntegrityFix)  
        `PlayIntegrityFix_v4.2-inject-s`
        
    - **TrickyStore**： [5ec1cff/TrickyStore](https://github.com/5ec1cff/TrickyStore)  
        `Tricky-Store-v1.3.0-180-8acfa57-release`
        
    - **TrickyStore Addon**： [KOWX712/Tricky-Addon-Update-Target-List](https://github.com/KOWX712/Tricky-Addon-Update-Target-List)  
        `TrickyAddonModule-v4.1`
        
---

### 解锁 Bootloader

1. 开启 **开发者选项** → 打开 **OEM 解锁** 和 **USB 调试**。
    
2. ADB 模式下执行：
```bash
adb reboot fastboot 
fastboot oem get_unlock_data
```

3. 将输出内容去掉换行与无关字符，提交到 [摩托罗拉解锁页面](https://motorola-global-portal.custhelp.com/app/standalone/bootloader/unlock-your-device-a) 获取解锁码。
    
4. 使用Tiny Fastboot Script或直接在adb执行：
    
```bash
fastboot oem unlock <解锁码>
```

    按音量键+电源键确认。
---

### 获取 Root（KSU-NEXT / LKM 模式）

1. 从官方固件提取 **init_boot.img**。
    
2. 推送到手机：
```bash
adb push init_boot.img /sdcard/
```

3. 在手机上使用 **KernelSU NEXT** 修补 `init_boot.img`（模式选择 **LKM**）。
    
4. 将修补好的文件拉回电脑：
```bash
adb pull /sdcard/<修补后文件名>.img
```
    
5. 确认当前槽位：
```bash
fastboot getvar current-slot
```
    我的返回 `a`，则刷写到 `a` 槽：
    
```
fastboot flash init_boot_a <patched_init_boot.img>
```
    
6. 重启并确认 **KSU-Next** 正常启用，且为 **LKM 模式**。
    

---

## 3. Zygisk + 模块环境

1. 在 **KSU-Next** 中启用 **Zygisk Next**。
2. 安装 **LSPosed Zygisk 版**（注意不要用无法启动的官方旧版）。
3. 安装 **Shamiko**（App Profile 模式，配合 DenyList 隐藏 Root）。
    
至此，root环境已经正常可用，但momo仍可检测到bl状态，Play完整性不通过

---

## 4. Play Integrity 隐藏方案

### 安装与配置

1. 安装：
    - **PIF**
    - **TrickyStore**
    - **TrickyStore Addon**
        
2. 重启手机。
3. 打开 **PIF** → 执行初始化。
4. 使用 MT 文件管理器或任何可使用root权限的文件管理器进入 `/data/adb`，找到 `pif.prop`（或 `pif.json`），确认指纹（我的是 `Pixel 8 Pro`）。  
    将该文件复制到：
```bash
/data/adb/modules/playintegrityfix/
```
5. 重启。
    
6. 打开 **KSU → Tricky Store**：
    - 勾选：
        - Google Play Services
        - Google Play Store
        - Google Services Framework
    - 菜单 →执行 **Set Valid Keybox**
        - 执行 **Set AOSP Key**
        - 执行 **Set Unknown Key**
    - 菜单 → **Set Security Patch** → **Get Security Patch Date** → **Save**
        
7. 重启设备。
---

## 5. 完整性验证

我在设置中清除了Google Play数据，然后使用了以下工具验证：

- **momo**（检测 Root / 系统状态）
- **SPIC**（Simple Play Integrity Checker）
- **TBCheck**（TBSafetyChecker）
    

结果：
```
deviceRecognitionVerdict： 
MEETS_BASIC_INTEGRITY 
MEETS_DEVICE_INTEGRITY 
MEETS_STRONG_INTEGRITY
```

已完全通过 Google Play Integrity（Strong）。