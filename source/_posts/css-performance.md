---
title: CSS性能调优
date: 2011-10-20
categories: 
 - css
tags: 
 - css
 - performance
---

### 简介

Web 开发中经常会遇到性能的问题，尤其是 Web 2.0 的应用。CSS 代码是控制页面显示样式与效果的最直接“工具”，但是在性能调优时他们通常被 Web 开发工程师所忽略，而事实上不规范的 CSS 会对页面渲染的效率有严重影响，尤其是对于结构复杂的 Web 2.0 页面，这种影响更是不可磨灭。所以，写出规范的、高性能的 CSS 代码会极大的提高应用程序的效率。本文主要来探讨一下如何优化，以及从哪些方面优化应用程序的 CSS 代码，从而最大限度的提高 Web 应用的性能。

CSS 代码的分析与渲染都是由浏览器来完成的，所以，了解浏览器的 CSS 工作机制对我们的优化有至关重要的作用。这篇文章我们主要从如下几个方面入手来介绍一下 CSS 的性能优化：

1.Style 标签的相关调优
2.特殊的 CSS 样式使用方式
3.CSS 缩写
4.CSS 的声明
5.CSS 选择器

### 把 Stylesheets 放在 HTML 页面头部：

浏览器在所有的 stylesheets 加载完成之后，才会开始渲染整个页面，在此之前，浏览器不会渲染页面里的任何内容，页面会一直呈现空白。这也是为什么要把 stylesheet 放在头部的原因。如果放在 HTML 页面底部，页面渲染就不仅仅是在等待 stylesheet 的加载，还要等待 html 内容加载完成，这样一来，用户看到页面的时间会更晚。

对于 @import 和 &lt;link&gt; 两种加载外部 CSS 文件的方式：@import 就相当于是把 &lt;link&gt; 标签放在页面的底部，所以从优化性能的角度看，应该尽量避免使用 @import 命令

### 避免使用 CSS Expressions：

参考下述代码：

#### 清单 1. CSS Expression 案例

``` css
Background-color: expression( (new Date()).getHours()%2 ? "#B8D4FF" : "#F08A00" )
```

Expression 只有 IE 支持，而且他的执行比大多数人想象的要频繁的多。不仅页面渲染和改变大小 (resize) 时会执行，页面滚动 (scroll) 时也会执行，甚至连鼠标在页面上滑动时都会执行。在 expression 里面加上一个计数器就会知道，expression 的执行上相当频繁的。鼠标的滚动很容易就会使 expression 的执行次数超过 10000。

##### 避免使用 Filter：

IE 特有的 AlphaImageLoader filter 是为了解决 IE6 及以前版本不支持半透明的 PNG 图片而存在的。但是浏览器在下载 filter 里面的图片时会“冻结”浏览器，停止渲染页面。同时 filter 也会增大内存消耗。最不能忍受的是 filter 样式在每个页面元素（使用到该 filter 样式）渲染时都会被浏览器分析一次，而不是像一般的背景图片渲染模式：使用过该背景图片的所有元素都是被浏览器一次性渲染的。

针对这种情况，最好的解决办法就是使用 PNG8。

##### CSS 缩写：

CSS 缩写可以让你用极少的代码定义一系列样式属性，这种做法可以极大程度的缩减代码量以达到提高性能的目的。

### 清单 2. Colour 缩写

``` css
#000000     ------>>     #000 
#336699     ------>>     #369
```

关于颜色，重复的属性值可以省略。

### 清单 3. 各种缩写方式

``` css
 Margin-top: 2px; 
 Margin-right: 5px; 
 Margin-bottom: 2em; 
 Margin-left: 15px;     ----->>     Margin: 2px 5px 2em 15px; 

 Border-width: 1px; 
 Border-style: solid; 
 Border-color: #000     ----->>     border: 1px solid #000 

 Font-style: italic; 
 Font-variant: small-caps; 
 Font-weight: bold; 
 Font-size: 1em; 
 Line-height: 140%; 
 Font-family: sans-serif;  ----->>  font: italic small-caps bold 1em 140% sans-serief 

 Background-color: #f00; 
 Background-image: url(background.gif); 
 Background-repeat: no-repeat; 
 Background-attachment: fixed; 
 Background-position: 0 0; 
  ----->>background: #f00 url(background.gif) no-repeat fixed 0 0 

 list-style-type: square; 
 list-style-position: inside; 
 List-style-image: url(image.gif)  ----->> list-style: square inside url(image.gif)
```

#### Multiple Declarations

关于 CSS 的 class 声明和定义，也有简写的方式

### 清单 4. Class 的声明

