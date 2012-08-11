---
layout: post
title: "关于 tmux"
description: ""
category:
tags: [terminal, tmux]
---
{% include JB/setup %}

在了解 tmux 之前我一直都用一个叫 Terminator 的终端模拟器。较之于 GNOME Terminal, Terminator 提供了分屏功能和更多的快捷键。而在某天终于下决心研究了一下 tmux 后，我果断把 Terminator 扔进了 /dev/null。

Tmux 除了可以分屏、分窗口之外，还可以把不同用处的分屏和窗口布局用 session 进行分组。更棒的是 tmux 提供了丰富的配置项让我们来对它进行定制。还有它的 session 共享的功能，可以让两个人同时连到同个 server 的 tmux session，使两个人可以看到互相的操作。总之这又是一个神器。

另外还要介绍一下的就是 [Tmuxinator](https://github.com/aziz/tmuxinator)。一般情况下，我们每次启动 tmux 后都会习惯性得打开几个窗口并运行一些命令，这些重复性的操作都可以通过 tmux 的命令写成脚本来自动执行。而 Tmuxinator 则简化了这个写脚本的过程，把每个 session 的配置通过 yml 文件来描述。

最后我参考[《tmux - Productive Mouse-Free Development》](http://book.douban.com/subject/10541112/) 这本书写了一个自己的 tmux 配置放在 [GitHub](https://github.com/yesmeck/tmuxrc) 上。

截图：

[<img src="http://dl.dropbox.com/u/1658623/tmux.png" width="800" />](http://dl.dropbox.com/u/1658623/tmux.png)
