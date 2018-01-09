---
title:  Node.js学习笔记——Buffers 缓冲器 
date: 2011-12-13
categories: 
 - node.js
tags: 
 - javascript
 - node 
 - buffer
---

纯Javascript语言是Unicode友好性的，但是难以处理二进制数据。在处理TCP流和文件系统时经常需要操作字节流。Node提供了一些列机制，用于操作、创建、以及消耗（consuming）字节流。

在实例化的Buffer类中存储了原始数据。Buffer类似于一个整数数组，但Buffer对应了在V8堆（the V8 heap）外的原始存储空间分配。一旦创建了Buffer实例，则无法改变其大小。

另外，Buffer是一个全局对象。

在缓冲器（Buffers）和JavaScript间进行字符串的转换需要调用特定的编码方法。如下列举了不同的编码方法：

* 'ascii' - 仅对应7位的ASCII数据。虽然这种编码方式非常迅速，并且如果设置了最高位，则会将其移去。

* 'utf8' - 对应多字节编码Unicode字符。大量网页和其他文件格式使用这类编码方式。

* 'ucs2' - 2字节的，低字节序编码Unicode字符。只能编码BMP（第零平面，U+0000 - U+FFFF）字符。

* 'base64' - Base64 字符串编码.

* 'binary' - 仅使用每个字符的头8位将原始的二进制信息进行编码。在需使用Buffer的情况下，应该尽量避免使用这个已经过时的编码方式。而且，这个编码方式不会出现在未来版本的Node中。

* 'hex' - 将一个字节编码为两个16进制字符。

### new Buffer(size)

使用array的空间创建一个buffer实例。

### new Buffer(str, encoding='utf8')

创建一个包含给定str的buffer实例。

### buffer.write(string, offset=0, encoding='utf8')

通过给定的编码方式把string写入到buffer的offset（偏移地址）中，并且返回写入的字节数。如果当前的buffer没有足够存储空间，字符串会部分地保存在buffer中，而不是整串字符。需要注意的是，如果使用'utf8'进行编码，该方法不会对零散的字符进行编写。

例如：将一串utf8格式的字符串写入Buffer，然后输出：
``` javascript
buf = new Buffer(256);
len = buf.write('\u00bd + \u00bc = \u00be', 0);
console.log(len + " bytes: " + buf.toString('utf8', 0, len));
// 12 bytes: ½ + ¼ = ¾
```

### buffer.toString(encoding, start=0, end=buffer.length)

对缓冲器中的以encoding方式编码的，以start标识符开始，以end标识符结尾的缓冲数据进行解码，并输出字符串。

### buffer[index]

获取或者设置位于index字节的值。由于返回值为单个的字节，因此其范围应该在0x00 到 0xFF（16进制）或者0 and 255（10进制）之间

例如：通过每次仅输入一个字符的方式将整串ASCII字符录入Buffer中：
``` javascript
str = "node.js";
buf = new Buffer(str.length);

for (var i = 0; i < str.length ; i++) {
    buf[i] = str.charCodeAt(i);
}

console.log(buf);

// node.js
```

### Buffer.isBuffer(obj)

验证obj的类别是否为Buffer类。

### Buffer.byteLength(string, encoding='utf8')

返回字符串长度的实际值。与String.prototype.length的区别之处在于该方法返回的是字符串中characters的个数。

例如：
``` javascript
str = '\u00bd + \u00bc = \u00be';

console.log(str + ": " + str.length + " characters, " +
        Buffer.byteLength(str, 'utf8') + " bytes");

// ½ + ¼ = ¾: 9 characters, 12 bytes
```

### buffer.length

返回Buffer占用的字节数。需要注意的是，length并非其内容占的大小，而是指分配给Buffer实例的存储空间的大小，因此该值不会随Buffer内容的变化而变化。

``` javascript
buf = new Buffer(1234);

console.log(buf.length);
buf.write("some string", "ascii", 0);
console.log(buf.length);

// 1234
// 1234
```

### buffer.copy(targetBuffer, targetStart=0, sourceStart=0, sourceEnd=buffer.length)

在两个Buffer之间进行memcpy() 操作。

例如：创建2个Buffer实例，然后将buf1中第16字节到第19字节间的信息复制到buf2中，并使在buf2中新的字符串首字符位于第8字节：

``` javascript
buf1 = new Buffer(26);
buf2 = new Buffer(26);

for (var i = 0 ; i < 26 ; i++) {
    buf1[i] = i + 97; // 97 is ASCII a
    buf2[i] = 33; // ASCII !
}

buf1.copy(buf2, 8, 16, 20);
console.log(buf2.toString('ascii', 0, 25));

// !!!!!!!!qrst!!!!!!!!!!!!!
```

### buffer.slice(start, end=buffer.length)

返回一个和原Buffer引用相同存储空间的新Buffer，但是新Buffer中的偏移地址截取了原Buffer偏移地址中自start到end的部分。

特别注意：通过修改新的Buffer切片（slice）中的内容同样会修改存储在原Buffer中的信息！

``` javascript
var buf1 = new Buffer(26);

for (var i = 0 ; i < 26 ; i++) {
    buf1[i] = i + 97; // 97 is ASCII a
}

var buf2 = buf1.slice(0, 3);
console.log(buf2.toString('ascii', 0, buf2.length));
buf1[0] = 33;
console.log(buf2.toString('ascii', 0, buf2.length));

// abc
// !bc
```

