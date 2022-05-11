---
title: 记录自己安装 Debian 11
date: 2021-10-01
tags: ['linux', 'debian']
---

4月，2022: 更新至 Debian 11

## 为什么选用 Debian？

上大学的时候第一次接触的是 Ubuntu 8.04（记得是一只长颈鹤）。中间曾经因为工作环境使用 `yum` 包管理而转为 Fedora，但是现在工作中又使用 `apt` 包管理了（Ubuntu 在云计算推广真的做得太好了），所以又换了回去。另外不太喜欢 Ubuntu 乱塞包和捆绑软件，就一直在用 Debian。

Debian 是一个非常 old fashion 的发行版，就是那种很难让你兴奋起来，但也很难给你突然崩坏。虽然 Debian 的包都非常老旧，但是基本任务其实完成起来障碍不大。只是比如你想尝新，可能会很麻烦。

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

再运行 `nvidia-detect` 并根据提示选择对应驱动安装。一般为，
```
sudo apt install nvidia-driver
```

### 无线网卡 Wifi

```
sudo apt install firmware-iwlwifi
```

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


