---
layout: page
title: justjavac的博客
keywords: justjavac, web开发
description: justjavac的个人博客， 探讨目前最时髦的web开发技术。
tagline: 专注最时髦的web开发技术
---
{% include JB/setup %}

<script type="text/javascript"><!--
google_ad_client = "ca-pub-5061364613986259";
/* justjavac 页首横幅 */
google_ad_slot = "6421375412";
google_ad_width = 728;
google_ad_height = 90;
//-->
</script>
<script type="text/javascript"
src="http://pagead2.googlesyndication.com/pagead/show_ads.js">
</script>

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
    {% for rpost in site.posts limit: 6 %}
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