---
title:  Node.js学习笔记——Timers 定时器 
date: 2011-12-08
categories: 
 - javascript
tags: 
 - javascript
 - nodejs 
 - timers
---

本节内容与原生js类似。

### setTimeout(callback, delay, [arg], [...])

设定一个delay毫秒后执行callback回调函数的计划。返回值timeoutId可被用于clearTimeout()。可以设定要传递给回调函数的参数。

### clearTimeout(timeoutId)

清除定时器，阻止指定的timeout（超时）定时器被触发。

### setInterval(callback, delay, [arg], [...])

设定一个每delay毫秒重复执行callback回调函数的计划。返回值intervalId可被用于clearInterval()。可以设定要传递给回调函数的参数。

### clearInterval(intervalId)

清除定时器，阻止指定的interval（间隔）定时器被触发。
