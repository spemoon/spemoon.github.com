---
title:  nodejs 运用 uglifyjs 模块 做批量 js 压缩
date: 2012-08-14
categories: 
 - node.js
tags: 
 - node.js
 - uglifyjs
 - compile
---

windows底下用uglifyjs批量压缩, 考虑到bat脚本实在不适合前端=。=，

用node写个批量压缩js的函数，方便、快速，分享之！

[uglifyjs在这](https://github.com/mishoo/UglifyJS/)

``` javascript
var fs  = require('fs'); 
var jsp = require("./uglify-js").parser;
var pro = require("./uglify-js").uglify;

// 批量读取文件，压缩之
function buildOne(fileIn, fileOut) {
    if (fileIn.length > 0) {
        var finalCode = [];
        var origCode = '';
        var ast = '';
        for (var i = 0,len = fileIn.length; i < len; i++) {
            origCode = fs.readFileSync(fileIn[i], 'utf8');
            ast = jsp.parse(origCode); 
            ast = pro.ast_mangle(ast); 
            ast = pro.ast_squeeze(ast);

            finalCode.push(pro.gen_code(ast), ';');
        };
    }

    fs.writeFileSync(fileOut, finalCode.join(''), 'utf8');
}
//压缩首页js
buildOne(['../lib/slides.jquery.js', '../tpl/header_notice.tpl.js'], '../compile/home.min.js');
```

[完整代码在这](https://github.com/spemoon/UglifyJS/blob/master/compile.js)

