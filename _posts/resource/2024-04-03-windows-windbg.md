---
layout: post
title: windbg 符号文件配置
category: 资源
tags:
  - Windows
keywords: Windows
---
前段时间帮忙看一个win的crash log，结果发现自己出门带的电脑甚至还没dbg环境，赶紧装了一个，结果符号文件怎么也调用不起来，各种尝试，配环境变量配注册表，下载了一堆不同版本的离线符号文件和dbg包，最后发现是网络原因。

解决网络问题之后，直接Symbol File Path里写一条
```
srv*C:\Symbol*https://msdl.microsoft.com/download/symbols
```

完事