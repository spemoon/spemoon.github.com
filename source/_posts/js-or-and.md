---
title: JS中的与（AND）、或（OR）工作原理
date: 2011-10-28
categories: 
 - javascript
tags: 
 - javascript
 - and
 - or
---

### 先来看mozilla的javascript文档对逻辑与的定义：

``` javascript
（逻辑与）若expr1可转换为false，则返回expr1；否则，返回expr2。因此，当使用布尔值时，若两个操作数都是true，则&&运算符返回true；否则，返回false。
```

因此换言之，这意味着，若expr1为真，则expr1 && expr2将返回expr2，否则将返回expr1。

注意：当逻辑与（&&）运算符与非布尔值（non Boolean Values）一起使用时，其运算结果不会返回“true”或“false”，而是返回两个表达式中的某一表达式本身。

那我们该怎么用呢？来接着看：

``` javascript
if(a){
    return a.method();
}else{
    return a;
}
```

根据&&的特性，我们可以将其改写为：

``` javascript
return a && a.method();
```

同样的，再来看逻辑或的定义：

``` javascript
（逻辑或）若expr1可转换为true，则返回expr1；否则，返回expr2。因此，当使用布尔值时，若任一操作数是true，则||运算符返回true；若两个操作数都是false，返回false。
```

所以我们可以利用其来以表达式风格来定义变量的缺省值

``` javascript
var myVar = input || default;
```

当然代码如果都运用这些特性的话，可读性就相对会低一点了，对于新人来说，阅读起来会有困难，需要在开发过程中进行相应的取舍。
