---
title: Vim Plugins
tags: ['vim', 'coding']
date: 2022-03-06
---

本文着重推荐多款优秀的适用于 Vim 软件开发的 Plugin。本文没有考虑仅兼容 neovim 的 Plugin。下面的截图展示了由 nnn.vim，vim-lsp，asyncomplete.vim 等 Plugin 组成的 Go 开发环境。

![vim-setup-for-go](/vim-dev0.png)

## 文件浏览

文件浏览的 Plugin 有两种大类。

1. 路径浏览
2. 模糊查找

### 模糊查找

这类 Plugin 可以通过关键词快速地查询文件，Buffers 或者文件内容

#### FZF.vim

[fzf.vim](https://github.com/junegunn/fzf.vim) 使用 [FZF](https://github.com/junegunn/fzf) 进行查询。

优势：
1. 可以快速查阅大型目录。查询可以在目录没有完全加载下开始
1. 最好的模糊匹配，可以不按照关键字顺序，甚至包含错误的关键字
1. 可以自定义文件初次查找工具，比如使用 `fd` 加快速度

不足：
1. 需要依赖 FZF
1. Plugin 弹出的响应有明显延迟

如果你需要查询大量文件，或者不想牢记关键字，FZF 是最佳选择。

#### CtrlP

[CtrlP](https://github.com/kien/ctrlp.vim) 是另一款类似的模糊查询 Plugin。

优势：
1. 不需要任何对外依赖，轻量化
2. Plugin 弹出的响应优秀
3. 支持自定义 matcher 来匹配潜在搜索结果来提升性能
4. 界面和 Vim 融合更好，且默认支持 nerd font 美化文件图标

不足：
1. 查阅大型目录卡顿明显，且需要漫长加载时间
2. 匹配算法 naive，使用的是简单的 regex，word length 以及 levenshtein distance 来匹配

如果你的工程文件数量小（1000项以下），且你比较熟悉里面的文件，CtrlP 是没有问题的。如果你需要远程工作，作业机器没有网络，CtrlP 就是不二选择。

### 路径浏览

这类 Plugin 可以按路径一层层浏览文件，比较经典的就是树状（tree）。也可以进行复制，剪切，删除，创建等基本文件操作。

#### Fern

[Fern](https://github.com/lambdalisue/fern.vim) 是一个类似 Nerdtree 的树状浏览 Plugin，它使用异步加载和渲染。

优势：
1. 功能齐全且可定制性高，有很多 Fern 专属 Plugin 可以使用，比如 nerd-font，netrw-hijack
1. 操作设计和 IDE 接近，容易上手

不足：
1. 项目比较新且还在不断开发中，Bug 偏多
1. 代码架构欠佳，包括不少对于异步的使用，显得 bloated

如果你习惯了大多数 IDE 的文件浏览，或者 NerdTree，那么 Fern 就是最为推荐（也大概是唯一的）的 Tree Plugin。

#### Dirvish

[Dirvish](https://github.com/justinmk/vim-dirvish) 和 Netrw 类似, KISS 的完美体现。

优势：
1. 速度最快的浏览插件，大型目录（1000+）无延迟打开
1.  内核代码极为精练，极轻量化
1.  功能单一，专注于文件浏览

不足：
1. 功能简陋，可定制性差

如果你习惯了 Netrw 或者想要一个轻量化的文件浏览 Plugin，Dirvish 是不二选择。

#### NNN.vim

[nnn.vim](https://github.com/mcchrish/nnn.vim) 使用 [nnn](https://github.com/jarun/nnn) 浏览操作文件。

优势：
1. 使用 nnn 作为后端，性能优异且功能齐全
不足：
1. 对 gVim 支持不佳，需要在 terminal 使用 Vim
1. 需要依赖 nnn
1. 按键 mapping 和 Vim 不兼容，交互感不佳

如果你偏爱在 terminal 下使用 Vim，nnn.vim 是最好的选择。

## 自动补全以及语言服务协议 LSP

语言服务协议可以让 Vim 实现对于许多语言的自动补全，跳转到定义实现以及搜索符号等。这类插件虽然也有不少，比如 CoC，或者语言类专用的比如 vim-go，但是我只推荐以下这个 Plugin。

#### vim-lsp

[vim-lsp](https://github.com/prabirshrestha/vim-lsp) 是性能优良且配置简单的 LSP 插件。使用 [vim-lsp-settings](https://github.com/mattn/vim-lsp-settings) 你可以无需配置就能得到许多主流语言的支持，比如 C/Cpp，Go，Java，PHP，Python，Ruby，Typescript 等。你甚至可以使用 `:LspHover` 以弹窗的形式来查看关键字的定义。

#### asyncomplete.vim
[asyncomplete.vim](https://github.com/prabirshrestha/asyncomplete.vim) 是目前我用过配置最简洁，代码最精简，性能优异的自动补全方案。和 CoC，YouCompleteMe 比，对外依赖极少，且功能轻量。目前使用下来补全速度非常快，没有任何延迟。但是对于比较大的库，会出现索引的候选词不完整的情况。比如 `aws ec2` 的库里面因为方法过多，没有办法通过前一两个字母，就筛选出所有的候选方法。

## 最后

如果你对于配置感兴趣，欢迎参考我的 [vimrc](https://github.com/ydzhou/dotfiles/blob/master/vim/vimrc)
