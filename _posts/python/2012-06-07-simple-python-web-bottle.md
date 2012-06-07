---
layout: post
title: 微型 Python Web 框架 Bottle
category: python
tags: [python, bottle, web]
keywords: web框架,python,bottle
description: Bottle 是一个非常小巧但高效的微型 Python Web 框架, 它被设计为仅仅只有一个文件的Python模块, 并且除Python标准库外, 它不依赖于任何第三方模块.
---

Bottle 是一个非常小巧但高效的微型 Python Web 框架, 它被设计为仅仅只有一个文件的Python模块, 并且除Python标准库外, 它不依赖于任何第三方模块.

- 路由(Routing): 将请求映射到函数, 可以创建十分优雅的 URL
- 模板(Templates): Pythonic 并且快速的 Python 内置模板引擎, 同时还支持 mako, jinja2, cheetah 等第三方模板引擎
- 工具集(Utilites): 快速的读取 form 数据, 上传文件, 访问 cookies, headers 或者其它 HTTP 相关的 metadata
- 服务器(Server): 内置HTTP开发服务器, 并且支持 paste, fapws3, bjoern, Google App Engine, Cherrypy 或者其它任何 WSGI HTTP 服务器

##安装 Bottle

正如上面所说的,  Bottle 被设计为仅仅只有一个文件, 我们甚至可以不安装它, 直接将 bottle.py 文件下载并复制到我们的应用中就可以使用了, 这是一个好办法, 但是如果还是想将其安装, 那么我们可以像安装其它的 Python 模块一样: 

    sudo easy_install -U bottle

如果我们直接将 bottle.py 下载到自己的应用中的话, 我们可以建立下面这样的目录结构: 

    + application
    +----bottle.py
    +----app.py

我们可以将下面的创建 Bottle 实例的示例代码复制到 app.py 文件中, 运行该文件即可. 

###示例: Bottle 的 "Hello World" 程序

下面的代码我们创建了一个十分简单但是完整的 Bottle 应用程序(在Python Consle)中

    >>> from bottle import route, run
    >>> @route('/hello/:name')
    ... def index(name = 'World'):
    ...     return '<strong>Hello {}!'.format(name)
    ... 
    >>> run(host='localhost',port=8080)
    Bottle server starting up (using WSGIRefServer())...
    Listening on http://localhost:8080/
    Use Ctrl-C to quit.

在 Python Consle中输入上面的代码, 我们就得到了一个最简单但完整的 Web 应用, 访问: "http://localhost:8080/hello/bottle" 试试. 上面到底发生了什么?

1. 首先, 我们导入了两个 Bottle 的组件,  route() Decorator 和 run() 函数
2. route() 可以将一个函数与一个URL进行绑定, 在上面的示例中, route 将 "/hello/:name" 这个URL地址绑定到了 "index(name = 'World')" 这个函数上
3. 这个是一个关联到 "/hello" 的 handler function 或者 callback , 任何对 "/hello" 这个URL的请求都将被递交到这个函数中
4. 我们获得请求后, index() 函数返回简单的字符串
5. 最后, run() 函数启动服务器, 并且我们设置它在 "localhost” 和 8080 端口上运行





<pre class="prettyprint linenums">
</pre>

