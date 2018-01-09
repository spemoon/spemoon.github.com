---
title:  windows 下的运行 uglifyjs
date: 2012-05-28
categories: 
 - node.js
tags: 
 - nodejs
 - uglifyjs
---

现在node在windows底下安装的方式越来越简便了，只需要一个msi文件，参看<a href="http://nodejs.org/" target="_blank">node的官网</a>。

装完后内置npm。

然后要安装的uglifyjs的话，只需打开cmd窗口。输入：
``` bash
npm install uglify-js -g --prefix="C:\Program Files\nodejs"
```
当然上面这个路径是node的安装路径，可自行变更。

使用的话，只需敲命令：
``` bash
uglifyjs animate.js > animate.min.js
```
这样既可。

当然，你也可以写脚本来批量处理。

