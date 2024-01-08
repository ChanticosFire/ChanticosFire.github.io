---
layout: post
title: 开启Pixel的VOLTE通话
category: 杂谈
tags:
  - Android
keywords: Android
description:
---
手里的Pixel 6a用了一天，惊觉打电话的时候音质奇差，细看才发现是2G通话，并没有使用VoLTE，上网搜索，得到解决办法，遂转载一下。

出处：[给国内新用户的 Pixel 7 系列使用指南 - 少数派 (sspai.com)](https://sspai.com/post/78200)

*在没有 root 的情况下，为 Pixel 7 系列（其实也同样适用于 Pixel 6 系列）解锁国内运营商 VoLTE 相关功能。*

*VoLTE 的基础性和重要性本文不再赘述了，对 Pixel 6/7 这两代设备而言，**VoLTE 功能同时也是电信用户正常使用通话功能的前提**。因此这里推荐没有 root 的用户选择近期由韩国开发者 @[kyujin-cho](https://sspai.com/link?target=https%3A%2F%2Fgithub.com%2Fkyujin-cho) 分享的免 root VoLTE 功能解锁方案。*

*说起 Android 这边的「免 root 玩机」话题，Shizuku 这款工具自然是少不了的。所幸 undefined此前已经分享过非常相近的介绍和配置方法，因此本教程的第一部分「Shizuku 配置」，请移步至下面这篇文章了解。*

关联阅读：**[别被 root 挡在门外：Shizuku 让 Android 玩机更简单](https://sspai.com/post/73294)

*确保 Shizuku 服务已经正常运行之后，前往 Pixel IMS 的[发行版](https://sspai.com/link?target=https%3A%2F%2Fgithub.com%2Fkyujin-cho%2Fpixel-volte-patch%2Freleases)页面下载安装该工具的最新版本。安装完成后启动 Pixel IMS，此时你应该能够看到一个 Shizuku 接口调用的权限请求，点击「总是允许」：*

![Shizuku授权](/assets/img/pixel-volte-3.png)

*随后进入 Pixel IMS 的主界面（希望开发者后续能适配个 Material 3 不知算不算过分），点击左下角的 ENABLE VOLTE 按钮进行激活。激活后，VoLTE Status 区域下的 VoLTE Enabled by Config 选项开关会自动变成启用状态。*
![开启volte](/assets/img/pixel-volte-1.png)
![使用4G替换VoLTE图标](/assets/img/pixel-volte-2.png)
*重启设备后你应该就能看见对应 SIM 设置中的 VoLTE 选项开关了。至此，电信用户已经可以在 Pixel 6/7 系列机型上拥有完整的 4G 网络体验。*

参考链接：[pixel-volte-patch | GitHub](https://sspai.com/link?target=https%3A%2F%2Fgithub.com%2Fkyujin-cho%2Fpixel-volte-patch) 

后面两张图是我自己手机上的新版截图，替换了原文的图片。事实上我在启用相关选项之后并未经过重启，就已经可以使用VoLTE，应该是新版变化。