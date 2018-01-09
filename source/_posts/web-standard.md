---
title: web标准
date: 2011-08-24
tags:
 - web
 - standard
category: web standard 
categories:
 - web
---

Web标准不是某一个标准，而是一系列标准的集合。网页主要由三部分组成：结构(Structure)、表现(Presentation)和行为(Behavior)。对应的标准也分三方面：结构化标准语言主要包括XHTML和XML，表现标准语言主要包括CSS，行为标准主要包括对象模型(如 W3C DOM)、ECMAScript等。这些标准大部分由W3C起草和发布，也有一些是其他标准组织制订的标准，比如ECMA的ECMAScript标准。

###XML###

XML是 The Extensible Markup Language(可扩展标识语言)的简写。目前推荐遵循的是W3C于2000年10月6日发布的[XML1.0](www.w3.org/TR/2000/REC-XML-20001006)。和HTML一样，XML同样来源于SGML，但XML是一种能定义其他语言的语。XML最初设计的目的是弥补HTML的不足，以强大的扩展性满足网络信息发布的需要，后来逐渐用于网络数据的转换和描述。关于XML的好处和技术规范细节这里就不多说了，网上有很多资料，也有很多书籍可以参考。

###XHTML###

XHTML是The Extensible HyperText Markup Language可扩展标识语言的缩写。目前推荐遵循的是W3C于2000年1月26日推荐[XML1.0](参考http://www.w3.org/TR/xhtml1)。XML虽然数据转换能力强大，完全可以替代HTML，但面对成千上万已有的站点，直接采用XML还为时过早。因此，我们在HTML4.0的基础上，用XML的规则对其进行扩展，得到了XHTML。简单的说，建立XHTML的目的就是实现HTML向XML的过渡。

###表现标准语言###

CSS是Cascading Style Sheets层叠样式表的缩写。目前推荐遵循的是W3C于1998年5月12日推荐[CSS2](http://www.w3.org/TR/CSS2/)。W3C创建CSS标准的目的是以CSS取代HTML表格式布局、帧和其他表现的语言。纯CSS布局与结构式XHTML相结合能帮助设计师分离外观与结构，使站点的访问及维护更加容易。

###DOM###

DOM是Document Object Model文档对象模型的缩写。根据W3C [DOM规范](http://www.w3.org/DOM/)，DOM是一种与浏览器，平台，语言的接口，使得你可以访问页面其他的标准组件。简单理解，DOM解决了Netscaped的Javascript和Microsoft的Jscript之间的冲突，给予web设计师和开发者一个标准的方法，让他们来访问他们站点中的数据、脚本和表现层对像。

###ECMAScript###

ECMAScript是ECMA(European Computer Manufacturers Association)制定的标准脚本语言（JAVAScript）。目前推荐遵循的是[ECMAScript 262](http://www.ecma.ch/ecma1/STAND/ECMA-262.HTM)。
