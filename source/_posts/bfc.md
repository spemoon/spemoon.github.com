---
title:  CSS BFC(Block Formatting Context)
date: 2013-05-06
categories: 
 - css
tags: 
 - css
 - bfc
---

最近这两天微博上关于前端面试这个话题又展开了讨论，BFC一词又冒出来了，写点东西纪念一下。
### BFC的定义

是 W3C CSS 2.1 规范中的一个概念，它决定了元素如何对其内容进行定位，以及与其他元素的关系和相互作用。

在创建了 Block Formatting Context 的元素中，其子元素会一个接一个地放置。垂直方向上他们的起点是一个包含块的顶部，两个相邻的元素之间的垂直距离取决于 ‘margin’ 特性。在 Block Formatting Context 中相邻的块级元素的垂直边距会折叠（collapse）。

在 Block Formatting Context 中，每一个元素左外边与包含块的左边相接触（对于从右到左的格式化，右外边接触右边）， 即使存在浮动也是如此（尽管一个元素的内容区域会由于浮动而压缩），除非这个元素也创建了一个新的 Block Formatting Context 。

### BFC到底是什么？

当涉及到可视化布局的时候，Block Formatting Context提供了一个环境，HTML元素在这个环境中按照一定规则进行布局。一个环境中的元素不会影响到其它环境中的布局。比如浮动元素会形成BFC，浮动元素内部子元素的主要受该浮动元素影响，两个浮动元素之间是互不影响的。这里有点类似一个BFC就是一个独立的行政单位的意思。

###怎样才能形成BFC

* float的值不为none。
* overflow的值不为visible。
* display的值为table-cell, table-caption, inline-block中的* 任何一个。
* position的值不为relative和static。

### BFC的作用
参看这篇blog写的demo，很详细了已经：
[demo](http://www.w3ctech.com/p/1101)