---
layout: default
title: A Guide to Install Debian 10 for My Gaming Machine
---

## Machine Specification

* CPU: Intel 10th-gen i5
* Graphic Card: MSI Nvidia RTX 2070 Duke
* Motherboard: Gigabyte Z490 mini-ITX
* RAM: Corsair 16x2 GB
* Hard Drive: Samsung 1TB SSD
* Power Supply: Corsair 600W Plat SFX PSU

## FAQ

### Cannot boot up into Graphical Desktop Environment After Installation

Debian has poor support for modern graphic cards due to its outdated driver collection. It is most likely that you cannot boot into graphical desktop environment (e.g. gnome). When you first boot up the system, press `e` in the Grub page and append this to the line starting with `linux`,

```
systemd.unit=multi-user.target
```

Press ctrl+x to continue the boot-up into command line session. To be noticed, you may need to add your user into sudo group first.

Run following commands to add non-free repo
```
sudo apt edit-resources
```

Append this in the repo config file
```
non-free
```

The run this to update APT repo and install Nvidia card driver
```
sudo apt update
sudo apt install nvidia-driver
```

### User Cannot Use Sudo

Run this to add your `username` into sudo list
```
sudo adduser ${username} sudo
```

### Getting TCS Deadline Error During First Bootup

Install Intel microcode
```
sudo apt install intel-microcode
```

### No Wifi Device with iwlwifi error

Run this,
```
apt install firmware-iwlwifi
```

### Change to Other Languages

Run this to add desired locales,
```
sudo dpkg-reconfigure locales
```

Then select the new locale in Settings/Region and Language, and logout

### Install Steam

It is super easy to install steam through flatpak.

First install flatpak via
```
sudo apt install flatpak
flatpak remote-add --if-not-exists \
    flathub https://flathub.org/repo/flathub.flatpakrepo
```

Optionally you can install flatpak gnome shop plugin,
```
sudo apt install gnome-software-plugin-flatpak
```

### Install Chinese input method

```
sudo apt install ibus ibus-rime ibus-libpinyin
```

