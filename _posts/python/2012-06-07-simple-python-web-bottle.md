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
5. 最后, run() 函数启动服务器, 并且我们设置它在 "localhost" 和 8080 端口上运行

上面这种方法仅仅只是展示一下下 Bottle 的简单, 我们还可以像下面这样, 创建一个 Bottle 对象 app, 然后我们会将所有的函数都映射到 app 的 URL 地址上, 如上示例我们可以用下面这种办法来实现: 

<pre class="prettyprint linenums">
#!/usr/bin/env python
# -*- coding: UTF-8 -*-

from bottle import Bottle, run

app = Bottle()
@app.route('/hello')
def hello():
    return "Hello World!"
run(app, host='localhost', port=8080)
</pre>

Bottle 的这种 URL 地址映射方法与我一直使用的 Flask 的地址映射方法很相似, 到现在为止, 我似乎只看到它们只是语法上面有些话不同. 

##路由器(Request Routing)

Bottle 应用会有一个 URL 路由器, 它将 URL 请求地址绑定到回调函数上, 每请求一些 URL, 其对应的 回调函数就会运行一些, 而回调函数返回值将被发送到浏览器, 你可以在你的应用中通过 route() 函数添加不限数目的路由器. 

<pre class="prettyprint linenums">
#!/usr/bin/env python
# -*- coding: UTF-8 -*-

from bottle import route

@route('/')
@route('/index.html')
def index():
    return '&lt;a href="/hello"&gt;Go to Hello World Page&lt;/a&gt;'

@route('/hello')
def hello():
    return 'Hello World'
</pre>

就像你看到, 你所发出的访问请求(URL), 应用并没有返回服务器上真实的文件, 而是返回与该URL绑定的函数的返回值, 如果其一个URL没有被绑定到任何回调函数上, 那么 Bottle 将返回"404 Page Not Found"的错误页面

##动态路由(Dynamic Routes)

Bottle 有自己特有的 URL 语法, 这让我们可以很轻松的在 URL 地址中加入通配符, 这样, 一个 route 将可以映射到无数的 URL 上, 这些动态的 路由常常被用来创建一些有规律性的内容页面的地址, 比如博客文章地址"/archive/1234.html"或者"/wiki/Page_Title", 这在上面的示例我已经演示过了, 还记得吗?

<pre class="prettyprint linenums">
@route('/hello/:name')
def hello(name = 'World'):
    return 'Hello {}!'.format(name)
</pre>

上面的路由器, 可以让我们通过"/hello/abc"或者"/hello/heroin"等地址来访问, 而 Bottle 返回的内容将是"Hello abc!" 或者 "Hello heroin!", "/hello/"之后的字符串交被返回来, 默认的通配符将匹配所有下一个"/"出现之前的字符. 我们还可以对通配符进行格式化: 

<pre class="prettyprint linenums">
@route('/object/:id#[0-9]+#')
def view_object(id):
    return 'Object ID: {}'.format(id)
</pre>

上面的路由将只允许 id 为由数字"0-9"组成的数字, 而其它的字符串都将返回 404 错误. 

##HTTP 请求方法(Request Methods)

HTTP 协议为不同的需求定义了许多不同的请求方法, 在 Bottle 中, GET方法将是所有未指明请求访问的路由会默认使用的方法, 这些未指明方法的路由都将只接收 GET 请求, 要处理如 POST, PUT 或者 DELETE 等等的其它请求, 你必须主动地在 route() 函数中添加 method 关键字, 或者使用下面这些 decorators：@get(), @post(), @put(), @delete(). 

POST 方法在经常被用来处理 HTML 的表单数据, 下面的示例演示了一个 登陆表单的处理过程：

<pre class="prettyprint linenums">
#!/usr/bin/env python
# -*- coding: UTF-8 -*-

from bottle import get, post, request

#@route('/login')
@get('/login')
def login_form():
    return '''&lt;form method = "POST"&gt;
                &lt;input name="name" type="text" /&gt;
                &lt;input name="password" type="password" /&gt;
                &lt;input type="submit" value="Login" /&gt;
                &lt;/form&gt;'''

#@route('/login', method = 'POST')
@post('/login')
def login():
    name = request.forms.get('name')
    password = request.forms.get('password')
    if check_login(name, password):
        return '&lt;p&gt;Your login was correct&lt;/p&gt;'
    else:
        return '&lt;p&gt;Login failed&lt;/p&gt;'
</pre>

在上面的示例中, /login 被绑定到两个不同的回调函数上, 一个处理 GET 请求, 另一个处理 POST 请求, 第一个返我们的登陆表单, 第二个接收登陆表单提交的数据，并进行处理, 得到结果后, 返回结果. 


<pre class="prettyprint linenums">
</pre>


<pre class="prettyprint linenums">
</pre>


<pre class="prettyprint linenums">
</pre>


<pre class="prettyprint linenums">
</pre>

