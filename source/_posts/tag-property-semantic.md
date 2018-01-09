---
title: 语义化标签和属性
date: 2011-09-01
category: html
categories:
 - html
tags:
 - html
 - property
 - semantic
comments: true
layout: post
---

分离结构与表现的另一个重要方面是使用语义化的标记来构造文档内容。一个 XHTML 元素的存在就意味被标记内容的那部分有相应的结构化的意义，没有理由使用其他的标记。换句话说，不要让 CSS 使一个 HTML 元素看起来就像另一个 HTML 元素，比如用&lt;div&gt;来代替&lt;p&gt;标记标题。

首先是关于语义（Semantics）和默认样式的区别，默认样式是浏览器设定的一些常用tag的表现形式，个人认为他的主要目的就是让大家直观的认识标签(markup)和属性(attribute)的用途和作用，很明显Hx系列看起来很像标题，因为拥有粗体和较大的字号。&lt;strong&gt;,&lt;em&gt;用来区别于其他文字，起到了强调的作用。至于列表和表格很明显的告诉你他们是做什么的。

其次，语义化的网页的好处，最主要的就是对搜索引擎友好，又了良好的结构和语义你的网页内容自然容易被搜索引擎抓取，你网站的推广便可以省下不少的功夫。

具体的语义和用途在，XHTML1.0 TAG 参考中都已经说明，这里将一些容易遗忘或者混淆的TAGS和属性予以补充。

###&lt;Hx&gt;###

&lt;h1&gt;、&lt;h2&gt;、&lt;h3&gt;、&lt;h4&gt;、&lt;h5&gt;、&lt;h6&gt;,作为标题使用，并且依据重要性递减。&lt;h1&gt;是最高的等级。

例如：

``` html
<h1>文档标题</h1>
<h2>次级标题</h2>
```
    

而不要使用&lt;div class="title"&gt;文档标题&lt;/div&gt;，或者&lt;span class="title"&gt;文档标题&lt;/span&gt;.很明显搜索引擎对于后者是不会认为他是标题的。

###&lt;p&gt;###

段落标记，知道了&lt;p&gt;作为段落，你就不会再使用&lt;br/&gt;来换行了，而且不需要&lt;br/&gt;&lt;br/&gt;来区分段落与段落。&lt;p&gt;&lt;/p&gt;中的文字会自动换行，而且换行的效果优于&lt;br /&gt;。段落与段落之间的空隙也可以利用CSS来控制，很容易而且清晰的区分出段落与段落。在利用行高(line-height)很容易的定义出行间距，再定义首字下沉等效果，那就挺完美了。

例如：

``` html
<p>这是一个段落哦~~</p>
```

###&lt;ul&gt;、&lt;ol&gt;、&lt;li&gt;###

&lt;ul&gt;无序列表，很常见的到了大家广泛的使用，&lt;ol&gt;有序列表也挺常用。在web标准化过程中，&lt;ul&gt;还被更多的用于导航条，本来导航条就是个列表，这样做是完全正确的，而且当你的浏览器不支持CSS的时候，导航链接仍然很好使，就是美观方面差了一点。

例如：

``` html
<ul>
<li>项目一</li>
<li>项目二</li>
<li>项目三</li>
</ul>
```

``` html
<ol>
<li>项目一</li>
<li>项目二</li>
<li>项目三</li>
</ol>
```

###&lt;dl&gt;、&lt;dt&gt;、&lt;dd&gt;###

dl就是“定义列表”。比如说词典里面的词的解释、定义就可以用这种列表。

``` html
<dl>
<dt>Dog</dt>
<dd>A carnivorous mammal of the family Canidae.</dd>
</dl>
```

###&lt;cite&gt;、&lt;q&gt;、&lt;blockquote&gt;###

论坛和blog经常会用到引用别人的话，用&lt;q&gt;来标记简短的单行引用。Web浏览器会自动识别在&lt;q&gt; 之间的内容。不幸的是，IE不能识别，并且有些时候， &lt;q&gt;会引起一些可访问性(Accessibility)的问题。正因为如此，一些人建议尽量不要使用 &lt;q&gt;,手动的插入引用标记。在一个包含适当的类的 &lt;span&gt;中加入单行的引用内容，那么就可以用CSS来给引用设计样式了，但是这个没有语义上的意义。

对于那些一段或者好几段的长篇引用，就应当使用 &lt;blockquote&gt;了。CSS可以用来定义引用的样式。注意，一段文章是不可以直接放在&lt;blockquote&gt;中的，引用的内容还必须包含在一个元素中，通常是&lt;p&gt;。属性cite既可以与&lt;q&gt; 一起用，也可以与&lt;blockquote&gt;一起用，用来提供引用内容的来源地址。需要注意的是，如果你使用&lt;span&gt;来代替 &lt;q&gt;标记引用内容,那么你就不能使用 cite属性了。例如:

``` html
<cite>Designing with Web Standards</cite> is an excellent book by Jeffrey Zeldman.
```

