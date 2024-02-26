---
layout: post
title: 网站访问量计数
category: 技术
tags:
  - SEO
keywords: SEO
---
今天突然想起来，加了个显示在页面底部的访问量计数，用的是不蒜子的js

```
<!-- 添加不蒜子访问量计数器，并使其居中 -->
<div style="text-align: center;">
	<span id="busuanzi_container_site_pv">
		本站总访问量<span id="busuanzi_value_site_pv"></span>次 </span>
</div>



<!-- 不蒜子计数器脚本 --> 
<script async src="//busuanzi.ibruce.info/busuanzi/2.3/busuanzi.pure.mini.js"></script>
```


但是不蒜子现在不让注册，导入以往访问量只能通过注册之后手动提交，所以相当于公开的访问量重新计数了。

另外blog.15926.tech和15926.tech是分开计数的，我把config.yml改了一下，长期的话还是以15926.tech为准，后面直接不用blog的二级域名了也行，毕竟这个站本身也就是以博客为主，域名干啥无所谓。
