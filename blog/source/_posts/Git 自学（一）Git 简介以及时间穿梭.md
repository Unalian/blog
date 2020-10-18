---
date: 2020/1/20
tags:
- git
- 自学笔记
categories:
- github

---
# 概览

一. 简介

​	1. 创建版本库

​	2. 把文件添加到文本库

二. 时间穿梭

1. 观察状态以及变化
2. 版本回退
  3. 工作区与暂存区
  4. 撤销修改
  5. 删除文件

<!-- more -->

# 简介

## 创建版本库

1.首先，选择一个合适的地方，创建一个空目录：

```
$ mkdir learngit
$ cd learngit
$ pwd
/Users/michael/learngit
```

**pwd**命令用于显示当前目录。

2.通过git init命令把这个目录变成Git可以管理的仓库：

```
$ git init
Initialized empty Git repository in /Users/michael/learngit/.git/
```

如果你没有看到.git目录，那是因为这个目录默认是隐藏的，用ls -ah命令就可以看见。

## 把文件添加到文本库

用终端建立文件：
1.vim 路径/***.txt
2.i
3.录入内容
4.esc
5.：wq

本实例中，把readme.txt放在之前建立的/Users/michael/learngit/下

内容是：

“Git is a version control system.
Git is free software.”

1.用命令git add告诉Git，把文件添加到仓库：

```
$ git add readme.txt
```

2.用命令git commit告诉Git，把文件提交到仓库：

```
$ git commit -m "wrote a readme file"
[master (root-commit) eaadf4e] wrote a readme file
 1 file changed, 2 insertions(+)
 create mode 100644 readme.txt
```

git commit命令:-m后面输入的是本次提交的说明，可以输入任意内容，当然最好是有意义的，这样你就能从历史记录里方便地找到改动记录。

git commit命令执行成功后会告诉你，1 file changed：1个文件被改动（我们新添加的readme.txt文件）；2 insertions：插入了两行内容（readme.txt有两行内容）。

因为commit可以一次提交很多文件，所以你可以多次add不同的文件，比如：

```
$ git add file1.txt
$ git add file2.txt file3.txt
$ git commit -m "add 3 files."
```

# 时间穿梭

## 观察状态以及变化

将之前的readme.txt 改成：

“Git is a distributed version control system.
Git is free software.”

运行**git status**命令看看结果：

```
$ git status
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

	modified:   readme.txt

no changes added to commit (use "git add" and/or "git commit -a")
```

git status命令可以让我们时刻掌握仓库当前的状态，上面的命令输出告诉我们，readme.txt被修改过了，但还没有准备提交的修改。

具体修改了什么内容，需要用**git diff**这个命令看看：

```
$ git diff readme.txt 
diff --git a/readme.txt b/readme.txt
index 46d49bf..9247db6 100644
--- a/readme.txt
+++ b/readme.txt
@@ -1,2 +1,2 @@
-Git is a version control system.
+Git is a distributed version control system.
 Git is free software.
```

可以从上面的命令输出看到，我们在第一行添加了一个distributed单词。

要将修改过的txt提交到版本库，还是需要git add和git commit.

```
$ git add readme.txt
```

在git commit之前用git status查看状态

```
$ git status
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)
modified: readme.txt
```

从 modified: readme.txt 可以看出readme.txt发生了变化但是还没有commit

```
$ git commit -m "add distributed"
[master e475afc] add distributed
 1 file changed, 1 insertion(+), 1 deletion(-)
```

提交后，查看状态

```
$ git status
On branch master
nothing to commit, working tree clean
```

## 版本回退

再次修改文件，改成：

“Git is a distributed version control system.
Git is free software distributed under the GPL.”

```
$ git add readme.txt
$ git commit -m "append GPL"
[master 1094adb] append GPL
 1 file changed, 1 insertion(+), 1 deletion(-)
```

回顾一下：
版本1：wrote a readme file
Git is a version control system.
Git is free software.

版本2：add distributed
Git is a distributed version control system.
Git is free software.

版本3：append GPL
Git is a distributed version control system.
Git is free software distributed under the GPL.

可以将**git log**命令查看已有版本:

```
$ git log
commit 1094adb7b9b3807259d8cb349e7df1d4d6477073 (HEAD -> master)
Author: Michael Liao <askxuefeng@gmail.com>
Date:   Fri May 18 21:06:15 2018 +0800

    append GPL

commit e475afc93c209a690c39c13a46716e8fa000c366
Author: Michael Liao <askxuefeng@gmail.com>
Date:   Fri May 18 21:03:36 2018 +0800

    add distributed

commit eaadf4e385e865d25c48e7ca9c8395c3f7dfaef0
Author: Michael Liao <askxuefeng@gmail.com>
Date:   Fri May 18 20:59:18 2018 +0800

    wrote a readme file
```

如果过于复杂，可用**git log --pretty=oneline**查看commit id和commit说明

```
$ git log --pretty=oneline
1094adb7b9b3807259d8cb349e7df1d4d6477073 (HEAD -> master) append GPL
e475afc93c209a690c39c13a46716e8fa000c366 add distributed
eaadf4e385e865d25c48e7ca9c8395c3f7dfaef0 wrote a readme file
```

当前版本是append GPL,要回溯到add distributed时,就可以使用**git reset**命令：：

首先，Git必须知道当前版本是哪个版本，在Git中，用HEAD表示当前版本，也就是最新的提交append GPL，上一个版本就是HEAD，上上一个版本就是HEAD，当然往上100个版本写100个比较容易数不过来，所以写成HEAD~100。

```
$ git reset --hard HEAD^
HEAD is now at e475afc add distributed
```

这时查看内容：

```
$ cat readme.txt
Git is a distributed version control system.
Git is free software.
```

这个时候如果查看版本库：

```
$ git log
commit e475afc93c209a690c39c13a46716e8fa000c366 (HEAD -> master)
Author: Michael Liao <askxuefeng@gmail.com>
Date:   Fri May 18 21:03:36 2018 +0800

    add distributed

commit eaadf4e385e865d25c48e7ca9c8395c3f7dfaef0
Author: Michael Liao <askxuefeng@gmail.com>
Date:   Fri May 18 20:59:18 2018 +0800

    wrote a readme file
```

append GPL的版本已经没有了。

找回方法：

1. **git reset --hard** – 查找版本号

```
$ git reset --hard 1094a
```

2. **git reflog** – 看之前的指令确定版本号

```
$ git reflog
e475afc HEAD@{1}: reset: moving to HEAD^
1094adb (HEAD -> master) HEAD@{2}: commit: append GPL
e475afc HEAD@{3}: commit: add distributed
eaadf4e HEAD@{4}: commit (initial): wrote a readme file
```

## 工作区与暂存区

git工作分为工作区（可见）与版本库（隐藏目录），在版本库里有暂存区（stage）和分支（master），git add相当于将工作区里被修改的文件加入stage，git commit相当于将stage里的文件一次性提交到master里。

几种状态：

1. 

```
$ git status
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

	modified:   readme.txt

Untracked files:
  (use "git add <file>..." to include in what will be committed)

	LICENSE

no changes added to commit (use "git add" and/or "git commit -a")
```

modified:readme.txt和no changes added to commit可以看出，readme.txt修改了还没有add。
untracked files:LICENSE和no changes added to commit可以看出，LICENSE从来没有添加过。

2. 

```
$ git status
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

	new file:   LICENSE
	modified:   readme.txt
```

changes to be committed可以看出，这两个文件add后还没有commit。

3. 

```
$ git status
On branch master
nothing to commit, working tree clean
```

提交以后，工作树就是干净的了。

查看工作区与版本库的区别：**git diff HEAD – readme.txt**

```
$ git diff HEAD -- readme.txt 
diff --git a/readme.txt b/readme.txt
index 76d770f..a9c5755 100644
--- a/readme.txt
+++ b/readme.txt
@@ -1,4 +1,4 @@
 Git is a distributed version control system.
 Git is free software distributed under the GPL.
 Git has a mutable index called stage.
-Git tracks changes.
+Git tracks changes of files.
```

## 撤销修改

1. 只是在工作区写错，将工作区的修改丢弃
   **git checkout - - <file>** 将工作区的改变撤销（恢复到最近的add/commit状态）
2. add后发现错误,将暂存区的修改回退到工作区
   **git reset HEAD <file>**

```
$ git reset HEAD readme.txt
Unstaged changes after reset:
M	readme.txt
```

3. commit后发现写错，直接回退版本。
   见版本回退

## 删除文件

工作区：直接从资源管理器里删除/rm <file>

```
    $ rm test.txt
```

此时git status可知，删除后还未添加到.git目录

```
$ git status
On branch master
Changes not staged for commit:
  (use "git add/rm <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

	deleted:    test.txt

no changes added to commit (use "git add" and/or "git commit -a")
```

之后git rm, git commit:

```
$ git rm test.txt
rm 'test.txt'

$ git commit -m "remove test.txt"
[master d46f35e] remove test.txt
 1 file changed, 1 deletion(-)
 delete mode 100644 test.txt
```

现在就把文件从版本库中被删除了。

删错时，可用$ git checkout - - <file>
**git checkout**其实是用版本库里的版本替换工作区的版本，无论工作区是修改还是删除，都可以“一键还原”。