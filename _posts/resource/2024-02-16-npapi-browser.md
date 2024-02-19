---
layout: post
title: 
category: 资源
tags:
  - 其他
keywords: Other
---
## 太长不看版：

[Mozilla Firefox History release](https://ftp.mozilla.org/pub/firefox/releases/)

## 正文
之前发了一个关于启用IE的blog，其实我用IE的原因只是要用JAVA插件，因为许多基础设施的维护管理依赖IE浏览器或老版本JAVA插件，所以必须要保留一个能正常使用的途径。装虚拟机倒是一劳永逸，但是每次启用的时候麻烦，平时带Surface干活这捉急的性能跑Vmware又很卡（此处怒骂Hyper-V的各种恶性bug），也只能在本机跑IE了。

实际上，除了IE之外还有其他使用JAVA插件的方法，JAVA插件本质上是依赖NPAPI来调用的，而由于拉跨的安全性，各浏览器厂商均在几年前把NPAPI给ban了，所以方法很简单：只需要使用未禁用NPAPI之前的版本即可。

找找这几个主流浏览器都在哪个版本禁用的NPAPI：

- **Chrome** 42版本开始禁用了NPAPI插件，但用户还能手动启用。到了Chrome 45版本（2015年9月发布），Google彻底移除了对NPAPI插件的支持。
- **Mozilla Firefox**：Firefox 52版本（2017年3月发布）开始，Mozilla移除了对大多数NPAPI插件的支持，除了Adobe Flash之外。
- **Apple Safari**：Safari 10版本（2016年发布）开始，Apple限制了NPAPI插件的使用，通过在Safari偏好设置中默认禁用这些插件来实现。Safari 12版本（2018年发布）中，Apple彻底移除了对大多数NPAPI插件的支持，包括Java和Silverlight，但对Adobe Flash的支持保留了更长时间。

Safair先不去想，和Chrome一比，很显然应该选择更良心更隐私的Firefox，Firefox在52版本禁用了NPAPI支持，实际上还可以通过在config中添加
```
`"plugin.load_flash_only"` false
```
短暂启用，直到Firefox53后被彻底移除。

所以很简单，只要下载52之前版本的Firefox并禁用update就可以了，Mozilla基金会非常良心的提供了所有历史版本的下载。当然由于安全问题，还是建议只在必要的时候使用，平时还是要使用最新版本的浏览器（然后被0day干。