---
title:  Node.js学习笔记——Events 事件模块 
date: 2011-12-12
categories: 
 - node.js
tags: 
 - javascript
 - node 
 - events
---

Node引擎中很多对象都会触发事件：例如net.Server会在每一次有客户端连接到它时触发事件，又如fs.readStream会在文件打开时触发事件。所有能够触发事件的对象都是events.EventEmitter的实例。你可以通过require("events");访问这个模块。

通常情况下，事件名称采用驼峰式写法，不过目前并没有对事件名称作任何的限制，也就是说任何的字符串都可以被接受。

可以将函数注册给对象，使其在事件触发时执行，此类函数被称作监听器。

### events.EventEmitter

通过调用require('events').EventEmitter，我们可以使用事件触发器类。

当EventEmitter事件触发器遇到错误时，典型的处理方式是它将触发一个'error'事件。Error事件的特殊性在于：如果没有函数处理这个事件，它将会输出调用堆栈，并随之退出应用程序。

当新的事件监听器被添加时，所有的事件触发器都将触发名为'newListener'的事件。

### emitter.addListener(event, listener)
### emitter.on(event, listener)

将一个监听器添加到指定事件的监听器数组的末尾。
``` javascript
server.on('connection', function (stream) {
    console.log('someone connected!');
});
```

### emitter.once(event, listener)

为事件添加一次性的监听器。该监听器在事件第一次触发时执行，过后将被移除。

``` javascript
server.once('connection', function (stream) {
    console.log('Ah, we have our first user!');
});
```

### emitter.removeListener(event, listener)

将监听器从指定事件的监听器数组中移除出去。 小心：此操作将改变监听器数组的下标。

``` javascript
var callback = function(stream) {
    console.log('someone connected!');
};
server.on('connection', callback);
// ...
server.removeListener('connection', callback);
```

### emitter.removeAllListeners(event)

将指定事件的所有监听器从监听器数组中移除。

### emitter.setMaxListeners(n)

默认情况下当事件触发器注册了超过10个以上的监听器时系统会打印警告信息，这个默认配置将有助于你查找内存泄露问题。很显然并不是所有的事件触发器都需要进行10个监听器的限制，此函数允许你手动设置该数量值，如果值为0意味值没有限制。

### emitter.listeners(event)

返回指定事件的监听器数组对象，你可以对该数组进行操作，比如说删除监听器等。

``` javascript
server.on('connection', function (stream) {
    console.log('someone connected!');
});
console.log(util.inspect(server.listeners('connection')); // [ [Function] ]
```

### emitter.emit(event, [arg1], [arg2], [...])

以提供的参数作为监听器函数的参数，顺序执行监听器列表中的每个监听器函数。

### Event: 'newListener'

function (event, listener) { }

任何时候只要新的监听器被添加时该事件就会触发。