``` css
.Class1{position: absolute; left: 20px; top: 30px;} 
.Class2{position: absolute; left: 20px; top: 30px;} 
.Class3{position: absolute; left: 20px; top: 30px;} 
.Class4{position: absolute; left: 20px; top: 30px;} 
.Class5{position: absolute; left: 20px; top: 30px;} 
.Class6{position: absolute; left: 20px; top: 30px;} 


 .class1 .class2 .class3 .class4 .class5 .class6{ 
 Position: absolute; left: 20px; top: 20px; 
 }
```

这种 Class 简写的方式可以极大的缩减我们的代码，提高浏览器分析识别的效率。

## CSS 选择器 （CSS Selectors）

先来看看下面这个例子：

### 清单 5. Child selector###

``` css
#toc > li {font-weight: bold}
```

按照我们惯常的理解，编译器应该是先查找 id 为“toc”的节点，然后在他的所有直接子节点中查找类型（tag）为“li”的节点，将“font-weight”属性应用到这些节点上。

但是，不幸的是，恰恰相反，浏览器是“从右往左”来分析 class 的，它的匹配规则是从右向左来进行匹配的。这里，浏览器首先会查找页面上所有的“li”节点，然后再去做进一步的判断：如果它的父节点的 id 为“toc”，则匹配成功。

由此可知，CSS 选择器的匹配远比我们想象的要慢的多，CSS 的性能问题不容忽视。

### 清单 6. Descendant selector

``` css
#toc  li {font-weight: bold}
```

这个效率比之前的“child selector”效率更慢，而且要慢很多。浏览器先便利所有的“li”节点，然后步步上溯其父节点，直到 DOM 结构的根节点（document），如果有某个节点的 id 为“toc”，则匹配成功，否则继续查找下一个“li”节点。

### 清单 7. 尽量避免 universal rules

``` css
[hidden="true"] { ... } /* A universal rule */ 
```

这里的匹配规则很明显：查找页面上的所有节点，如果有节点存在“hidden”属性，并且其属性值为“true”，则匹配成功。这是最耗时耗力的匹配，页面上的所有节点都需要进行匹配运算，这种规则应尽量避免。

### 清单 8. Id-categorized 规则与 tag name 或 class 规则并行

``` css
button#goButton {...};----->>#goButton 
.fundation#testIcon {...};----->>#testIcon
```

这里，按照我们常规的理解，箭头左边的写法似乎是应该更快的，因为它的限制更多。其实不然，id 是全局唯一的，在匹配 CSS 选择器时浏览器定位到 id 是最快的，如果伴随有其他的非 id 的 selector，反而会影响匹配的效率。

### 清单 9. 关于 class-categorized 规则

``` css
utton.indented {...}----->>.button-indented {...}
```

程序员们经常会给某个 Class 前面加上标签名称（Tag Name），以更精确且快速的定位该节点，但是这样往往效率更差。和清单 8 中的原理一样，页面上的 class 在全局范围内来讲应该是唯一的，用唯一的 Class 名称来定位一个节点往往比组合定位更加快捷。事实上，这种做法也可以避免由于开发修改页面元素的类型（Tag）而导致的样式失效的情况，做到样式与元素的分离，两者独立维护。

### 清单 10. 尽量减少规则数量

``` css
 Span[mailfolder="true"] > table > tr > td.columnClass {...} 

 .span-mailfolder-tbl-tdCol {...}
```

规则越多，匹配越慢，上面一种规则需要进行 6 项匹配，先找“columnClass”，再找“td”，然后是“tr”，“table”，最后是符合“mailfolder”为“true”的 span，这种效率是非常慢的。如果用一个比较特殊的 class 替代（span-mailfolder-tbl-tdCol），效率会快上好几倍。

### 清单 11. 尽量避免使用 descendant selector

``` css
treehead treerow treecell {...} ----->> treehead > treerow > treecell {...}
```

Descendant 选择器是耗时相对高的选择器，通常来讲，它在 CSS 里的使用应该是尽量避免的，如果能用 child 选择器替代就应该尽量这样去做。

### 清单 12. 利用 CSS 的继承机制

``` css
 Color 
 Font 
 Letter-spacing 
 Line-height 
 List-style 
 Text-align 
 Text-indent 
 Text-transform 
 White-space 
 Word-spacing 

 #bookmark  > .menu-left {list-style-image: url(blah)} 

 ------------>>>>>>>> 

 #bookmark  {list-style-image: url(blah)}
```

在 CSS 中，有很多 CSS 的属性以可以继承的，如果某个节点的父节点已经设定了上述的 CSS 样式（如：color, font 等 ...），并且子节点无需更改该样式，则无需再作相关设定，同时，也可以利用这一点：如果很多子节点都需要设定该 CSS 属性值，可以统一设定其父节点的该 CSS 属性，这样一来，所有的子节点再无需做额外设定，加速了 CSS 的分析效率。

[原文地址](http://www.ibm.com/developerworks/cn/web/1109_zhouxiang_optcss/index.html)

