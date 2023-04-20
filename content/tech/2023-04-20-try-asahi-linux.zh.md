 ---
title: Asahi Linux 初体验：Linux + Apple M1
tags: ['linux']
date: 2023-04-20
---

## 体验

Asahi （旭りんご） Linux 是第一款能在 M1 Mac 上运行的 Linux。Asahi 是一个对 Apple Scilicon 提供支持的项目，包括了硬件支持，驱动和其他工具。Asahi 发行版基于 Arch Linux，也是我在自己的 M1 Mac 上安装的版本。

![full-width](/asahi_mac.jpg)

#### 软件

由于 Asahi Linux 基于 Arch Linux，`pacman` 和 AUR 都可以正常使用。实际系统软件体验和普通的 Arch Linux 无差别。X11 和 wayland 都很好的支持。开发者推荐搭配最新的显卡驱动使用 wayland 环境。我使用了轻量的桌面 DWL。官方也提供了完整 KDE 的安装选项。唯一需要注意的是很多 AUR 的包不支持 ARM 架构，需要自己手动编译。

#### 硬件

M1 Mac 整体体验很亮眼。支持蓝牙，我的 Magic Mouse 有和 Mac OSX 里一样的体验；开发者最近也发布了显卡驱动，可以支持到 OpenGL 2.0的标准。即使没有显卡，单纯靠软解，基本浏览网页日常代码也非常流畅。Mac 休眠也没有问题。CPU 的风扇管理也正常。基本上 Linux 常见的坑都很好地在 Asahi 上避免了。

目前暂不支持内置音响和屏幕亮度调整。

## 安装步骤

根据官网的 script 安装 [alpha release](https://asahilinux.org/2022/03/asahi-linux-alpha-release)：

```
curl https://alx.sh | sh
```

中间会重启一次，记得开机时长按电源键进入启动盘选项然后选择 Asahi Linux 继续完成安装

## 安装后系统初步设置

因为我安装的是 minimal 版本 Arch Linux，所以需要先配置无线网卡

```
systemctl enable dhcpd
systemctl start dhcpd
systemctl enable iwd
systemctl start iwd
```

然后使用 `iwctl` [配置 Wifi](https://wiki.archlinux.org/title/iwd)

```
[iwd]# device list
[iwd]# device wlan0 set-property Powered on
[iwd]# station wlan0 scan
[iwd]# station wlan0 get-networks
[iwd]# station wlan0 device connect {SSID}
```

`wlan0` 是你的网卡设备，可能名称会不同。 

之后可以为鼠标键盘继续[设置蓝牙](https://wiki.archlinux.org/title/bluetooth)。

