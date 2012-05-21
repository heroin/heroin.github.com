---
layout: post
title: erlang配置emacs开发
category: ide
tags: [erlang, emacs, ide]
---

打开emacs配置文件, 输入配置文件

    (setq load-path (cons "*erlang安装目录*/lib/tools-2.6.6.6/emacs" load-path))
    (setq erlang-root-dir "*erlang安装目录*")
    (setq exec-path (cons "*erlang安装目录*/bin" exec-path))
    (require 'erlang-start)

用emacs打开任意一个`*.erl`源文件, 在菜单栏将会看到`Erlang`菜单选项.

打开erlang终端`C-c C-z`

编译文件`C-c C-k`
