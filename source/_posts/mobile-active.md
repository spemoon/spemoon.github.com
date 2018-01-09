---
title:  移动端开发技巧 —— active失效
date: 2014-01-05
categories: 
 - css
tags: 
 - css
 - mobile
---

之前在移动端写相关的active样式，没有任何响应，后来发现[stackoverflow](http://stackoverflow.com/questions/3885018/active-pseudo-class-doesnt-work-in-mobile-safari), 有人也遇到一样的问题，解决方案如下：

最简单写法：
在body上附加属性ontouchstart=""
``` html
<body ontouchstart="">
    ...
</body>
```

当然你也可以在响应的需要体现active的地方附加属性ontouchstart=""
``` html
<a ontouchstart="">Click me</a>
```
