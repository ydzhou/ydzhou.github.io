---
title: Debian, always rocking
date: 2022-04-23
---

## Why Debian？

> It just works.

Stability of Debian distro has nothing left to argue. The software can be far behind latest version. The default configurations can be lacking. And you may not have the most active community. But hey, it just works and it will not easily break.

## Basic Setup

### Add yourself into sudoer

Switch to root first, then run
```
/sbin/usermod -aG sudo {your-username}
```

### Add non-free and contrib repos

Edit `/etc/apt/sources.list` to add `non-free contrib` for existing 4 entries.

Then run
```
sudo apt update
```

### Nvidia Driver

Make sure you have `non-free` repo setup.

```
sudo apt install nvidia-detect
```

Then run `nvidia-detect` and follow the instruction to install proper driver package.

## Dev Environment Setup

### Pyenv setup

I use `pyenv` manage python environments

I use [pyenv installer](https://github.com/pyenv/pyenv-installer) to install
```
curl https://pyenv.run | bash
```

After installation, you need to add `pyenv` into `PATH`,
```
$(pyenv root)/shims:/usr/local/bin:/usr/bin:/bin
```

`openssl` is required to install python
```
sudo apt install libssl-dev
```


