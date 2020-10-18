---
date: 2020/1/31
tags:
- git
- 自学笔记
categories:
- github
---
# 概览

一. 标签管理

1. 创建标签
2. 操作标签

二. github

1. 使用码云 

<!-- more -->

# 标签管理

标签（tag）是版本库的一个快照，相当于绑定某个commit的指针，拥有一个比commit id 更加简洁的名字。

## 创建标签

命令git tag <tagname>用于新建一个标签，默认为HEAD，也可以指定一个commit id；

命令git tag -a <tagname> -m "blablabla…"可以指定标签信息；

命令git tag可以查看所有标签。

1. 切换至目标分支

```
$ git branch
* dev
  master
$ git checkout master
Switched to branch 'master'
```

2. **git tag <name>**创建标签

```
$ git tag v1.0
```

3. **git tag**查看所有标签

```
$ git tag
v1.0    
```

可通过commit id给版本打标签

```
$ git log --pretty=oneline --abbrev-commit
12a631b (HEAD -> master, tag: v1.0, origin/master) merged bug fix 101
4c805e2 fix bug 101
e1e9c68 merge with no-ff
f52c633 add merge
cf810e4 conflict fixed
5dc6824 & simple
14096d0 AND simple
b17d20e branch test
d46f35e remove test.txt
b84166e add test.txt
519219b git tracks changes
e43a48b understand how stage works
1094adb append GPL
e475afc add distributed
eaadf4e wrote a readme file
```

比方说要对add merge这次提交打标签，它对应的commit id是f52c633，敲入命令：

```
$ git tag v0.9 f52c633
$ git tag
v 0.9
v 1.0
```

用git show <tagname>查看tag信息

```
$ git show v0.9
commit f52c63349bc3c1593499807e5c8e972b82c8f286 (tag: v0.9)
Author: Michael Liao <askxuefeng@gmail.com>
Date:   Fri May 18 21:56:54 2018 +0800

    add merge

diff --git a/readme.txt b/readme.txt
...
```

可以绑定有说明的tag: git tag -a <tagname> -m “”

```
$ git tag -a v0.1 -m "version 0.1 released" 1094adb
$ git show v0.1
tag v0.1
Tagger: Michael Liao <askxuefeng@gmail.com>
Date:   Fri May 18 22:48:43 2018 +0800

version 0.1 released

commit 1094adb7b9b3807259d8cb349e7df1d4d6477073 (tag: v0.1)
Author: Michael Liao <askxuefeng@gmail.com>
Date:   Fri May 18 21:06:15 2018 +0800

    append GPL

diff --git a/readme.txt b/readme.txt
...
```

标签总是和某个commit挂钩。如果这个commit既出现在master分支，又出现在dev分支，那么在这两个分支上都可以看到这个标签。

## 操作标签

命令git push origin <tagname>可以推送一个本地标签；

命令git push origin --tags可以推送全部未推送过的本地标签；

命令git tag -d <tagname>可以删除一个本地标签；

命令git push origin :refs/tags/<tagname>可以删除一个远程标签。

tag打错了，git tag -d <tagname>可以删除。

```
$ git tag -d v0.1
Deleted tag 'v0.1' (was f15b0dd)
```

推送特定tag到远程**git push origin <tagname>**：

```
$ git push origin v1.0
Total 0 (delta 0), reused 0 (delta 0)
To github.com:michaelliao/learngit.git
 * [new tag]         v1.0 -> v1.0
```

一次性推送全部tag到远程：

```
$ git push origin --tags
Total 0 (delta 0), reused 0 (delta 0)
To github.com:michaelliao/learngit.git
 * [new tag]         v0.9 -> v0.9
```

删除远程标签：
首先删除本地标签：

```
$ git tag -d v0.9
Deleted tag 'v0.9' (was f52c633)
```

其次从远程删除：**$ git push origin :refs/tags/<tagname>**

```
$ git push origin :refs/tags/v0.9
To github.com:michaelliao/learngit.git
 - [deleted]         v0.9
```

# use Github

首先，在某个你想要参与修改的库中fork,相当于在自己的github账号底下克隆了一个远程库，这样的话，我们就可以对我们自己的远程库进行修改，最后将这个修改pull request给库主人，交给他审核即可。

对自己的库修改：

1. clone

```
git clone git@github.com:Unalian/learngit-1.git
```

2. change

```
vim Unalian.txt
//change the name:mv Unalian.txt una.txt
```

3. add una.txt

```
commit -m "add una.txt to master"
```

注意，以上是将本地进行了修改。

```
git push origin master
```

我们的远程库learngit-1修改好了，之后就可以pull request了。

## 使用码云

当github速度慢的时候可以进行操作

[http://git-scm.com](http://git-scm.com/) （指令总结）