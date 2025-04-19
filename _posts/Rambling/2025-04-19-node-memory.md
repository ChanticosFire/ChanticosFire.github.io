---
layout: post
title: IBM 堆叠服务器故障处理
category: 技术
tags:
  - Linux
keywords: Linux
---

近期处理了一台IBM X3950 X5堆叠服务器的故障问题，现整理处理过程如下：

设备型号：IBM X3950 X5
系统版本：RHEL 5.5
配置及硬件结构：双节点堆叠，每节点4颗10核物理CPU，共计8颗CPU，编号0-7
每节点8个内存板，每板插有2条4GB DIMM，共计16个内存板，32条4GB DIMM，总物理内存128GB。

**初始问题现象：**
用户报设备可用内存减少，IMM查看及dmidecode查看均无异常，更换可疑节点内存（从节点内存板8的两条内存）后，出现堆叠启动失败，主机卡在00 bb状态，从节点IMM接口无响应或hang死。

经过对启动顺序、启动时间的反复排查，均无法解决，检查QPI堆叠线无异常，在检查接口过程中，发现其中一QPI接口针脚处有一发泡海绵，疑似引发接触不良。
移除海绵后，主机可恢复正常堆叠状态。
疑似在初次上架或后续调整中被推进接口中，在初次配置或后续使用过程中，QPI针脚在发泡海绵处保持了较差的接触状态，但仍可通信。在再次插拔QPI线后，无法保持接触，导致无法完成堆叠握手。

**恢复堆叠后，进入系统检查内存状态**

使用dmidecode -t 17 和 grep Ssize 确认 32 条内存均 Present，状态正常，在IMM中查看全部内存条均在线，无报错。
在系统中使用numactl -H查看内存结点，发现node0-7中缺少node1和node5，free -g 和 dmesg 显示可用物理内存为 98GB

说明有约 30GB 内存未被识别（缺少两个 node，每 node 理应配 16GB）

对从节点逐块测试内存版定位灯，发现内存板4故障灯亮，更换该板两条内存后，node5成功上线，系统识别内存增至110GB。

接下来需要定位node1所属的内存板
基于 NUMA 拓扑结构分析：

每个 NUMA node ≈ 1 个 CPU + 2 块内存板

node5 → CPU5 → 从节点内存板 3、4

node1 → CPU1 → 推测应为主节点内存板 3、4


进一步验证命令：
```
dmesg | grep -i node
```
```
grep . /sys/devices/system/cpu/cpu*/topology/physical_package_id
```
```
cat /sys/devices/system/node/node*/cpulist
```

根据命令输出分析得出：

CPU0–CPU1 的核心（core0–core19）都归属 node1

剩余 CPU 对应核心被平均分配至其他 node（每 node 10 个核心）

确认 node1 绑定 CPU1，对应主节点内存板 3、4

**最终处理**

更换主节点内存板 3、4 上的4GB DIMM 后重启系统

验证：

numactl -H 显示所有 node0–node7 均已上线

总内存恢复至完整的 128GB

**NUMA Node–CPU–内存板 映射表**

| NUMA node | CPU  | 所属服务器 | 内存板编号 |
|-----------|------|-------|-------|
| node0     | CPU0 | 主节点   | 板1/2  |
| node1     | CPU1 | 主节点   | 板3/4  |
| node2     | CPU2 | 主节点   | 板5/6  |
| node3     | CPU3 | 主节点   | 板7/8  |
| node4     | CPU4 | 从节点   | 板1/2  |
| node5     | CPU5 | 从节点   | 板3/4  |
| node6     | CPU6 | 从节点   | 板5/6  |
| node7     | CPU7 | 从节点   | 板7/8  |
