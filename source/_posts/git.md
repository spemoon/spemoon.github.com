---
title: git使用说明
date: 2011-07-20
tags:
 - git
categories: 
 - git
---

记录git常用命令

*git clone:*

git仓库可以使用git clone获得：
``` sh
git clone git://url
```

也可以通过浏览器浏览。
``` sh
http://url/gitweb/
```

通过`git pull`更新仓库，使用`git init-db`初始化自己的仓库。


*config:*

开发人员需要为git仓库配置相关信息，这样在提交代码时，这些信息会自动
反映在git仓库的日志中。

``` bash
git config user.name "your name"
git config user.email yourname@email_server
git config core.editor vim
git config core.paper "less -N"
git config color.diff true
git config alias.co checkout
```

`git config alias`表示，可以用`git co`代表`git checkout`。`git var -l`可以查看
已经设置的配置。


*diff:*

开发人员在本地进行开发后，可以使用git diff查看改动。
除了直接比较当前开发后的改动外，git diff还可以：

``` bash
git diff tag                    比较tag和HEAD之间的不同。
git diff tag file               比较一个文件在两者之间的不同。
git diff tag1..tag2             比较两个tag之间的不同。
git diff SHA11..SHA12           比较两个提交之间的不同。
git diff tag1 tag2 file or
git diff tag1:file tag2:file    比较一个文件在两个tag之间的不同。
```

`ORIG_HEAD`用于指向前一个操作状态，因此在git pull之后如果想得到pull的
内容就可以：

``` bash
git diff ORIG_HEAD
git diff --stat                 用于生成统计信息。
git diff --stat ORIG_HEAD
```


*apply:*

`git apply`相当于patch命令。
`--check` 检查能否正常打上补丁，-v verbose模式， -R reverse模式，反打补丁。


*log:*

``` bash
git log file                    查看一个文件的改动。
git log -p                      查看日志和改动。
git log tag1..tag2              查看两个tag之间的日志。
git log -p tag1..tag2 file      查看一个文件在两个tag之间的不同。
git log tag..                   查看tag和HEAD之间的不同。
```


*commit:*

``` bash
git commit -a -e        提交全部修改文件，并调用vim编辑提交日志。
git reset HEAD^ or
git reset HEAD~1        撤销最后一次提交。
git reset --hard HEAD^  撤销最后一次提交并清除本地修改。
git reset SHA1          回到SHA1对应的提交状态。
```


*add/delete/ls:*

``` bash
git add -a              添加所有文件。除了.gitignore文件中的文件。
git rm file             从git仓库中删除文件。
git commit              添加或是删除后要提交。
git ls-files -m         显示修改过的文件。
git ls-files            显示所有仓库中的文件。
```

git中有四种对象：blob、tree、commit、tag。
blob代表文件，tree代表目录，commit代表提交历史，tag代表标签。
这四种对象都是由SHA1值表示的。在仓库的.git目录中保存了git管理仓库
所需要的全部信息。

``` bash
git ls-tree HEAD file   显示file在HEAD中的SHA1值。
git cat-file -t SHA1    显示一个SHA1的类型。
git cat-file type SHA1  显示一个SHA1的内容。type是blob、tree、commit、tag之一。
```

*patch:*

``` bash
git format-patch -1     生成最后一个提交对应的patch文件。
git am < patch          把一个patch文件加入git仓库中。
git am --resolved       如果有冲突，在解决冲突后执行。
git am --skip           放弃当前git am所引入的patch。
```

*conflict:*

``` bash
git merge               用于合并两个分支。
git diff                如果有冲突，直接使用diff查看，
冲突代码用<<<和>>>表示。手动修改冲突代码。
git update-index        更新修改后的文件状态。
git commit -a -e        提交为解决冲突而修改的代码。
```

*branch:*

``` bash
git branch -a           查看所有分支。
git branch new_branch   创建新的分支。
git branch -d branch    删除分支。
git checkout branch     切换当前分支。-f参数可以覆盖未提交内容。
```

*daemon:*

有时更新公共代码仓库使用patch的方式，或者直接
用`git pull git://ip/repo branch`
的方式更新每个人的代码。使用git pull的方式需要
提交代码的机器运行：
``` bash
git daemon --verbose --export-all --enable=receive-pack --base-path=/repo
```


*request-pull:*


``` bash
git request-pull start url      用于产生本次pull请求的统计信息。
```

*clean:*

``` bash
git clean -dxf          用于清除未跟踪文件。
git clean -dnf          可以显示需要删除的文件，但不包括被.gitignore忽略的。
git reset --hard HEAD   用于清除跟踪文件的修改。
```
