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


###添加文章
在`_posts`目录下新建一个`markdown`(`*.md`)文件,
文件命名规范是`yyyy-mm-dd-url`, 例如该文章的文件为`2012-05-18-github-blog-jekyll-bootstrap.md`
