---
layout: post
title: maven 上传 插件
category: java
tags: [java, mavan, upload, deploy, plugin]
keywords: java, mavan, upload, deploy-maven-plugin
description: maven上传插件
---

## 配置方式

<pre class="prettyprint linenums">
&lt;plugin&gt;
    &lt;groupId&gt;so.heroin.maven.plugins&lt;/groupId&gt;
    &lt;artifactId&gt;deploy-maven-plugin&lt;/artifactId&gt;
    &lt;version&gt;1.0.0.0&lt;/version&gt;
    &lt;configuration&gt;
        &lt;hostname&gt;192.168.1.12&lt;/hostname&gt;
        &lt;username&gt;root&lt;/username&gt;
        &lt;password&gt;root&lt;/password&gt;
        &lt;port&gt;22&lt;/port&gt;
        &lt;remotePath&gt;/root/code&lt;/remotePath&gt;
    &lt;/configuration&gt;
&lt;/plugin&gt;
</pre>

### 配置描述

<table class="table table-bordered table-striped">
  <thead>
    <tr><th>配置项</th><th>说明</th></tr>
  </thead>
  <tbody>
    <tr><td>hostname</td><td>服务器ip</td></tr>
    <tr><td>username</td><td>服务器帐号</td></tr>
    <tr><td>password</td><td>服务器密码</td></tr>
    <tr><td>port</td><td>服务器ssh端口, 默认22</td></tr>
    <tr><td>remotePath</td><td>上传服务器地址, 默认/tmp</td></tr>
  </tbody>
</table>

其中`port`和`remotePath`是有默认值的, 可以不填写
