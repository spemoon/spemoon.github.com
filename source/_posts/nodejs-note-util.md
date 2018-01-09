---
title:  Node.js学习笔记——util 工具模块 
date: 2011-12-11
categories: 
 - node.js
tags: 
 - javascript
 - node 
 - util
---

下列函数属于'util'（工具）模块，可使用require('util')访问它们。

### util.debug(string)

这是一个同步输出函数，将string参数的内容实时输出到stderr标准错误。调用此函数时将阻塞当前进程直到输出完成。

``` javascript
require('util').debug('message on stderr');
```

### util.log(string)

将string参数的内容加上当前时间戳，输出到stdout标准输出。

``` javascript
require('util').log('Timestmaped message.');
```

### util.inspect(object, showHidden=false, depth=2)

以字符串形式返回object对象的结构信息，这对程序调试非常有帮助。

如果showHidden参数设置为true，则此对象的不可枚举属性也会被显示。

可使用depth参数指定inspect函数在格式化对象信息时的递归次数。这对分析复杂对象的内部结构非常有帮助。

默认情况下递归两次，如果想要无限递归可将depth参数设为null。

显示util对象所有属性的例子如下：
``` javascript
var util = require('util');
console.log(util.inspect(util, true, null));
```

### util.pump(readableStream, writableStream, [callback])

实验性的

从readableStream参数所指定的可读流中读取数据，并将其写入到writableStream参数所指定的可写流中。当writeableStream.write(data)函数调用返回为false时，readableStream流将被暂停，直到在writableStream流上发生drain事件。当writableStream流被关闭或发生一个错误时，callback回调函数被调用。此回调函数只接受一个参数用以指明所发生的错误。

### util.inherits(constructor, superConstructor)

将一个构造函数的原型方法继承到另一个构造函数中。constructor构造函数的原型将被设置为使用superConstructor构造函数所创建的一个新对象。

此方法带来的额外的好处是，可以通过constructor.super_属性来访问superConstructor构造函数。
``` javascript
var util = require("util");
var events = require("events");

function MyStream() {
    events.EventEmitter.call(this);
}

util.inherits(MyStream, events.EventEmitter);

MyStream.prototype.write = function(data) {
    this.emit("data", data);
}

var stream = new MyStream();

console.log(stream instanceof events.EventEmitter); // true
console.log(MyStream.super_ === events.EventEmitter); // true

stream.on("data", function(data) {
    console.log('Received data: "' + data + '"');
})
stream.write("It works!"); // Received data: "It works!"
```


