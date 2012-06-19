---
layout: post
title: linux 安装 svn 服务
category: linux
tags: [linux, svn, subversion]
keywords: linux,svn,svn服务,subversion
description: linux下安装subversion
---

##安装subversion

linux下直接输入命令安装subversion

debian

    apt-get install subversion

redhat

    yum install subversion

##创建svn库

安装完subversion后使用`svnadmin`创建一个库

例如

    svnadmin create /var/svn/repo

创建完毕后会生成几个目录
- conf
- db
- hooks
- locks

还会创建2个文件
- README.txt
- format