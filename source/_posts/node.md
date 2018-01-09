---
title:  折腾Node.js
date: 2011-12-05
categories: 
 - javascript
tags: 
 - javascript
 - nodejs 
---

前段时间在折腾Node.js。刚刚入门，啥都不懂，写篇东西记录下自己碰到的问题。

谷歌的blog被墙了，故得搬过来=。=

说起Node.js，不得不提到git。

git的使用可以去看它的官网的[帮助手册](http://help.github.com/linux-set-up-git/)

接着就是Node.js了，大头的来了

第一步：安装依赖包

``` javascript
1.安装 python2.6或者更高
2.sudo apt-get install g++ curl libssl-dev apache2-utils
3.sudo apt-get install git-core
```

第二步：获取源码

``` javascript
git clone git://github.com/joyent/node.git
```

第三步：指定编译版本，这个比较关键

``` javascript
1.cd node
2.git checkout v0.4.10  （这里比较重要，目前很多常用的包只支持到0.4.10，如express，所以如果用最新的版本的话，会导致npm无法下载相应的包）
3.指定路径，编译执行
mkdir ~/local
./configure -prefix=$HOME/local/node
./configure
make
make install
echo 'export PATH=$HOME/local/node/bin:$PATH'>> ~/.profile
echo 'export NODE_PATH=$HOME/local/node:$HOME/local/node/lib/node_modules'>> ~/.profile
source ~/.profile
```

第四步：设置环境变量


如果想重启后还能继续直接使用node命令,那么需要设置环境变量:
使用命令 sudo gedit /etc/profile 打开配置文件，在文件最后中添加如下两行：
``` javascript
export PATH=”$HOME/local/node/bin:$PATH”
export NODE_PATH=”$HOME/local/node:$HOME/local/node/lib/node_modules”
```
保存后重启系统使设置生效。


第五步：安装npm (这里注意可能需要翻墙来安装)

``` javascript
    curl http://npmjs.org/install.sh | sh
    根据需要，安装相应的包，例如express：
    npm install express
    如果输入该命令后长时间没有反应，可以通过添加 -verbose参数查看执行的详细信息，即：
    npm install express -verbose
    一般情况下无法下载有两个原因：
    1. 网速太慢，超时退出。
    2. node的版本太新，当前下载的包不支持。（解决方法在第三步已说明。）
```

如果被墙了，可以用SSH翻墙后，用proxychains来执行，当然还得先装个proxychains(详见附录)。

    安装完proxychains后可用命令proxychains npm install  XXX，来安装node相应的包。

    搞定后你可以先跑个helloworld例子看一下，这个例子网上到处都有，就不贴了=。=
    附录: proxychains安装方法如下：

    ubuntu11.10下直接sudo apt-get install proxychains就可以安装了，其他版本的linux系统可以看看自己系统的软件包支持有木有，如果软件包更新中没有就点击这里去[proxychains官方下载最新的版本](http://sourceforge.net/projects/proxychains/files/proxychains/version%203.1/proxychains-3.1.tar.gz/download)。然后编译，具体编译方法可以看包中的INSTALL文件说明。

    安装完成后我们需要对程序进行配置，配置文件是/etc/proxychains.conf，但是根据作者的说明，其实配置文件在三个地方都是有效的。

    好了，既然配置文件已经找到了，我们就来看看配置文件的具体配置吧。proxychains的模式有三种

    dynamic_chain，按照列表中出现的代理服务器的先后顺序组成一条链，如果有代理服务器失效，则自动将其排除，但至少要有一个是有效的。

    strict_chain，按照后面列表中出现的代理服务器的先后顺序组成一条链，要求所有的代理服务器都是有效的

    random_chain，列表中的任何一个代理服务器都可能被选择使用，这种方式很适合网络扫描操作（参数chain_len只对random_chain有效）。

    默认是选择的strict_chain，因此这里我们不做改变。在最下方可以配置自己的代理，方式可以参照配置文件。例如
    http 127.0.0.1 8080

socks5 127.0.0.1 7070 (ssh用这个配置就可以了)

    http 123.456.789.1 username passwd

    呃，但是如果选择strict_chain的方式，建议就留一个可用的代理即可，要不会无法使用。ok，把配置文件放到你的用户目录下就可以了。配置文件在哪里？下载这个吧，可以直接用做ssh的配置，其它代理自己修改即可。[点此下载配置文件](http://yunfile.com/file/nenew/76c8efa9/)

    执行程序的时候直接输入proxychains 程序名即可，比如打开火狐可以用 proxychains firefox。还有，启动个别程序的时候可能需要sudo权限。

