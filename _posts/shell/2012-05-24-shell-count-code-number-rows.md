---
layout: post
title: 统计代码行数脚本
category: shell
tags: [shell, linux, script, sed]
---

执行以下代码

<pre class="prettyprint linenums">
find . -name '*.java' -type f -exec cat {} \; | sed '/^$/d;/^[ ]*$/d;/.*#$/d' | wc -l
</pre>

其中`'*.java'`可以换成其他语言的扩展名
