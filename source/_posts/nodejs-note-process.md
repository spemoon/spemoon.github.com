---
title:  Node.js学习笔记——process 进程 
date: 2011-12-10
categories: 
 - node.js
tags: 
 - javascript
 - node 
 - process
---

process对象是一个全局对象，可以在任何地方访问它。

它是EventEmitter事件触发器类型的一个实例。

### Event: 'exit' 事件：'exit'

当进程对象要退出时会触发此方法，这是检查模块状态（比如单元测试）的好时机。当'exit'被调用完成后主事件循环将终止，所以计时器将不会按计划执行。

监听exit行为的示例：
``` javascript
process.on('exit', function () {
    process.nextTick(function () {
        console.log('This will not run');
        });
    console.log('About to exit.');
});
```

### Event: 'uncaughtException' 事件：'uncaughtException'

function (err) { }

当一个异常信息一路冒出到事件循环时，该方法被触发。如果该异常有一个监听器，那么默认的行为（即打印一个堆栈轨迹并退出）将不会发生。

监听uncaughtException事件的示例：
``` javascript
process.on('uncaughtException', function (err) {
        console.log('Caught exception: ' + err);
        });

setTimeout(function () {
        console.log('This will still run.');
        }, 500);

// Intentionally cause an exception, but don't catch it.
nonexistentFunc();
console.log('This will not run.');
```

注意：就异常处理来说，uncaughtException是一个很粗糙的机制。在程序中使用try/catch可以更好好控制程序流程。而在服务器编程中，因为要持续运行，uncaughtException还是一个很有用的安全机制。

### Signal Events 信号事件

该事件会在进程接收到一个信号时被触发。可参见sigaction(2)中的标准POSIX信号名称列表，比如SIGINT，SIGUSR1等等。

监听 SIGINT的示例：

``` javascript
// Start reading from stdin so we don't exit.
process.stdin.resume();

process.on('SIGINT', function () {
    console.log('Got SIGINT.  Press Control-D to exit.');
});
```

在大多数终端程序中，一个简易发送SIGINT信号的方法是在使用Control-C命令操作。

### process.stdout

一个指向标准输出stdout的Writable Stream可写流。

示例：console.log的定义。

``` javascript
console.log = function (d) {
    process.stdout.write(d + '\n');
};
```

### process.stderr

一个指向错误的可写流，在这个流上的写操作是阻塞式的。

### process.stdin

一个到标准输入的可读流Readable Stream。默认情况下标准输入流是暂停的，要从中读取内容需要调用方法process.stdin.resume()。

示例：打开标准输入与监听两个事件：
``` javascript
process.stdin.resume();
process.stdin.setEncoding('utf8');

process.stdin.on('data', function (chunk) {
    process.stdout.write('data: ' + chunk);
});

process.stdin.on('end', function () {
    process.stdout.write('end');
});
```

### process.argv

一个包含命令行参数的数组。第一个元素是'node'，第二个元素是JavaScript文件的文件名。接下来的元素则是附加的命令行参数。
``` javascript
// print process.argv
process.argv.forEach(function (val, index, array) {
    console.log(index + ': ' + val);
});
```

这产生如下的信息：
``` javascript
$ node process-2.js one two=three four
0: node
1: /Users/mjr/work/node/process-2.js
2: one
3: two=three
4: four
```

### process.execPath

这是一个启动该进程的可执行程序的绝对路径名。

例如：
``` javascript
/usr/local/bin/node
```

### process.chdir(directory)

改变进程的当前工作目录，如果操作失败则抛出异常。
``` javascript
console.log('Starting directory: ' + process.cwd());
try {
    process.chdir('/tmp');
    console.log('New directory: ' + process.cwd());
}
catch (err) {
    console.log('chdir: ' + err);
}
```

### process.cwd()

返回进程的当前工作目录。

``` javascript
console.log('Current directory: ' + process.cwd());
```

### process.env

一个包括用户环境的对象。可参见environ(7)。

