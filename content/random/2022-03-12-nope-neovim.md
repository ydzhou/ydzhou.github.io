---
title: Nope, Neovim
date: 2022-03-12
tags: ['vim','rant']
draft: true
---

我认为 neovim 是一个失败的产品，或者说，是一个不懂 Unix 哲学的产品，那就是

> Keep it simple，stupid

很多时候，我们常探讨为什么需要改变，我一直认同这句话，

> If it works, it works

如果 Vim 能做到，那为什么去改变？那 Neovim 又做出了什么改变？

## 功能越多，越好吗？

正如官网所言，Neovim 正在努力把自己做成一个 IDE。我认为是可悲的，因为这和 Vim 只是一个文字编辑器的初衷背道而驰。首先，如果你真的需要 IDE，为什么不去用 Jetbrain，Visual Studio Code 这些从根本上就是使用 IDE，面向强劲处理器和现代操作系统，集合以图形界面为首的人机交互来主导开发的软件呢？

Vim 本身就是为终端设计的一个文字编辑器，如果把 Vim 做成 IDE，就好像要在沙漠种出一片片包谷地，有点啼笑皆非。Vim 本身的核心人机交互设计，比如 insert/normal mode，实际就是为了去掉对于鼠标的支持，一种精简的做法。但是代价必然是当功能趋于复杂，单靠键盘是无法准确方便地进行操作。

另外 Vim 自己的 Vimscript，被诟病学习成本，难以编程以及性能不佳。但是关键在于，Vimscript 不需要任何其他语言编译器的支持，这也是 KISS 原则的体现。现在 Neovim 主导用 Lua 配置，让我百思不得其解，Neovim 的开发者就好像从来只是在配置齐全的桌面操作系统里工作一样。这实在是对 Vim 运行环境多样性的严重削弱。

## 微服务思想下的插件

Neovim 提出的更为大胆的思路，就是

> API is first-class: discoverable, versioned, documented

这个论调在互联网，特别是微服务下，非常的普遍。但问题是，我们是在试图设计一个巨无霸吗？微服务的优势在于业务逻辑复杂多变，将每个领域分而治之，可以更加灵活。但是对于文字编辑器，这是我们需要考虑的吗？

不得不承认，LSP 的成功让很多桌面应用开发者开始克意引入前后端概念，将大量功能从核心抛离，正如 Neovim 提到的，

> Remote plugins run as co-processes, safely and asynchronously

我觉得这更多的是东施效颦，到底效率能提高多少，客户体验能优化多少，并没有看到什么数据。但是我更相信 Neovim 的开发者只是盲目跟随趋势，缺乏对于 Vim 设计的深刻理解。

不难想象，未来 Neovim 插件可以发展的愈发复杂，什么煮咖啡，甚至语音识别，我都相信不成问题。但这还是 Vim 吗？

## 我想对 Neovim 说不

我相对 Neovim 说不。我理解 Neovim 的愿景。这其实就像我工作中接触过的很多讨论，对于老旧的系统，我们总能跳出成百上千个缺憾，并想去解决这些不足。但是发现缺点容易，承认优点很难。Neovim 就是这样的例子，它盲目地把 Vim 和其他优秀的现代 IDE 作比较，过于集中地去解决这些比较种的不足，缺忽视了 Vim 本身的优点。

> Vim is a highly configurable text editor

Vim 是一个可高定制化的文字编辑器，但它终究只是一个编辑器。Neovim 这个项目，更像是打造一个类似 Vim 编辑风格的 IDE，而并非完善 Vim 或者做出 Vim 2.0
