---
title:  Node.js学习笔记——Streams 流 
date: 2011-12-14
categories: 
 - node.js
tags: 
 - javascript
 - node 
 - stream
comments: true
layout: post
---

在Node中，Stream（流）是一个由不同对象实现的抽象接口。例如请求HTTP服务器的request是一个流，类似于stdout（标准输出）。流可以是可读的，可写的，或者既可读又可写。所有流都是EventEmitter的实例。

## Readable Stream 可读流

一个可读流具有下述的方法、成员、及事件。

### Event: 'data' 事件：'data'

function (data) { }

'data'事件的回调函数参数默认情况下是一个Buffer对象。如果使用了setEncoding() 则参数为一个字符串。

### Event: 'end' 事件：'end'

function () { }

当流中接收到EOF（TCP中为FIN）时此事件被触发，表示流的读取已经结束，不会再发生任何'data'事件。如果流同时也是可写的，那它还可以继续写入。

### Event: 'error' 事件：'error'

function (exception) { }

接收数据的过程中发生任何错误时，此事件被触发。

### Event: 'close' 事件：'close'

function () { }

当底层的文件描述符被关闭时触发此事件，并不是所有流都会触发这个事件。（例如，一个连接进入的HTTP request流就不会触发'close'事件。）

### Event: 'fd' 事件：'fd'

function (fd) { }

当在流中接收到一个文件描述符时触发此事件。只有UNIX流支持这个功能，其他类型的流均不会触发此事件。

### stream.readable

这是一个布尔值，默认值为true。当'error'事件或'end'事件发生后，或者destroy()被调用后，这个属性将变为false。

### stream.setEncoding(encoding)

调用此方法会影响'data'事件的回调函数参数形式，默认为Buffer对象，调用此方法后为字符串。encoding参数可以是'utf8'、'ascii'、或'base64'。

### stream.pause()

暂停'data'事件的触发。

### stream.resume()

恢复被pause()调用暂停的'data'事件触发。

### stream.destroy()

关闭底层的文件描述符。流上将不会再触发任何事件。

### stream.destroySoon()

在写队列清空后（所有写操作完成后），关闭文件描述符。

### stream.pipe(destination, [options])

这是Stream.prototype（Stream原型对象）的一个方法，对所有Stream对象有效。

用于将这个可读流和destination目标可写流连接起来，传入这个流中的数据将会写入到destination流中。通过在必要时暂停和恢复流，来源流和目的流得以保持同步。

模拟Unix系统的cat命令：
``` javacript
process.stdin.resume();
process.stdin.pipe(process.stdout);
```

默认情况下，当来源流的end事件触发时目的流的end()方法会被调用，此时destination目的流将不再可写入。要在这种情况下为了保持目的流仍然可写入，可将options参数设为{ end: false }。

这使process.stdout保持打开状态，因此"Goodbye"可以在end事件发生后被写入。

``` javacript
process.stdin.resume();

process.stdin.pipe(process.stdout, { end: false });

process.stdin.on("end", function() {
    process.stdout.write("Goodbye\n");
});
```

注意：如果来源流不支持pause()和resume()方法，此函数将在来源流对象上增加这两个方法的简单定义，内容为触发'pause'和'resume'事件。




## Writable Stream 可写流

一个可写流具有下列方法、成员、和事件。

### Event: 'drain' 事件：'drain'

function () { }

发生在write()方法被调用并返回false之后。此事件被触发说明内核缓冲区已空，再次写入是安全的。

### Event: 'error' 事件：'error'

function (exception) { }

发生错误时被触发，回调函数接收一个异常参数exception。

### Event: 'close' 事件：'close'

function () { }

底层文件描述符被关闭时被触发。

### Event: 'pipe' 事件：'pipe'

function (src) { }

当此可写流作为参数传给一个可读流的pipe方法时被触发。

### stream.writable

一个布尔值，默认值为true。在'error'事件被触发之后，或end() / destroy()方法被调用后此属性被设为false。

### stream.write(string, encoding='utf8', [fd])

使用指定编码encoding将字符串string写入到流中。如果字符串被成功写入内核缓冲区，此方法返回true。如果内核缓冲区已满，此方法返回false，数据将在以后被送出。当内核缓冲区再次被清空后'drain'事件将被触发。encoding参数默认为'utf8'`。

如果指定了可选参数fd，它将被作为一个文件描述符通过流传送。此功能仅被Unix流所支持，对于其他流此操作将被忽略而没有任何提示。当使用此方法传送一个文件描述符时，如果在流没有清空前关闭此文件描述符，将造成传送一个无效（已关闭）FD的风险。

### stream.write(buffer)

除了用一个Buffer对象替代字符串之外，其他同上。

### stream.end()

使用EOF或FIN结束一个流的输出。

### stream.end(string, encoding)

以指定的字符编码encoding传送一个字符串string，然后使用EOF或FIN结束流的输出。这对降低数据包传输量有所帮助。

### stream.end(buffer)

除了用一个buffer对象替代字符串之外，其他同上。

### stream.destroy()

关闭底层文件描述符。在此流上将不会再触发任何事件。
