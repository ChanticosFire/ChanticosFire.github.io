---
layout: post
title: 
category: 技术
tags:
  - 技巧
  - Google
keywords: Google
---
### 摘录Google SRE中，Effective Troubleshooting（有效的故障排除）一节

Written by Chris Jones
##### Common Pitfalls  常见陷阱

Ineffective troubleshooting sessions are plagued by problems at the Triage, Examine, and Diagnose steps, often because of a lack of deep system understanding. The following are common pitfalls to avoid:  
无效的故障排除会话会受到 Triage、Examine 和 Diagnose 步骤中问题的困扰，这通常是由于缺乏对系统的深入了解。以下是需要避免的常见陷阱：

- Looking at symptoms that aren’t relevant or misunderstanding the meaning of system metrics. Wild goose chases often result.  
    查看不相关的症状或误解系统指标的含义。经常导致野鹅追逐。
- Misunderstanding how to change the system, its inputs, or its environment, so as to safely and effectively test hypotheses.  
    误解如何改变系统、其输入或环境，以便安全有效地检验假设。
- Coming up with wildly improbable theories about what’s wrong, or latching on to causes of past problems, reasoning that since it happened once, it must be happening again.  
    想出关于问题所在极不可能的理论，或者抓住过去问题的原因，推理既然它发生过一次，它就必须再次发生。
- Hunting down spurious correlations that are actually coincidences or are correlated with shared causes.  
    寻找实际上是巧合或与共同原因相关的虚假相关性。

Fixing the first and second common pitfalls is a matter of learning the system in question and becoming experienced with the common patterns used in distributed systems. The third trap is a set of logical fallacies that can be avoided by remembering that not all failures are equally probable—as doctors are taught, “when you hear hoofbeats, think of horses not zebras.”[61](https://sre.google/sre-book/effective-troubleshooting/#id-GbduJSWtpTeiW) Also remember that, all things being equal, we should prefer simpler explanations.[62](https://sre.google/sre-book/effective-troubleshooting/#id-D04IaFmtGTziJ)  
修复第一个和第二个常见陷阱是学习相关系统并熟练使用分布式系统中使用的常见模式的问题。第三个陷阱是一组逻辑谬误，可以通过记住并非所有失败的可能性都相同来避免——正如医生被教导的那样，“当你听到蹄声时，想想马而不是斑马。[61](https://sre.google/sre-book/effective-troubleshooting/#id-GbduJSWtpTeiW) 也要记住，在所有条件相同的情况下，我们应该选择更简单的解释。[62](https://sre.google/sre-book/effective-troubleshooting/#id-D04IaFmtGTziJ)

Finally, we should remember that correlation is not causation:[63](https://sre.google/sre-book/effective-troubleshooting/#id-kD2uEF7hnT7io) some correlated events, say packet loss within a cluster and failed hard drives in the cluster, share common causes—in this case, a power outage, though network failure clearly doesn’t cause the hard drive failures nor vice versa. Even worse, as systems grow in size and complexity and as more metrics are monitored, it’s inevitable that there will be events that happen to correlate well with other events, purely by coincidence.[64](https://sre.google/sre-book/effective-troubleshooting/#id-4DBukIohGT7ir)  
最后，我们应该记住，相关性不是因果关系：[63](https://sre.google/sre-book/effective-troubleshooting/#id-kD2uEF7hnT7io) 一些相关事件，例如集群内的数据包丢失和集群中的硬盘驱动器故障，具有共同的原因 — 在本例中，停电，尽管网络故障显然不会导致硬盘驱动器故障，反之亦然。更糟糕的是，随着系统规模和复杂性的增加，以及监控的指标越来越多，不可避免地会有一些事件恰好与其他事件密切相关，这纯粹是巧合。[64](https://sre.google/sre-book/effective-troubleshooting/#id-4DBukIohGT7ir)

Understanding failures in our reasoning process is the first step to avoiding them and becoming more effective in solving problems. A methodical approach to knowing what we do know, what we don’t know, and what we need to know, makes it simpler and more straightforward to figure out what’s gone wrong and how to fix it.  
了解我们推理过程中的失败是避免失败并更有效地解决问题的第一步。一种有条不紊的方法来了解我们确实知道什么、我们不知道什么以及我们需要知道什么，这使得弄清楚哪里出了问题以及如何解决它变得更简单、更直接。