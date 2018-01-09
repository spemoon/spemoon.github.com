---
title:  vim指令小收集
date: 2012-04-27
categories: 
 - vim
tags: 
 - vim
---

1.改变当前窗口高度(增加n行的高度)
``` bash
:res n
```
或者：
``` bash
n ctrl w +
```
2.改变当前窗口宽度(增加n列的高度)
``` bash
:vertical res n
```
或者：
``` bash
n ctrl w >
```
PS:这尼玛这么麻烦，还不如用鼠标拖一下来得快=。不过如果木有鼠标的话另当别论了。。。     

3.去掉^M 
``` bash
:%s/\r/
```

4.行首插入字符和行尾追加字符 
在当前文件的所有行首插入字符"a"，可以用如下命令：
``` bash
:%s/^/a
```
又如，在当前文件的所有行尾追加字符"b"，可用如下命令：
``` bash
:%s/$/b
```
其中的^和$就分别是正则表达式中表示行首和行尾的表达式。

5.字符串替换
``` bash
:s/vivian/sky/ 替换当前行第一个 vivian 为 sky
:s/vivian/sky/g 替换当前行所有 vivian 为 sky
:n,$s/vivian/sky/ 替换第 n 行开始到最后一行中每一行的第一个 vivian 为 sky
:n,$s/vivian/sky/g 替换第 n 行开始到最后一行中每一行所有 vivian 为 sky
n 为数字，若 n 为 .，表示从当前行开始到最后一行
:%s/vivian/sky/(等同于 :g/vivian/s//sky/) 替换每一行的第一个 vivian 为 sky
:%s/vivian/sky/g(等同于 :g/vivian/s//sky/g) 替换每一行中所有 vivian 为 sky
可以使用 # 作为分隔符，此时中间出现的 / 不会作为分隔符
:s#vivian/#sky/# 替换当前行第一个 vivian/ 为 sky/
:%s+/oradata/apras/+/user01/apras1+ (使用+ 来 替换 / )： /oradata/apras/替换成/user01/apras1/
* ************************************
1.:s/vivian/sky/ 替换当前行第一个 vivian 为 sky
:s/vivian/sky/g 替换当前行所有 vivian 为 sky
2. :n,$s/vivian/sky/ 替换第 n 行开始到最后一行中每一行的第一个 vivian 为 sky
:n,$s/vivian/sky/g 替换第 n 行开始到最后一行中每一行所有 vivian 为 sky
(n 为数字，若 n 为 .，表示从当前行开始到最后一行)
3. :%s/vivian/sky/(等同于 :g/vivian/s//sky/) 替换每一行的第一个 vivian 为 sky
:%s/vivian/sky/g(等同于 :g/vivian/s//sky/g) 替换每一行中所有 vivian 为 sky
```


