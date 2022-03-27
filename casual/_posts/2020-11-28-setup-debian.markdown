---
layout: default
title: 在 Debian 10 上玩游戏
---

## PC Spec

* CPU: Intel 10th-gen i5
* Graphic Card: MSI Nvidia RTX 2070 Duke
* Motherboard: Gigabyte Z490 mini-ITX
* RAM: Corsair 16x2 GB
* Hard Drive: Samsung 1TB SSD
* Power Supply: Corsair 600W Plat SFX PSU

## FAQ
### 第一次装机无法启动进入图形界面

Debian 安装盘里自带的驱动都非常老旧，这使得一般较新的显卡，都没法在第一次安装完时运作。我们需要自行更新显卡驱动。在 grub 画面按 `e`，然后把下面这句加在以 `linux` 开头的那行,

```
systemd.unit=multi-user.target
```
按 ctrl+x 继续启动并以命令行界面登陆。这里你可能需要先获得 sudo 权限来运行下面命令，添加 non-free repo。

```
sudo apt edit-resources
```

将一下写进配置文件每行末尾，
```
non-free
```

更新 API 源，然后安装 Nvidia 驱动。此外，你也可以通过 backported 来安装更新的驱动，但这不是必须的。
```
sudo apt update
sudo apt install nvidia-driver
```

### 用户没有 sudo 权限
```
Run this to add your `username` into sudo list
```
### TCS Deadline Error

```
sudo apt install intel-microcode
```

### 找不到 wifi 硬件

```
apt install firmware-iwlwifi
```

### 切换系统语言
先配置想要的语言 locale
```
sudo dpkg-reconfigure locales
```

再去 Settings/Region and Language 里选择这个语言并且重新登陆。

### 安装 Steam

Debian 有自己的 repo 安装 Steam，但是配置繁琐且存在 bug（libsol丢失），而且版本也偏老旧。这里我们选择通过 Flatpak 来安装 Steam。先安装 Flatpak，
```
sudo apt install flatpak
flatpak remote-add --if-not-exists \
    flathub https://flathub.org/repo/flathub.flatpakrepo
```

然后安装 Gnome 插件，之后你就可以在商店找到 Steam 安装选项。此外，你也可以通过命令行安装。
```
sudo apt install gnome-software-plugin-flatpak
```

### 安装中文输入法

```
sudo apt install ibus ibus-rime ibus-libpinyin
```

