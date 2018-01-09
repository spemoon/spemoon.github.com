---
title:  前端开发的优化问题
date: 2011-12-18
categories: 
 - fe
tags: 
 - front-end
 - optimization 
 - performance
---

1. 减少http请求次数：css spirit,data uri
2. JS，CSS源码压缩
3. 前端模板 JS+数据，减少由于HTML标签导致的带宽浪费，前端用变量保存AJAX请求结果，每次操作本地变量，不用请求，减少请求次数
4. 用innerHTML代替DOM操作，减少DOM操作次数，优化javascript性能
5. 用setTimeout来避免页面失去响应
6. 用hash-table来优化查找
7. 当需要设置的样式很多时设置className而不是直接操作style
8. 少用全局变量
9. 缓存DOM节点查找的结果
10. 避免使用CSS Expression
11. 图片预载
12. 避免在页面的主体布局中使用table，table要等其中的内容完全下载之后才会显示出来，显示比div+css布局慢

