---
layout: post
title: linux tomcat 脚本
category: linux
tags: [shell, linux, tomcat, script]
keywords: linux,tomcat,start,stop,script,shell
description: linux下tomcat启动关闭脚本
---

<pre class="prettyprint linenums">
#!/bin/bash

PID=`ps -ef | grep tomcat | grep java | grep ClassLoaderLogManager | grep -v grep | awk '{print $2}'`

tomcat_log() {
  tail -f $CATALINA_HOME/logs/catalina.out
}

</pre>
