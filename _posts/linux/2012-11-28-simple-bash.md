---
layout: post
title: 简洁的Bash编程技巧
category: linux
tags: [linux, bash, shell]
keywords: linux,bash,shell
description: 简洁的Bash编程技巧
---

##1. 检查命令执行是否成功

第一种写法, 比较常见

    echo abcdee | grep -q abcd
     
    if [ $? -eq 0 ]; then
        echo "Found"
    else
        echo "Not found"
    fi

简洁的写法

    if echo abcdee | grep -q abc; then
        echo "Found"
    else
        echo "Not found"
    fi

当然你也可以不要if/else, 不过这样可读性比较差

    [Sun Nov 04 05:58 AM] [kodango@devops] ~/workspace 
    $ echo abcdee | grep -q abc && echo "Found" || echo "Not found"
    Found
