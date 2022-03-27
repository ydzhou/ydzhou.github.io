---
layout: default
title: OSX 安装 Vim 的中文支持
---

OSX 系统自带的 Vim 安装在 `/usr/bin/vim`。现在 Vim 已经可以自动匹配系统的 locale 来切换菜单界面的语言。但是我发现不同于 MacVim，自带的 Vim 不会使用除英文以外的语言。

在检查了 `vim --versino` 后，我发现虽然 `multi_lan` 特性是编译了，理论上已经有多语言的界面文件。但是 `gettext` 特性没有编译，于是 Vim 无法调用其他语言包。

所以 OSX 自带的 Vim 是不支持中文显示的。解决方法有两种，
1. `brew install vim`：安装的 Vim 支持中文显示，并自动会替换系统自带的 Vim。
2. `/Applications/MacVim.app/Contents/bin`：使用 MacVim 自带的 Vim，也支持中文显示。你需要让 `vim` 命令指向这个 Vim。
