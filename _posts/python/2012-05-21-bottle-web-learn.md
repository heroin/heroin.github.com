---
layout: post
title: 使用bottle搭建web
category: python
tags: [python, bottle]
---

##安装bottle

访问bottle官网, [http://bottlepy.org](http://bottlepy.org/)

先在github上clone源码到本地进行安装

    git clone https://github.com/defnull/bottle.git
    cd bottle
    python setup.py install

安装完后打开python终端测试是否安装成功

    >>> import bottle
    >>> bottle.__version__
    '0.10.9'

能显示出版本号, 说明安装成功

##使用bottle进行开发

新建一个`.py`文件, 
