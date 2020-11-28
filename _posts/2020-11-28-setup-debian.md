---
layout: post
title: A Guide to Install Debian 10 for My Gaming Machine
---

## Machine Specification

* CPU: Intel 7th-gen i5
* Graphic Card: MSI Nvidia RTX 2070 Duke
* Motherboard: Gigabyte Z97 mini-ITX
* RAM: Corsair 4x2 GB
* Hard Drive: Samsung 1TB SSD
* Power Supply: Silverstone 500W Gold SFX PSU

## FAQ

### Cannot boot up into Graphical Desktop Environment After Installation

Debian has poor support for modern graphic cards due to its outdated driver collection. It is most likely that you cannot boot into graphical desktop environment (e.g. gnome). When you first boot up the system, press `e` in the Grub page and append this to the line starting with `linux`,

```
systemd.unit=multi-user.target
```

Press ctrl+x to continue the boot-up into command line session. To be noticed, you may need to add your user into sudo group to get root permission for following setups.
```
sudo adduser ${username} sudo
```

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

First make sure you include `non-free` repo. Then enable mutli-arch since steam is a i386 app.
```
sudo dpkg --add-architecture i386
sudo apt update
sudo apt install steam
```

Run this to ensure Vulkan and 32-bit support,
```
sudo apt install mesa-vulkan-drivers libglx-mesa0:i386 mesa-vulkan-drivers:i386 libgl1-mesa-dri:i386
```

If you run into error `libGL.so.1 is missing`, run this to install legacy driver to pull in this lib,
```
sudo apt install libgl1-nvidia-glx:i386
```
