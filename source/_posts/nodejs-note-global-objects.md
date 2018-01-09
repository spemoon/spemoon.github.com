---
title:  Node.js学习笔记——Global Objects 全局对象 
date: 2011-12-07
categories: 
 - javascript
tags: 
 - javascript
 - nodejs 
 - globe object
---

### global

在浏览器中，顶级作用域为全局作用域，在全局作用域下通过var something即定义了一个全局变量。但是在Node中并不如此，顶级作用域并非是全局作用域，在Node模块中通过var something定义的变量仅作用于该模块。

### require.resolve()

使用内部函数require()的机制查找一个模块的位置，而不用加载模块，只是返回解析后的文件名。

### require.paths 

require()的搜索路径数组，你可以修改该数组添加自定义的搜索路径。

例如：将一个新的搜索路径插入到搜索列表的头部。
``` javascript
require.paths.unshift('/usr/local/node');
```

### __filename 

当前正在执行的脚本的文件名。这是一个绝对路径，可能会和命令行参数中传入的文件名不同。

例如：在目录/Users/mjr下运行node example.js
``` javascript
console.log(__filename);
// /Users/mjr/example.js
```

### __dirname

当前正在执行脚本所在的目录名。

例如：在目录/Users/mjr下运行node example.js
``` javascript
console.log(__dirname);
// /Users/mjr
```

### module

指向当前模块的引用。特别的，当你通过module.exports和exports两种方式访问的将是同一个对象，参见src/node.js。