### process.exit(code=0)

用指定的code代码结束进程。如果不指定，退出时将使用'success'（成功）代码 0。

以'failure'（失败）代码退出的示例：
``` javascript
process.exit(1);
```

执行node的shell会把退出代码视为1。

### process.getgid()

获取进程的群组标识（详见getgid(2)）。这是一个数字的群组ID，不是群组名称。
``` javascript
console.log('Current gid: ' + process.getgid());
```

### process.setgid(id)

设置进程的群组标识（详见getgid(2)）。参数可以是一个数字ID或者群组名字符串。如果指定了一个群组名，这个方法会阻塞等待将群组名解析为数字ID。

``` javascript
console.log('Current gid: ' + process.getgid());
try {
    process.setgid(501);
    console.log('New gid: ' + process.getgid());
}
catch (err) {
    console.log('Failed to set gid: ' + err);
}
```

### process.getuid()

获取进程的用户ID（详见getgid(2)）。这是一个数字用户ID，不是用户名。

``` javascript
console.log('Current uid: ' + process.getuid());
```

### process.setuid(id)

设置进程的用户ID（详见getgid(2)）。参数可以使一个数字ID或者用户名字符串。如果指定了一个用户名，那么该方法会阻塞等待将用户名解析为数字ID。
``` javascript
console.log('Current uid: ' + process.getuid());
try {
    process.setuid(501);
    console.log('New uid: ' + process.getuid());
}
catch (err) {
    console.log('Failed to set uid: ' + err);
}
```

### process.version

一个编译内置的属性，用于显示NODE_VERSION（Node版本）。

``` javascript
console.log('Version: ' + process.version);
```

### process.installPrefix

一个编译内置的属性，用于显示NODE_PREFIX（Node安装路径前缀）。

``` javascript
console.log('Prefix: ' + process.installPrefix);
```

### process.kill(pid, signal='SIGTERM')

发送一个信号到进程。pid是进程的ID，参数signal是欲发送信号的字符串描述。信号名称是像'SIGINT'或者'SIGUSR1'这样的字符串。如果参数signal忽略，则信号为'SIGTERM'。详见kill(2)。

注意该函数名为process.kill，实际上也就像kill系统调用一样仅仅是一个信号发送器。发送的信号可能是要终止目标进程，也可能是实现其他不同的目的。

一个给自己发送信号的示例：
``` javascript
process.on('SIGHUP', function () {
    console.log('Got SIGHUP signal.');
});

setTimeout(function () {
    console.log('Exiting.');
    process.exit(0);
}, 100);

process.kill(process.pid, 'SIGHUP');
```

### process.pid

进程的PID。

``` javascript
console.log('This process is pid ' + process.pid);
```

### process.title

获取或设置在'ps'命令中显示的进程的标题。

### process.platform

运行Node的平台信息，如'linux2'，'darwin'`等等。
``` javascript
console.log('This platform is ' + process.platform);
```

### process.memoryUsage()

返回一个描述Node进程内存使用情况的对象。

``` javascript
var util = require('util');
console.log(util.inspect(process.memoryUsage()));
```

这会生成如下信息：
``` javascript
{ rss: 4935680,
  vsize: 41893888,
  heapTotal: 1826816,
  heapUsed: 650472 }
```

heapTotal与heapUsed指V8的内存使用情况。

### process.nextTick(callback)

在事件循环的下一次循环中调用callback回调函数。这不是setTimeout(fn, 0)的一个别名，因为它有效率多了。

``` javascript
process.nextTick(function () {
    console.log('nextTick callback');
});
```

### process.umask([mask])

设置或者读取进程的文件模式创建掩码。子进程从父进程中继承这个掩码。如果设定了参数mask那么返回旧的掩码，否则返回当前的掩码。
``` javascript
var oldmask, newmask = 0644;

oldmask = process.umask(newmask);
console.log('Changed umask from: ' + oldmask.toString(8) + ' to ' + newmask.toString(8));
```

