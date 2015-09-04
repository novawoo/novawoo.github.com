---
layout:     post
title:      我的Tmux向导
category:   工具
description:    我调教Tmux的一些记录
tags: [tmux, Mac]
---

在日常的工作和写作中，经常需要打开多个终端，然后在多个终端中进行切换，虽然已经配备了`iTerm2`，但还是有些不方便，为了解决这个问题，发现了`tmux`。本篇文字记录我调教`tmux`的过程和一些备忘。

## 我对tmux的要求
1. 在一个窗口中打开多个终端；
2. 能方便的在多个终端中进行切换；
3. 能方便的调整多个终端窗口的大小和整体布局；
4. 要是能记录下我的`工作环境`(比如：写博客的时候会打开多个终端，一个用`vim`写博客，一个进行`git`操作，一个用于启动`jekyll`，还有一个运行`vifm`进行文件管理)就更好了；


我的`工作环境` pic here

## tmux基础

- 基础&配置:[tmux基础](http://mingxinglai.com/cn/2012/09/tmux/)
- 常用:[tmux shortcuts & cheatsheet](https://gist.github.com/MohamedAlaa/2961058)
- 好基友:[tmux最佳伴侣：tmuxinator](http://zuyunfei.com/2013/08/09/tmuxinator-best-mate-of-tmux/)
- 扩展:[文本三巨头：zsh、tmux 和 vim](http://blog.jobbole.com/86571/)

## tmux session/window管理
### 显示存在的session和window
    > tmux ls

### rename session或windown
    > tmux rename-session -t <oldSessionName> <newSessionName>
    > tmux rename-window -t <oldWindowName> <newWindowName>

## 重中之重--tmux的配置文件
我的配置文件如下：

    # 设置前缀为Ctrl + w
    set -g prefix C-w

    # 解除Ctrl+b
    unbind C-b

    # 将r 设置为加载配置文件，并显示“reloaded!”信息
    bind -r source-file ~/.tmux.conf \; display "Reloaded!"

    # split window
    unbind '"'
    # vertical split (<prefix> -)
    bind - splitw -v
    unbind %
    # horizontal split (<prefix> |)
    bind | splitw -h

    # select panel，方便在panel间切换，按键类似于vim
    # up
    bind k select-pane -U
    # down
    bind j select-pane -D
    # left
    bind h select-pane -L
    # right
    bind l select-pane -R

    # resize panel
    # upward
    bind -r K resizep -U 5
    # downward
    bind -r J resizep -D 5
    # leftward
    bind -r H resizep -L 5
    # rightward
    bind -r L resizep -R 5

    # copy-mode 将快捷键设置为vi模式
    setw -g mode-keys vi
    # enter copy mode (<prefix> Escape)
    bind Escape copy-mode
    # select (v)
    bind -t vi-copy v begin-selection
    # copy (y)
    bind -t vi-copy y copy-selection
    # paste buffer (prefix Ctrl+p)
    bind Ctrl-p pasted

    # zoom pana <-> window
    # http://tmux.svn.sourceforge.net/viewvc/tmux/trunk/examples/tmux-zoom.sh
    # bind ^z run "tmux-zoom"
    # 新版本已经自带这个功能  <prefix> z

    # app
    # htop (<prefix> !)
    bind ! splitw -h htop
    # man (<prefix> m)
    bind m command-prompt "splitw -h 'exec man %%'"


## 版本修订记录
 V1 2015-09-01 初始版本
