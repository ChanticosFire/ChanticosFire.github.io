
layout: post
title: OneNote转Obsdian的记录
category: 杂谈
tags: 其他
keywords: Other
description: 
---

### OneNote转Obsdian的操作记录 
#### 环境：Win主机+Surface端Win/Linux双系统   

由于许久以前就购买了Office365的全家桶，习惯于多端之间文件的互相同步与OneDrive备份系统文件的便捷，再加上几年前某次干活见到另一位工程师笔记本上OneNote精细的个人文档，
于是在建立自己的个人文档库的时候顺理成章的也开始用起了OneNote。   

在转到Surface之后，配合着Slim2手写笔，OnenNote用得是更加顺手，但情况在我给Surface装了Linux之后急转直下，Linux端的Onenote客户端只有p3x一个，
本质是Web版OneNote的浏览器套皮，且对网络要求极高，这给我带来了很大的不便，于是就想把自己的文档从微软自家私有的OneNote转向开放的Markdown，
关于Markdown的文档管理器我的备选其实有很多，比如之前用过的幕布，但最终还是选定了插件丰富的Obsdian，于是就开始研究把Onenote转到Markdown并导入Obsdian。   


关于格式转换的文档其实网上有很多，比如Obsdian教程的这一篇[如何将OneNote转md](https://publish.obsidian.md/chinesehelp/03+%E6%95%99%E7%A8%8B/%E5%A6%82%E4%BD%95%E5%B0%86OneNote%E8%BD%ACmd)
，而我使用的是经过此文档内工具修改过的库[https://github.com/youkaichao/onenote-to-markdown](https://github.com/youkaichao/onenote-to-markdown)，
文档内的README讲的还是挺详细的，但有一点需要注意：转换的原理是利用工作站上的 OneNote 对象模型将所有 OneNote 页面转换为 Word 文档，然后利用 Pandoc 将 Word 文档转换为 Markdown格式，
所以你的OneNote笔记标题，一定要符合Windows的文件命名规则，不能出现文件命名规则不允许的符号，否则此篇文档就会转换失败，转换之后的文件会自动按照原来的顺序以000-xxx,001-xxx,……命名，以实现在其他文档管理器内的排序。   

经过如上的转换步骤，我的文档就已经转换为了Obsdian可使用的.md格式，而接下来需要解决的问题就是：如何实现多端文件修改同步，因为在Linux下还是需要查文档的。   

Obsdian官方自带付费的笔记同步服务，但并不想单单为了同步功能就付费，能省则省，于是我选择了OneDrive（反正已经付过钱了.jpg，主机端将文档库所在的文件夹放到OneDrive的路径下，Surface端进入Win下将此文件夹设置为同步，
并将Obsdian库位置指定为此目录，这样可以实现两端Win的文件同步，而Linux端就头疼一点了，由于Linux端没有可用的OneDrive，所以只能从Surface的Windows系统所属磁盘分区下拉取文件，给对应分区写一个自动挂载，然后将文件夹位置拉到
Linux的文档库位置做个硬链接，Obsdian直接指向硬链接位置，也算是退了一步满足了看文档的需求，只不过文件同步的话还需要重启一次系统进入Win端来让OneDrive自动同步。   

至此，OneNote转Obsdian的操作基本完成，实现了三个系统内查文档编辑文档的需求，Linux端同步略有欠缺，经过群友提醒后面也可以放到GitHub上用Git来实现同步，对网络有一些要求。
手写笔的功能略有损失（Obsdian手写插件只能满足单页面内手写，不可以像OneNote那样手写与笔记在单文件内混合实现。）
