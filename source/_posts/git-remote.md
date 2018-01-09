---
title:  git remote 基本使用
date: 2012-09-04
categories: 
 - git
tags: 
 - git
 - remote
comments: true
layout: post
---

###添加远程仓库路径###
``` javascript
git remote add github git@github.com:spemoon/spemoon.github.com.git
```
# 实际上，pull 就是 fetch + merge
``` javascript
git pull github --all --tags
```
把工作目录迁移到github上面：

``` javascript
git remote add github git@github.com:spemoon/spemoon.github.com.git
git push github --all --tags
```
显示所有的远程仓库

``` javascript
git remote -v
origin  git@github.com:spemoon/spemoon.github.com.git (fetch)
origin  git@github.com:spemoon/spemoon.github.com.git (push)
github  git@github.com:xxxx.git (fetch)
github  git@github.com:xxxx.git (push)
```
重命名远程仓库

``` javascript
git remote rename github gh
git remote
origin
gh
```
删除远程仓库

``` javascript
git remote rm github
git remote
origin
```
从远程仓库抓取数据，更新本地仓库：

``` javascript
git fetch origin
remote: Counting objects: 58, done.
remote: Compressing objects: 100% (41/41), done.
remote: Total 44 (delta 24), reused 1 (delta 0)
Unpacking objects: 100% (44/44), done.
From git@github.com:spemoon/spemoon.github.com.git
* [new branch]      product     -> origin/product
```
查看远程仓库信息，可用于跟踪别人的push：

``` javascript
git remote show origin
* remote origin
Fetch URL: git@github.com:spemoon/spemoon.github.com.git
Push  URL: git@github.com:spemoon/spemoon.github.com.git
HEAD branch: master
Remote branches:
..........
```

