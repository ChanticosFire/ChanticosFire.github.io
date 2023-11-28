---
layout: post
title: Quest2投屏优化
category: 技术
tags:
  - 其他
keywords: Android
---
这两天把Quest2翻出来折腾折腾，打BeatSaber的时候想录个屏看看，于是打开Side Quest连一下，顺便看了看默认的参数，Side Quest那边默认的''Crop''参数为1600:900:2017:510，意味着宽度是 1600 像素，高度是 900 像素，起始X坐标是 2017，起始Y坐标是 510。

默认的画面投出来显得裁切了很多，于是就开始折腾更改分辨率参数，因为要增加高度值，所以将第二个数字900改为了1000，同时为了保证画面居中，所以需要适当减少起始Y坐标，如果增加了 100 像素的高度，那么理论上需要从原始的起始Y坐标减去 50（因为需要在画面的上下各增加 50 像素），调整后的画面参数为"1600:1000:2017:460"，投屏测试高度还是差一些，于是决定再加100像素，同理同时调整Y坐标，调整后新的参数是 "1600:1100:2017:410"，测试没问题之后尝试优化画面宽度减少左右黑色圆角，最后直接凑了个整，改成了"1600:1100:2000:410"。

在这里附上整理好的投屏命令：

优化尺寸直接投屏
```
scrcpy --crop=1600:1100:2000:410 --disable-screensaver --crop=1600:1100:2000:410 --no-audio 
```

优化尺寸声音转发投屏
```
scrcpy --crop=1600:1100:2000:410 --disable-screensaver --crop=1600:1100:2000:410 
```

优化尺寸静音录制
```
$timestamp = Get-Date -Format "yyyyMMdd_HHmmss"
scrcpy --crop 1600:1100:2000:410 --disable-screensaver  --no-audio --record "C:\Data\Game\VR\$timestamp.mp4"

```

优化尺寸声音转发录制(powershell)，使用aac编码
```
$timestamp = Get-Date -Format "yyyyMMdd_HHmmss"
scrcpy --crop 1600:1100:2000:410 --disable-screensaver --audio-codec=aac --audio-encoder='c2.android.aac.encoder' --record "C:\Data\Game\VR\$timestamp.mp4"

```

无声音双目投屏
```
scrcpy --no-audio
```

Sidequest声音转发默认投屏
```
scrcpy --crop=1600:900:2017:510
```


参数解释：
```
--crop=
#画面分辨率

--disable-screensaver
#禁止熄屏保护

--no-audio
#不转发设备音频

--record
#录制 后跟输出路径

--audio-codec=aac --audio-encoder='c2.android.aac.encoder'
#使用的音频编码

```


补充：
如果对录制视频有音频编码格式要求，可使用如下命令查看设备支持格式
```
scrcpy --list-encoders
```
