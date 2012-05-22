---
layout: post
title: jekyll-bootstrap添加代码高亮
category: javascript
tags: [bootstrap, jekyll, javascript, prettify, jquery]
---

jekyll-bootstarp是用`markdown`写博文的, 但是markdown默认不支持`pre`加class属性

但是, 支持直接在`markdown`中写`html`代码, [传送门](http://daringfireball.net/projects/markdown/syntax#html)

修改`/includes/themes/twitter/default.html`, 添加几行代码

<pre class="prettyprint linenums">
&lt;link href="\{\{ ASSET_PATH \}\}/google-code-prettify/prettify.css" rel="stylesheet" type="text/css" media="all"&gt;
&lt;script type="text/javascript" src="http://ajax.googleapis.com/ajax/libs/jquery/1.7.2/jquery.min.js"&gt;&lt;/script&gt;
&lt;script type="text/javascript" src="\{\{ ASSET_PATH \}\}/google-code-prettify/prettify.js"&gt;&lt;/script&gt;
&lt;script type="text/javascript" src="\{\{ ASSET_PATH \}\}/js/application.js"&gt;&lt;/script&gt;
</pre>