``` html
<p> <cite>孔子</cite>曰：<q>学而不思则罔，思而不学则殆</q>.</p>
```

``` html
<p>The W3C says that <q cite="http://www.w3.org/TR/REC-html40/struct/text.html#h-9.2.1">The presentation of phrase elements depends on the user agent.</q>.</p>
```

``` html
<blockquote cite="http://www.w3cn.org/">
<p>“我们大部分人都有深刻体验，每当主流浏览器版本的升级，我们刚建立的网站就可能变得过时，我们就需要升级或者重新建造一遍网站。例如1996-1999年典型的"浏览器大战"，为了兼容 Netscape 和 IE，网站不得不为这两种浏览器写不同的代码。同样的，每当新的网络技术和交互设备的出现，我们
也需要制作一个新版本来支持这种新技术或新设备，例如支持手机上网的 WAP 技术。类似的问题举不胜举：网站代码臃肿、繁杂浪费了我们大量的带宽；针对某种浏览器的 DHTML 特效，屏蔽了部分潜在的客户；不易用的代码，残障人士无法浏览网站等等。这是一种恶性循环，是一种巨大的浪费。</p>
</blockquote>
```

###&lt;em&gt;、&lt;strong&gt;###

&lt;em&gt; 是用作强调的，&lt;strong&gt;是用作重点强调的。 大部分浏览器用斜体显示强调的内容，用粗体来显示重点强调的内容，然而，这是没有必要的，如果是为了确定强调内容的显示方式，最好的方法就是使用CSS来定义他们的表现。当你想要的只是视觉上的效果时，就不要使用强调了。而且如果你想要强调但是还觉得粗体或者斜体不视觉效果没那么好，特别是斜体对于中文来说，那么你完全可以定义一些其他的比较醒目的样式达到强调的效果。

``` html
<p><em>强调</em> 的文本通常用斜体显示，然而，<strong>特别强调</strong> 的文本通常以粗体显示。</p>
```

###&lt;table&gt;、&lt;td&gt;、&lt;th&gt;、&lt;caption&gt;、summary###

XHTML中的表格不应用来布局。然而如果是为了标记列表的数据，就应该使用表格了。&lt;th&gt;为表格标题，属性summar为摘要，&lt;caption&gt;标签为首部说明，&lt;thead&gt;标签为表格头部，&lt;tbody&gt;标签为表格主体内容，&lt;tfoot&gt;标签为表格尾部。

其中还可以使用scope 可用于取代headers属性，标记含有表头信息的单元格，其中各数值的内容如下：

row 指示当前单元格，为包含当前单元格的行提供相关的表头信息。

col 指示当前单元格，为根据当前单元格指定的列提供相应的表头信息。

rowgroup 指示当前单元格，为包含当前单元格的其余行组提供相关的表头信息。

colgroup 指示当前单元格，为根据当前单元格指定的其余列组提供相应的表头信息。

abbr 用于定义表头单元格中的缩写名，如果没有定义该属性，则将默认单元格内容为节略形式。

###&lt;dfn&gt;###

``` html
<p><dfn title="Microsoft web browser">Internet Explorer</dfn> is the most popular browser used underwater.</p>
```

###&lt;ins&gt;, &lt;del&gt;###

知道del，就不要再用&lt;s&gt;做删除线了，用del显然更具有语义化。而且del还带有cite和datetime来表明删除的原因以及删除的时间。ins是表示插入，也有这样的属性。

``` html
<p>It really was <ins cite="rarara.html" datetime="20031024">very</ins> good.</p>
```

###&lt;code&gt;###

表示是计算机代码。而默认样式为打字体。技术论坛和blog中经常遇到。

``` html
<code>p{margin:2px 0;}</code>
```

###&lt;abbr&gt;、&lt;acronym&gt;###

&lt;abbr&gt;标签是表示web页面上的简称，&lt;acronym&gt;标签为取首字母缩写。（注：这里把简称和缩写分开而论，简称范围比缩写大，取首字母的缩写用&lt;acronym&gt;标签）Windows的IE6.0以下的浏览器暂不支持&lt;abbr&gt;标签。 在IE里，你可以应用CSS给&lt;acronym&gt;但是不能应用给&lt;abbr&gt;标签，IE会为&lt;acronym&gt;标签的title属性显示提示，但是会忽略&lt;abbr&gt;标签。

``` html
<abbr title="Cascading Style Sheets">CSS</abbr>
<acronym title="Cascading Style Sheets">CSS</acronym>
```

###alt属性和title属性###

title属性用来为元素提供额外说明信息title属性可以用在除了base，basefont，head，html，meta，param，script和title之外的所有标签。但是并不是必须的。

alt属性为不能显示图像、窗体或applets的用户代理（UA），指定替换文字。替换文字的语言由lang属性指定。
只需要一行语句把生产者、过滤器和消费者串联起来就可以创建出一个管道：

