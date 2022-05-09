---
title: Ready Debian 11
date: 2021-10-01
tags: ['linux', 'debian']
---

## Why Debian？

Stability of Debian distro has nothing left to argue. The software can be far behind latest version. The default configurations can be lacking. And you may not have the most lousy community. But hey, it just works and it will not give you any surprise. There is a old saying,

> Debian unstable is more stable than most of stable releases of other distros.

## Basic Setup

Even though Debian tends to work out of box, it still requires some efforts to properly setup.

### Sudo yourself

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

### Pyenv

I use `pyenv` to manage different python versions.

It can be installed via [pyenv installer](https://github.com/pyenv/pyenv-installer),
```
curl https://pyenv.run | bash
```

After installation, you need to add `pyenv` into `PATH`,
```
$(pyenv root)/shims:/usr/local/bin:/usr/bin:/bin
```

`openssl` is required to install python,
```
sudo apt install libssl-dev
```
