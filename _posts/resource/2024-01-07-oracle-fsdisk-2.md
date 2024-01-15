---
layout: post
title: Oracle FS1-2 降级的连接
category: 技术
tags:
  - 存储
keywords: Storage
---
“外部”驱动器将出现系统警报，并且驱动器状态在 GUI 和 fscli 输出中都显示为“外部”。

通常，当更换故障驱动器或将驱动器机箱添加到 FS1-2 系统时，会发生这种情况。 在驱动器箱或驱动器组中执行恢复操作时也可能遇到此问题。

FS1-2 软件要求手动接受任何它事先不了解的驱动器。

注意：接受“外部”驱动器将使此驱动器上的任何现有数据无法访问。

在 FS1 系统中更换故障驱动器后，新驱动器可能会显示“外部”状态，如下图所示：
![Foreign disk](/assets/img/fsforeigndisk.jpg)
  

这个新的“外部”驱动器还不是驱动器组的成员，必须先接受它，然后才能在配置中使用：

- Using the GUI: 使用 GUI：
    1. Open FS System Manager, go to System --> Alerts and Events --> System Alerts  
        打开 FS 系统管理器，转到系统 --> 警报和事件 --> 系统警报
    2. Right click on the Foreign Disk Drive alert in the right pane and select Manage.  
        右键单击右窗格中的“外部磁盘驱动器”警报，然后选择“管理”。
    3. Check the box "Accept Foreign Drive" and click OK.  
        选中“接受外部驱动器”框，然后单击确定。
使用 FSCLI：

1.  通过 FS1-2 浏览器用户界面下载最新版本的 fscli。请参阅文档 1991938.1 FS 系统：如何获取和安装 fscli 工具软件。
2.  使用 fscli 以管理员身份登录：
```
使用 fscli 以管理员身份登录：  

C:\>fscli login -u administrator -oracleFs <_FQN or IP of FS1-2_>  
C：\>fscli login -u administrator -oracleFs
```

接受外部驱动器（此示例使用驱动器框 05、驱动器 15）：  
```
C:\> fscli enclosure -modify -enclosure /ENCLOSURE-05 -acceptDrive 15  
```

如果外部驱动器是由驱动器更换引起的，则在接受驱动器后将开始复制，并在完成后将其变为“正常”。  

如果外部驱动器是由添加机箱引起的，您还将看到驱动器组不在存储域中的警报。将它们添加到存储域将解决此问题。
