---
layout: post
title: kdeconnect的故障排查
category: 技术
tags:
  - Linux
keywords: Linux
---
刚刚遇到一个问题，简单排查之后解决掉了，记录一下。

kalilinux端的kdeconnect打开之后显示为空白页面，编辑本机主机名不生效，排除了Wayland导致的显示问题后，尝试在shell中使用`kdeconnect-cli`命令查看相关信息，执行`kdeconnect-cli --refresh`命令刷新设备列表，但出现了如下报错：

```
QSocketNotifier: Can only be used with threads started with QThread error activating kdeconnectd: QDBusError("org.freedesktop.DBus.Error.ServiceUnknown", "The name org.kde.kdeconnect was not provided by any .service files")
```

将命令更换为`kdeconnect-app`观察打开GUI时的报错信息，得到如下报错：

```
QSocketNotifier: Can only be used with threads started with QThread error activating kdeconnectd: QDBusError("org.freedesktop.DBus.Error.ServiceUnknown", "The name org.kde.kdeconnect was not provided by any .service files") error activating kdeconnectd: QDBusError("org.freedesktop.DBus.Error.ServiceUnknown", "The name org.kde.kdeconnect was not provided by any .service files") kdeconnect.interfaces: dbus interface not valid error activating kdeconnectd: QDBusError("org.freedesktop.DBus.Error.ServiceUnknown", "The name org.kde.kdeconnect was not provided by any .service files") error activating kdeconnectd: QDBusError("org.freedesktop.DBus.Error.ServiceUnknown", "The name org.kde.kdeconnect was not provided by any .service files") kdeconnect.interfaces: dbus interface not valid kdeconnect.interfaces: dbus interface not valid error activating kdeconnectd: QDBusError("org.freedesktop.DBus.Error.ServiceUnknown", "The name org.kde.kdeconnect was not provided by any .service files") file:///usr/lib/x86_64-linux-gnu/qt5/qml/org/kde/kdeconnect/DBusProperty.qml:48: ReferenceError: write is not defined
```

  
这个错误信息表明 KDE Connect 在尝试通过 D-Bus 与其守护进程（kdeconnectd）进行通信时遇到问题。这些错误通常与 D-Bus 配置有关，可能是因为 KDE Connect 的 D-Bus 服务文件没有正确安装或配置。
可以尝试以下办法：
1. **检查 KDE Connect 安装**： 确保 KDE Connect 已正确安装。可以尝试重新安装或更新 KDE Connect。
2. **检查 D-Bus 服务文件**： 确保 `/usr/share/dbus-1/services/org.kde.kdeconnect.service` 文件存在并且内容正确。这个文件通常是由 KDE Connect 的安装过程创建的。
3. **重启 D-Bus 服务**： 虽然表面上需求是重启D-Bus服务，但实际上重启D-Bus服务会导致系统hung住，真正的解决办法是重启系统。
4. **检查系统日志**： 查看系统日志（如 `/var/log/syslog` 或使用 `journalctl` 命令）以获取更多错误信息，可能有助于诊断问题。
5. **检查 Qt 环境**： 错误信息中提到了 Qt（`QSocketNotifier`），确保系统安装了所有必要的 Qt 库，并且环境没有配置错误。

```
#移除kdeconnect
sudo apt-get remove --purge kdeconnect
#清除配置文件和缓存文件
rm -rf ~/.config/kdeconnect
rm -rf ~/.local/share/kdeconnect
rm -rf ~/.cache/kdeconnect

#重新安装kdeconnect
sudo apt-get update 
sudo apt-get install kdeconnect
```

重新安装后，尝试启动，GUI页面和报错信息仍和之前一样，接下来检查D-Bus服务文件。

```
┌──(user㉿hostname)-[~] └─# cat /usr/share/dbus-1/services/org.kde.kdeconnect.service 
cat: /usr/share/dbus-1/services/org.kde.kdeconnect.service: 没有那个文件或目录 
┌──(user㉿hostname)-[~] └─# cat /usr/share/dbus-1/services/org.kde.kdeconnect.service.original 
[D-BUS Service] Name=org.kde.kdeconnect Exec=/usr/lib/x86_64-linux-gnu/libexec/kdeconnectd
```

很显然我们没有`org.kde.kdeconnect.service`这个文件，而此目录下存在一个内容正确的`org.kde.kdeconnect.service.origina`文件，接下来就很简单了。

```
#将重命名的文件复制回原始位置：
cp /usr/share/dbus-1/services/org.kde.kdeconnect.service.original /usr/share/dbus-1/services/org.kde.kdeconnect.service

#重启系统
reboot
```

重启，再次启动`kdeconnect-cli`和`kdeconnect-app`，均正常工作，GUI恢复正常，能够配对到局域网内其它设备，问题解决。

本次故障排查结论：

KDE Connect 的正常运行依赖于正确配置的 D-Bus 服务文件。如果遇到类似问题，检查相关的服务文件是否存在和正确配置是一个很好的起点。
