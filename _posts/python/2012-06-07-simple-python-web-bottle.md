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

在上面的示例中, /login 被绑定到两个不同的回调函数上, 一个处理 GET 请求, 另一个处理 POST 请求, 第一个返我们的登陆表单, 第二个接收登陆表单提交的数据, 并进行处理, 得到结果后, 返回结果. 

##自动回退（Automatic Fallbacks）

特殊的 HEAD 方法, 经常被用来处理一些仅仅只需要返回请求元信息而不需要返回整个请求结果的事务, 这些HEAD方法十分有用, 可以让我们仅仅只获得我们需要的数据, 而不必要返回整个文档, Bottle 可以帮助我们很简单的实现这些功能, 它会将这些请求映射到与URL绑定的回调函数中, 然后自动截取请求需要的数据, 这样一来, 你不再需要定义任何特殊的 HEAD 路由了. 

##静态文件路由（Routing Static Files）

对于静态文件,  Bottle 内置的服务器并不会自动的进行处理, 这需要你自己定义一个路由, 告诉服务器在哪些文件是需要服务的, 并且在哪里可以找到它们, 我们可以写如下面这样的一个路由器:

<pre class="prettyprint linenums">
from bottle import static_file

@route('/static/:filename')
def server_static(filename):
    return static_file(filename, root='/path/to/your/static/files')
</pre>

static_file() 函数是一个安全且方便的用来返回静态文件请求的函数, 上面的示例中, 我们只返回"/path/to/your/static/files" 路径下的文件, 因为 :filename 通配符并不接受任何 "/" 的字符, 如果我们想要"/path/to/your/static/files" 目录的子目录下的文件也被处理, 那么我们可以使用一个格式化的通配符: 

<pre class="prettyprint linenums">
@route('/static/:path#.+#')
def server_static(path):
    return static_file(path, root='/path/to/your/static/files')
</pre>

##错误页面（Error Pages）

如果任何请求的URL没有的到匹配的回调函数, 那么 Bottle 都会返回错误页面, 你可以使用 error() decorator 来抓取 HTTP 状态, 并设置自己的相关回调函数, 比如下面我们的处理404错误的函数: 

<pre class="prettyprint linenums">
@error(404)
def error404(error):
    return '404 error, nothing here, sorry!'
</pre>

这个时候, 404 文件未找到错误将被上面的自定义404错误处理方法代替, 传送给错误处理函数的唯一的一个参数是一个 HTTPError 实例, 它非常将普通的 request, 所以, 你也可以有 request 中读取到, 也可以写入 response 中, 并且返回任何 HTTPError 支持的数据类型. 

##生成内容(Generating Content)

在纯粹的 WSGI中, 你的应用能返回的数据类型是十分有限的, 你必须返回可迭代的字符串, 你能返回字符串是因为字符串是可以迭代的, 但是这导致服务器将你的内容按一字符一字符的传送, 这个时候, Unicode 字符将不允许被返回了, 这是肯定不行的。

Bottle 则支持了更多的数据类型, 它甚至添加了一个 Content-Length 头信息, 并且自动编码 Unicode 数据, 下面列举了 Bottle 应用中, 你可以返回的数据类型, 并且简单的介绍了一下这些数据类型的数据都是怎么被 Bottle 处理的:

<table class="table table-bordered table-striped">
  <thead>
    <tr><th>数据类型</th><th>介绍</th></tr>
  </thead>
  <tbody>
    <tr><td>字典(Dictionaries)</td><td>Python 内置的字典类型数据将自动被转换为 JSON 字符串, 并且添加 Content-Type 为 ’application/json’ 的头信息返回至浏览器, 这让我们可以很方便的建立基于 JSON 的API</td></tr>
    <tr><td>空字符串, False, None或者任何非真的数据</td><td>Bottle 将为这类数据创建 ContentLength 头文件, 被设置为 0 返回至浏览器</td></tr>
    <tr><td>Unicode 字符串</td><td>Unicode 字符串将自动的按 Content-Type 头文件中定义的编码格式进行编码（默认为UTF8）, 接着按普通的字符串进行处理</td></tr>
    <tr><td>字节串(Byte strings)</td><td>Bottle 返回整个字符串（而不是按字节一个一个返回）, 同时增加 Content-Length 头文件标示字节串长度</td></tr>
    <tr><td>HTTPError 与 HTTPResponse 实例</td><td>返回这些实例就像抛出异常一样, 对于 HTTPError, 错误将被与相关函数处理</td></tr>
    <tr><td>文件对象</td><td>然后具有 .read() 方法的对象都被看作文件或者类似文件的对象进行处理, 并传送给 WSGI 服务器框架定义 wsgi.file_wrapper 回调函数, 某一些WSGI服务器会使用系统优化的请求方式（Sendfile）来发送文件。</td></tr>
    <tr><td>迭代器与生成品</td><td>你可以在你的回调函数使用 yield 或者 返回一个迭代器, 只要yield的对象是字符串, Unicode 字符串, HTTPError 或者 HTTPResponse 对象就行, 但是不允许使用嵌套的迭代器, 需要注意的是, 当 yield 的值第一次为非空是,  HTTP 的状态 和 头文件将被发送到 浏览器</td></tr>
  </tbody>
</table>


<pre class="prettyprint linenums">
</pre>


<pre class="prettyprint linenums">
</pre>

