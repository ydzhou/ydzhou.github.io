---
title: 记录自己安装 Debian 11
date: 2021-10-01
tags: ['linux', 'debian']
---

## 为什么选用 Debian？

众所周知，Linux 发行版千千万万，从最流行的 ubunutu 到 big brain 的 gentoo，让每一个人都会问出这个问题。

我最开始接触的 Linux 是 Ubuntu，但我不太喜欢 ubuntu 捆绑的软件和过多修改。加上工作中的服务器部署也是 ubuntu 居多，所以就退而求其次，使用了 Debian。

Debian 的包都非常老，不适合折腾，但是也绝不会突然崩坏。如果只是普通开发，日常使用，甚至玩游戏，那么 Debian 是很好的选择。

## 基本设置

虽然说 Debian 安装非常傻瓜化，但是仍有一些需要手动配置的。

### Sudo 你自己

切成 Root 帐号，运行，
```
/sbin/usermod -aG sudo {your-username}
```

### 添加 non-free 和 contrib repo

修改 `/etc/apt/sources.list` 给里面 4 个 entry 都添加 `non-free contrib`。

再运行 `sudo apt update`。

### （作死）切换成 testing repo

是的，Debian 也可以滚动更新。

同上，将 Debian 的 repo 改成下一代即可。改动完成后运行 `sudo apt full-upgrade`。Debian 收纳包是，
```
upstream -> unstable -> testing -> stable (freeze)
```

因为 Ubuntu 是基于 Debian unstable，理论上 Debian testing 稳定性已经超过了许多发行版，可以放心使用。

### Nvidia 显卡

没办法，N 卡开源驱动太拉垮，不能糟蹋了 money。

先确保你配置了 `non-free` repo，然后运行 `sudo apt install nvidia-detect`。

再运行 `nvidia-detect` 并根据提示选择对应驱动安装。

## 开发环境配置

### Pyenv

我使用 `pyenv` 管理 python 版本。

通过 [pyenv installer](https://github.com/pyenv/pyenv-installer) 来安装，
```
curl https://pyenv.run | bash
```

安装结束后，你需要添加 `pyenv` 到 `PATH`,
```
$(pyenv root)/shims:/usr/local/bin:/usr/bin:/bin
```

`openssl` 需要事先装好才能安装 python，
```
sudo apt install libssl-dev
```


