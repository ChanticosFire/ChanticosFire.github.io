---
title: 使用Tasker自动备份文件
layout: post
category: 技术
tags:
  - Android
keywords: Android
---
### Tasker 操作流程文档：自动备份程序的 `.log` 日志文件

由于要调试一个会自动覆盖日志的App，借用Android应用Tasker编写了一个自动备份日志的任务，每 10 秒检查指定目录下是否有包含当天日期的 `.log` 格式日志文件，将这些文件备份到目标目录，并在文件不存在或访问受限时继续执行任务而不中断。
我的Tasker应用语言使用的是英语，所以接下来的步骤中涉及到应用内功能名称的均使用原英文名称，你可以在右上角的首选项中暂时更改语言以跟随教程。

---

### **任务概述**

- **任务名称**: `AutoBackupLogs`
- **目标**: 每 10 秒检查一次指定目录中是否有包含当天日期的 `.log` 格式日志文件，并将其备份到目标目录。如果没有找到文件，任务将忽略错误继续执行。

### **操作流程**

#### **步骤 1：创建任务**

1. 打开 Tasker，进入 “Tasks” 选项卡，点击 `+` 按钮创建一个新任务，并命名为 `AutoBackupLogs`。

#### **步骤 2：分割日期**

1. **Variable Split**:
    - **Action**: `+` > `Variables` > `Variable Split`
    - **Name**: `%DATE`
    - **Splitter**: `-`
    - **作用**: 将系统日期按 `-` 分割成年、月、日，存储在 `%DATE1`、`%DATE2`、`%DATE3`。

#### **步骤 3：确保月份和日期为两位数**

1. **Variable Set (月份)**:
    
    - **Name**: `%DATE2`
    - **To**: `0%DATE2`
    - **If**: `%DATE2 < 10`
2. **Variable Set (日期)**:
    
    - **Name**: `%DATE3`
    - **To**: `0%DATE3`
    - **If**: `%DATE3 < 10`

#### **步骤 4：组合日期为 `YYYYMMDD` 格式**

1. **Variable Set**:
    - **Name**: `%DateToday`
    - **To**: `%DATE1%DATE2%DATE3`
    - **作用**: 组合日期为 `YYYYMMDD` 格式，并存储在 `%DateToday` 变量中。

#### **步骤 5：列出匹配的日志文件**

1. **List Files**:
    - **Directory**: 设置为 `/storage/emulated/0/MyAppLogs`（虚构路径，用于存放源程序生成的日志文件）。
    - **Match**: `*%DateToday*.log`
    - **Sort Select**: 选择 `Modification Date, Reverse` 以确保按修改日期降序排列，最新文件在最前面。
    - **Variable Array**: `%FilesMatched`
    - **Use Root**: 根据需要决定是否启用，如果目录访问权限受限，尝试禁用此选项。

#### **步骤 6：条件判断是否存在匹配的文件**

1. **If 条件**:
    - **Action**: `+` > `Task` > `If`
    - **Condition**: `%FilesMatched ~ *`
    - **作用**: 仅当存在匹配的文件时，才执行后续的复制操作。
	- **注意**：完成`If`的创建并返回后，会出现弹窗让你选择仅创建`If`还是同时创建`End If`，在此步骤请点击`End If`。

#### **步骤 7：备份日志文件**

1. **Copy File**:
    - **From**: `%FilesMatched1`（仅复制列表中的第一个文件，即最新日志文件）。
    - **To**: `/storage/emulated/0/BackupLogs`（虚构路径，用于存放备份的日志文件）。
    - **Continue Task After Error**: 启用此选项，以确保在没有文件匹配时任务继续执行而不会中止。
	- 创建完成Copy File后，长按此模块将其拖到If和End If中间。

#### **步骤 8：等待 10 秒**

1. **Wait**:
    - **Action**: `+` > `Task` > `Wait`
    - **Seconds**: 10

#### **步骤 9：循环任务**

1. **Goto**:
    - **Action**: `+` > `Task` > `Goto`
    - **Type**: `Action Number`
    - **Number**: 1（或第一步的编号）
    - **作用**: 返回任务的开头，继续循环检查。

### **指定路径的注意事项**

- 在指定源路径和目标路径时，务必填写完整路径。例如，将路径设置为 `/storage/emulated/0/MyAppLogs` 和 `/storage/emulated/0/BackupLogs`。虽然 Tasker 参数中显示的路径可能只是相对路径 `MyAppLogs` 或 `BackupLogs`，但在实际调用时使用的是完整路径。**不要手动填写相对路径**，以免出现路径解析问题。

### **常见问题及解决方案**

- **文件未找到错误**:
    
    - 通过启用 Copy File模块中的`Continue Task After Error` 选项，可以让任务在没有匹配文件时继续执行而不停止。
- **访问受限错误**:
    
    - 如果使用 Root 权限访问特定目录时出现问题，尝试禁用 `Use Root` 选项。
- **文件排序问题**:
    
    - 在List Files模块中启用 `Modification Date, Reverse` 来确保最新的文件在列表的最前面，以确保只备份最新生成的日志文件。