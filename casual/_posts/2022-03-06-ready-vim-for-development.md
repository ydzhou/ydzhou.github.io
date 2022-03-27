---
layout: default
title: 极简的 Vim 软件开发配置
---

首先我个人不赞同使用 SpaceVim 这类预先配置好的 IDE，因为我认为 Vim 最好的定位还是一个轻量级文本编辑器。本篇文章以极简主义为中心，提供了配置 Vim 的思路

![vim-setup-for-go]({{ site.baseurl }}/assets/vim-dev0.png)

## 文件的浏览

便捷地浏览和查询项目文件是必需的。这里介绍两类插件。

### 模糊查找

这类插件作用在于直接通过关键字能搜寻到对应文件，甚至文件中的对应段落。

#### FZF (recommended)

[FZF](https://github.com/junegunn/fzf) 是一个通用的命令行模糊查找文件的工具。你可以通过 [fzf.vim](https://github.com/junegunn/fzf.vim) 在 Vim 中使用 `:FZF` 来调出进行查询。

#### CtrlP

[CtrlP](https://github.com/kien/ctrlp.vim) 是另一个模糊查找工具。他的优势在于不像 FZF，所有的实现都在插件里，且使用 Vimscript 实现。使得它没有其他依赖，在任何系统环境都能使用。他的缺点主要在性能上，一是载入速度有明显差距，二是模糊搜索的算法不够智能。

#### Ack

[ack.vim](https://github.com/mileszs/ack.vim) 插件可以在 Vim 中直接调用 Ack 进行查找。Ack 只能通过 regex 来支持模糊查询，但是优点在于性能好，并且不同于 FZF，大多数系统环境都可以方便安装。

### 浏览文件

这类插件提供了类似 Project Drawer，使你可以通过根目录，一层层地浏览项目文件。

#### Fern (recommended)

[Fern](https://github.com/lambdalisue/fern.vim) 是一个类似 Nerdtree 的树状浏览，但是它的性能优于 Nerdtree，并且是异步插件。

#### Dirvish

[Dirvish](https://github.com/justinmk/vim-dirvish) 更偏向于 netrw。它的性能是最强的。但是 Dirvish 并没有树状浏览。

## 自动补全以及语言服务协议 LSP

语言服务协议可以让 Vim 实现对于许多语言的自动补全，跳转到定义实现以及搜索符号等。

### vim-lsp

[vim-lsp](https://github.com/prabirshrestha/vim-lsp) 是性能优良且配置简单的 LSP 插件。使用 [vim-lsp-settings](https://github.com/mattn/vim-lsp-settings) 你可以无需配置就能得到许多主流语言的支持，比如 C/Cpp，Go，Java，PHP，Python，Ruby，Typescript 等。你甚至可以使用 `:LspHover` 以弹窗的形式来查看关键字的定义。

### asyncomplete.vim
[asyncomplete.vim](https://github.com/prabirshrestha/asyncomplete.vim) 是目前我用过配置最简洁，代码最精简，性能优异的自动补全方案。和 CoC，YouCompleteMe 比，对外依赖极少，且功能轻量。目前使用下来补全速度非常快，没有任何延迟。但是对于比较大的库，会出现索引的候选词不完整的情况。比如 `aws ec2` 的库里面因为方法过多，没有办法通过前一两个字母，就筛选出所有的候选方法。

以下是 asyncomplete 使用 vim-lsp 以及 buffer 的配置：
```
" Async Completion
let g:asyncomplete_auto_popup = 1
autocmd! CompleteDone * if pumvisible() == 0 | pclose | endif
" Preview window
set completeopt=menuone,noinsert,noselect,preview

call asyncomplete#register_source(asyncomplete#sources#buffer#get_source_options({
    \ 'name': 'buffer',
    \ 'allowlist': ['*'],
    \ 'blocklist': ['markdown'],
    \ 'completor': function('asyncomplete#sources#buffer#completor'),
    \ 'config': {
    \    'max_buffer_size': 5000000,
    \  },
    \ }))
```

## 最后

如果你对于配置感兴趣，欢迎参考我的 [vimrc](https://github.com/ydzhou/dotfiles/blob/master/vim/vimrc)
