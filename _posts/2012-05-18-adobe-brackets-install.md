---
layout: post
title: Adobe Brackets 安装体验
category: ide
tags: [adobe, brackets, ide, javascript, html, css]
---

Brackets 是 Adobe 的开源 `HTML/CSS/JavaScript` 集成开发环境. Brackets 提供 Windows 和 OS X 平台支持.

要想试用先得`clone`以下几个项目

[https://github.com/adobe/brackets](https://github.com/adobe/brackets)  
brackets 执行文件(`win/mac`)  
[https://github.com/adobe/brackets-app](https://github.com/adobe/brackets-app)  

brackets 所依赖的js库  
[https://github.com/jblas/path-utils](https://github.com/jblas/path-utils)  
[https://github.com/adobe/CodeMirror2](https://github.com/adobe/CodeMirror2)  
[https://github.com/laktek/jQuery-Smart-Auto-Complete](https://github.com/laktek/jQuery-Smart-Auto-Complete)  
[https://github.com/douglascrockford/JSLint](https://github.com/douglascrockford/JSLinthttps://github.com/laktek/jQuery-Smart-Auto-Complete)


    git clone git://github.com/adobe/brackets.git
    git clone git://github.com/adobe/brackets-app.git

    git clone git://github.com/jblas/path-utils.git
    git clone git://github.com/adobe/CodeMirror2.git
    git clone git://github.com/douglascrockford/JSLint.git
    git clone git://github.com/laktek/jQuery-Smart-Auto-Complete.git


![](/assets/blog/adobe-brackets-install/dir.png)

####安装步骤
> 将`brackets`内全部文件移动到`brackets-app/brackets`
> 将`CodeMirror2`内全部文件移动到`brackets-app/brackets/src/thirdparty/CodeMirror2`
> 将`path-utils`内全部文件移动到`brackets-app/brackets/src/thirdparty/path-utils`
> 将`jQuery-Smart-Auto-Complete`内全部文件移动到`brackets-app/brackets/src/thirdparty/smart-auto-complete`
> 将`JSLint`内全部文件移动到`brackets-app/brackets/src/thirdparty/jslint`