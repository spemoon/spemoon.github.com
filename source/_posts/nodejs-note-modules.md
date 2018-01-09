---
title:  Node.js学习笔记——Modules 模块 
date: 2011-12-09
categories: 
 - javascript
tags: 
 - javascript
 - nodejs 
 - modules
---

Node使用CommonJS模块系统。Node有一个简单的模块装载系统，在Node中，文件和模块是一一对应的。下面的例子展示了foo.js文件如何在相同的目录中加载circle.js模块。

foo.js的内容为：

``` javascript
var circle = require('./circle.js');
console.log( 'The area of a circle of radius 4 is ' + circle.area(4));
```

circle.js的内容为：

``` javascript
var PI = Math.PI;

exports.area = function (r) {
    return PI * r * r;
};

exports.circumference = function (r) {
    return 2 * PI * r;
};
```

circle.js模块输出了area()和circumference()两个函数，为了以对象的形式输出，需将要输出的函数加入到一个特殊的exports对像中。

模块的本地变量是私有的。在上面的例子中，变量PI就是circle.js私有的。

###Core Modules 核心模块###

Node有一些编译成二进制的模块。

核心模块在node源代码中的lib文件夹下。

核心模块总是被优先加载，如果它们的标识符被require()调用。例如，require('http')将总是返回内建的HTTP模块，即便又一个同名文件存在。

###File Modules 文件模块###

如果没有找到确切的文件名，node将尝试以追加扩展名.js后的文件名读取文件，如果还是没有找到则尝试追加扩展名.node。.js文件被解释为JavaScript格式的纯文本文件，.node文件被解释为编译后的addon（插件）模块，并使用dlopen来加载。

以'/'为前缀的模块是一个指向文件的绝对路径，例如require('/home/marco/foo.js')将加载文件/home/marco/foo.js。

如果标明一个文件时没有 '/' 或 './'前缀，该模块或是"核心模块"，或者位于 node_modules目录中。

###Loading from 'node_modules' Folders 从 'node_modules' 目录中加载###

如果传递到 require()的模块标识符不是一个核心模块，并且不是以'/'，'../'或'./'开头，node将从当前模块的父目录开始，在其/node_modules子目录中加载该模块。

如果在那里没有找到，就转移到上一级目录，依此类推，直到找到该模块或到达目录树的根结点。

例如，如果在文件 '/home/ry/projects/foo.js'中调用 `require('bar.js')，node将会依次查找以下位置：

* /home/ry/projects/node_modules/bar.js
* /home/ry/node_modules/bar.js
* /home/node_modules/bar.js
* /node_modules/bar.js

这允许程序本地化他们的依赖关系，避免发生冲突。

###Optimizations to the 'node_modules' Lookup Process 优化 'node_modules' 的查找过程###

如果有很多级的嵌套信赖，文件树会变得相当的长，下面是对这一过程的一些优化。

首先， /node_modules不要添加到以 /node_modules结尾的目录上。

其次，如果调用require()的文件已经位于一个node_modules层次中，最上级的node_modules目录将被作为搜索的根。

例如，如果文件'/home/ry/projects/foo/node_modules/bar/node_modules/baz/quux.js'调用require('asdf.js')，node会在下面的位置进行搜索：

* /home/ry/projects/foo/node_modules/bar/node_modules/baz/node_modules/asdf.js
* /home/ry/projects/foo/node_modules/bar/node_modules/asdf.js
* /home/ry/projects/foo/node_modules/asdf.js

###Folders as Modules 目录作为模块###

很方便将程序或库组织成自包含的目录，并提供一个单独的入口指向那个库。有三种方式可以将一个子目录作为参数传递给 require() 

第一种方法是在目录的根下创建一个名为package.json的文件，它指定了一个main 模块。一个package.jso文件的例子如下面所示：
``` javascript
{ "name" : "some-library",
  "main" : "./lib/some-library.js" }
```

如果此文件位于./some-library目录中，require('./some-library')将试图加载文件./some-library/lib/some-library.js。

这是Node感知package.json文件的范围。

如果在目录中没有package.json文件，node将试图在该目录中加载index.js 或 index.node文件。例如，在上面的例子中没有 package.json文件，require('./some-library')将试图加载：

* ./some-library/index.js
* ./some-library/index.node

###Caching 缓存###

模块在第一次加载后将被缓存。这意味着（类似其他缓存）每次调用require('foo')如果解析到相同的文件，那么将返回同一个对象。

