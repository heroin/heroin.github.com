---
layout: post
title: 在github上搭建博客
category: javascript
tags: [github, bootstrap, jekyll, javascript]
---

###注册github帐号
在github上注册帐号, 如果你的帐号为`heroin`
创建`heroin.github.com`这个项目.


###安装jekyll
安装`jekyll`到github上, 这里我用的是
[Jekyll-Bootstrap][]
  [Jekyll-Bootstrap]: http://jekyllbootstrap.com/

执行下列`git`命令

    $ git clone https://github.com/plusjade/jekyll-bootstrap.git heroin.github.com
    $ cd heroin.github.com
    $ git remote set-url origin git@github.com:heroin/heroin.github.com.git
    $ git push origin master

然后直接访问<http://heroin.github.com>, 就能访问到你搭建的博客了.

###配置jekyll

修改`_config.yml`文件

将一些基础信息配置成想要的内容

####配置首页
先贴下我的首页配置代码, jekyllbootstrap默认的首页是`index.md`

但是如果需要分页效果的话需要使用的是`index.html`, 并且修改`_config.yml`, 添加一个配置项`paginate: 5`

我是用的`index.html`

    ---
    layout: page
    title: Heroin blog
    tagline: 
    ---
    {% include JB/setup %}

    <div class="row">
      <div class="span8">
      {% for post in paginator.posts %}
      {% assign content = post.content %}
        <article>
          <header>
          <h1><a href="{{ BASE_PATH }}{{ post.url }}">{{ post.title }}</a></h1>
          <div class="date">{{ site.author.name }} 发表于 <span>{{ post.date | date:"%Y-%m-%d" }}</span></div>
        </header>
        <div class="content">{{ content | truncatewords:40 }}</div>
        </article>
      {% endfor %}
      <div class="pagination">
          <ul>
          {% if paginator.next_page %}
            <li class="prev"><a href='/page{{ paginator.next_page }}'>&larr; Older</a></li>
          {% endif %}
            <li><a href="{{ BASE_PATH }}{{ site.JB.archive_path }}">归档</a></li>
          {% if paginator.previous_page %}
            <li class="next"><a href='/page{{ paginator.previous_page}}'>Newer &rarr;</a></li>
          {% endif %}
          </ul>
      </div>
      </div>
      
      <aside class="span4">

        <section>
        <h4>最近发表</h4>
        <ul id="recent_posts">
        {% for rpost in site.posts limit: 5 %}
          <li class="post">
            <a href="{{ BASE_PATH }}{{ rpost.url }}">{{ rpost.title }}</a>
          </li>
        {% endfor %}
        </ul>
        </section>
        
      {% unless site.categories == empty %}
        <h4>分类</h4>
        <ul class="tag_box">
          {% assign categories_list = site.categories %}
          {% include JB/categories_list %}
        </ul>
      {% endunless %}  
        
      </aside>
    </div>





###添加文章
在`_posts`目录下新建一个`markdown`(`*.md`)文件,
文件命名规范是`yyyy-mm-dd-url`, 例如该文章的文件为`2012-05-18-github-blog-jekyll-bootstrap.md`

得到的访问路径却是
[/javascript/2012/05/18/github-blog-jekyll-bootstrap/][]
  [/javascript/2012/05/18/github-blog-jekyll-bootstrap/]: /javascript/2012/05/18/github-blog-jekyll-bootstrap/
其中`/javascript`是在markdown文件中配置的.


markdown文件头需要几个配置, 以下是该文章的头配置

    ---
    layout: post
    title: 在github上搭建博客
    category: javascript
    tags: [github, bootstrap, jekyll, javascript]
    ---

每个markdown必须在头部加上这段. 然后下面直接写markdown代码就行了.
