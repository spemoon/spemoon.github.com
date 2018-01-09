---
title:  placeholder
date: 2013-03-04
categories: 
 - fe
tags: 
 - web
 - placeholder
---

基本上现在的web站点，如果涉及到表单之类的东东，一般都会用到[placeholder](http://www.w3.org/WAI/GL/wiki/Using_@placeholder_on_input)。

但是IE之流的浏览器要用placeholder，就需要去模拟。

目前能想到的一般有两种思路，但都有些尴尬，或者说是弊端。

#### 第一种是通过input的value变化来做。

这种优点是无需增加额外的结构，对于样式就无需做额外的控制。

但是它的缺点也很明显，就是提交相关的表单的时候，就得对表单上的所有input做一遍过滤，而且对于 type = password 的 input, 会被用星号代替我们想要的字符。

#### 第二种就是通过增加一个label标签来实现，具体做法是外层有个块结构(如div)，里面是input跟label并列的结构，然后通过位置将label跟input显示在相关的位置(relative跟absolute就不多说了吧=。=)，然后监听输入域的内容变化来判断显示隐藏label。

第二种方式显然避免掉了第一种的两个缺点，这算优点吧。

第二种方法的缺点是增加了额外的结构，需要更加自身的需求进行额外的样式修改。

PS：第二种方法input背景设置透明的时候最好要用一张一像素的透明gif图片来repeat,不然input区域会被label遮挡掉，无法正常点击。

不知还有何种思路？？？