---
title: IE与FF对盒模型的不同解析
date: 2011-09-20
categories: 
 - css
tags: 
 - IE
 - FF
 - padding
 - margin
 - width
 - resolve
---

首先，单纯从页面显示的角度来说，IE和FF的和模型的现实宽度/高度，都等于width+padding+border+margin宽度/高度总和。

但是在剥削的属性里，FF仍然是本身width属性的宽度，而IE的width属性width+padding+border+margin宽度/高度总和

下面举例说明：

IE和FF对盒模型的解释也不一样，代码说明：

test { width: 650px !important;width: 648px;padding-left:2px;background:#fff; }

test 显示的宽度是 650px

IE Box的总宽度是：width+padding+border+margin宽度总和

FF Box的总宽度就是 width的宽度，padding+border+margin的宽度在含在width内。

如果有BOX{width:"300px"; padding:"5px";}

则BOX在IE的宽度应该为:310px

而在FF的宽度则是:300px

所以在BOX有填充的情况下应该这样使用

BOX{width:”290″!IMPORTANT; width: “300″;}

这样子才能确保BOX的宽度始终在300px,而不会出现被撑开的现象

而在FF里面则不会造成浮动层填不满的情况

ul 标签在 Mozilla 中有 padding 值的，而在 IE 中只有 margin 有值。

设置ul{margin:0;padding:0}

IE和Mozilla浏览器对盒模型的解释不一致，造成定位上的困难，主要差别是：

1.IE计算2个div之间的距离时候，会把1px的border计算在内，而mozilla没有.

2.设定div的宽度后，如果给padding加一个值，IE5会在宽度内减去这个值，而Mozilla会在宽度基础上加上这个值。

针对firefox ie6 ie7的css样式

现在大部分都是用!important来hack，对于ie6和firefox测试可以正常显示，

但是ie7对!important可以正确解释，会导致页面没按要求显示!找到一个针

对IE7不错的hack方式就是使用“*+html”，现在用IE7浏览一下，应该没有问题了。
