---
layout: post
title: Wireshark网络分析的艺术笔记
category: 技术
tags:
  - Linux
keywords: Linux
---
今天整理文件，看到了一张之前阅读的时候存的截图：
![](/assets/img/rfc5531wireshark.png)
图中内容出自《Wireshark网络分析的艺术》，作者林沛满先生在EMC网络存储部门任主任工程师，书中使用了非常多的实际业务中的案例进行讲解，我从中受益良多。

特意标出这一句是在读至此段时被厂商粗放的解决方式震惊到，此段的前文是一家提供NFS服务器的厂商遇到了由RFC 5531限制了用户组访问数量导致的文件无法访问问题，惊讶于为了绕过此限制竟然使用了把客户端系统的/etc/passwd和/etc/group文件复制到服务器进行本地查询的方式。这样的解决方式在安全上无疑是具有巨大风险的，一旦被攻击就容易导致横向移动一锅端，这种方法更多是一种权宜之计而不是最佳实践。

通常，面对需要跨系统同步用户信息的场景，更多的解决方案倾向于使用集中式身份验证服务，如LDAP、Active Directory或使用OAuth、SAML等现代身份验证和授权协议来统一管理用户身份和权限。