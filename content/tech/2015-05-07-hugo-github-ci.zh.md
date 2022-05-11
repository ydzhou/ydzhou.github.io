---
title: Github Hugo 搭建个人博客
date: 2022-05-07
tags: ['hugo']
---

这个博客一直使用 Github Pages 托管。之前用默认支持的 Jekyll 搭建。但是因为 Jekyll 在 Apple Scillion（M1）下编译有问题，所以决定换成 Hugo。

介于网上很多类似资料实际上并不是官方推荐做法（hacky workarounds），所以在这里分享下 onboarding 过程中需要注意到的或者可能出现问题的地方。

### 部署到 Github Pages

Hugo 原理是在 `/public` 文件夹下生成一个静态的网站。所以最“笨”的方法就是在本地编译，然后把生成的 `/public` 和源码一起上传到 Github。

还有一个更加便捷的方法，就是使用 Github Action。

我们需要 2 个 git branch，一个存我们的源码，一个存生成的静态网站。Github Action 需要做的就是从我们制定的源码 branch 运行 `hugo server`，然后把生成好的文件放入静态网站的 branch。然后我们设置 Github Pages 由这个 branch 读取就可以啦。
