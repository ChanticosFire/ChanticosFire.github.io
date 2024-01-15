---
layout: post
title: IBM DS8000系列存储的一些笔记
category: 技术
tags:
  - 存储
keywords: Storage
---
前面板标识灯连接C5-J3(RACK ID CARD)，控制信号来自C1-J1/C2-J1(RPC CARD) 


## 电源

> 每个机架中的电源系统都是一对带内部电池的直流不间断电源 (DC-UPS)。这些 DC-UPS 分配经整流的交流电，并提供电源切换以实现冗余。在一个 DC-UPS 停止服务时，单个 DC-UPS 具有足够的能力为整个机架供电并提供备用电池。
>
> 有两根交流电源线，分别为一个 DC-UPS 供电。如果输入线路上不存在交流电，那么输出线路会切换到来自伙伴 DC-UPS 的经整流的交流电。如果两个交流电输入线路均处于不活动状态，那么 DC-UPS 会切换到 208 伏直流电池电源。将保护具有扩展电源线干扰 (ePLD) 选件的存储系统免受电源线干扰，时间可长达 50 秒。 不具有 ePLD 选件的存储系统将得到 4 秒时间的保护。
>
> 一对集成的机架电源控制 (RPC) 卡管理存储系统内的配电效率。RPC 卡连接到每个处理器节点。 RPC 卡还连接到每个机架中的主电源系统。


## 管理


>1.  从对硬件管理控制台 （HMC） 具有 [https://*HMC_IP*/service](https://HMC_IP/service) 网络访问权的系统上的 Web 浏览器访问 DS 服务 GUI，其中 HMC_IP 是 HMC 的 IP 地址或主机名。您也可以通过 DS8000® 存储管理 GUI 登录页面上的链接访问 DS 服务 GUI。
>2.  Log in to the DS Service GUI by using the service administrator account and change the password for that account.\
    使用服务管理员帐户登录到 DS 服务 GUI，并更改该帐户的密码。\
    The service administrator account is pre-configured with user ID (customer) and password (cust0mer).\
    服务管理员帐户预先配置了用户标识 （ customer ） 和密码 （ cust0mer ）。
>1. 执行更换和查看part需要CE用户 CE/serv1cece。
>2. [http://HMC-IP:8080](http://HMC-IP:8080)
>  
>
> 不安全端口8451，https端口8452
>

