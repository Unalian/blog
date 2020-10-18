---
date: 2020/1/28
tags:
- git
- 自学笔记
categories:
- github
---
# 概览

一. 远程仓库

1. 创建SSH密钥
2. 创建远程库
3. 从远程库克隆

二. 分支管理

1. 创建与合并分支

2. 解决冲突

3. 分支管理策略

4. Bug分支

5. feature分支

6. 多人协作

7. rebase

   <!-- more -->

## 远程仓库

## 创建SSH密钥

github提供远程仓库，本地Git仓库和GitHub仓库之间的传输是通过SSH加密的，需要设置：

创建SSH key

```
$ ssh-keygen -t rsa -C "Dengmengda@Gmail.com"
```

一路回车

如果一切顺利的话，可以在用户主目(cd ~)(ls)录里找到.ssh目录，里面有id_rsa和id_rsa.pub两个文件，这两个就是SSH Key的秘钥对，id_rsa是私钥，不能泄露出去，id_rsa.pub是公钥，可以放心地告诉任何人。

查看：(cat)

```
adeMacBook-Air:.ssh una$ cat ~/.ssh/id_rsa.pub
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQD2WRdPq72pjh5ZiG7fzhFwHsZtQ7KbWA4ZF0G6LoFmh4JTSkGOTBaixu8+NNmRksxYKVenYwqFPT2meWocDm19Lly6wN6+rxdf9pmiRRv+5zJb4XkGi2j4+foPt1XPYc1FZM6lzeyh127W5+2VPZDG7hKatzh9OEsiKXHI5OaANPpW+diPs/kAEMsrMMuzMOUTJFhbqaS48Pv1aZaSBotI2YbHjOEGA2KAXH1dEepLM17fpVvFV3eVi5k+erTOj38yxiW598x+ZBrig5Jwf+6X1LJiB7VFW1QEqCcQxf1aEgErhRmrxEp+YWnJQCJHvWjp8N7f+txYFkoBkJfoq4zD Dengmengda@Gmail.com
```

之后，登录github，在settings->SSH key添加SSHkey(id_rsa.pub公钥)

## 创建远程库

在本地创建了一个Git仓库后，又想在GitHub创建一个Git仓库，让这两个仓库进行远程同步。So,GitHub上的仓库既可以作为备份，又可以让其他人通过该仓库来协作

在github里creat a new repository

在GitHub上的这个learngit仓库还是空的，GitHub告诉我们，可以从这个仓库克隆出新的仓库，也可以把一个已有的本地仓库与之关联，然后，把本地仓库的内容推送到GitHub仓库。

根据提示

```
git remote add origin https://github.com/Unalian/learngit.git
git push -u origin master
```

由于远程库是空的，我们第一次推送master分支时，加上了-u参数，Git不但会把本地的master分支内容推送的远程新的master分支，还会把本地的master分支和远程的master分支关联起来，在以后的推送或者拉取时就可以简化命令。

之后就可以在github的页面里看见了。
以后修改就可以

```
$ git push origin master
```

将修改推至远程版本库了，这样就真正拥有了一个分布式版本库。

在过程中会有SSH警告，yes即可。

## 从远程库克隆

在github上建立一个新的库，勾选**Initialize this repository with a README**，这样系统会自动建立一个README.md的文件。这样远程库就准备好了。

下一步是用命令git clone克隆一个本地库：

```
$ git clone git@github.com:Unalian/gitskills.git
Cloning into 'gitskills'...
remote: Counting objects: 3, done.
remote: Total 3 (delta 0), reused 0 (delta 0), pack-reused 3
Receiving objects: 100% (3/3), done.
```

Git支持多种协议，包括https，但通过ssh支持的原生git协议速度最快。

# 分支管理

分支：
分支在实际中有什么用呢？假设你准备开发一个新功能，但是需要两周才能完成，第一周你写了50%的代码，如果立刻提交，由于代码还没写完，不完整的代码库会导致别人不能干活了。如果等代码全部写完再一次提交，又存在丢失每天进度的巨大风险。
现在有了分支，就不用怕了。你创建了一个属于你自己的分支，别人看不到，还继续在原来的分支上正常工作，而你在自己的分支上干活，想提交就提交，直到开发完毕后，再一次性合并到原来的分支上，这样，既安全，又不影响别人工作。

## 创建与合并分支

创建dev分支

```
$ git branch dev
```

切换到dev分支

```
$ git checkout dev
Switched to branch 'dev'
```

将两个指令综合起来就是

```
$ git checkout -b dev
Switched to a new branch 'dev'
```

查看所有分支（当前分支前有*）

```
$ git branch
* dev
  master
```

然后在dev分支上正常提交，比如在readme.txt上进行修改
加上一行
Creating a new branch is quick.

提交：

```
$ git add readme.txt 
$ git commit -m "branch test"
[dev b17d20e] branch test
 1 file changed, 1 insertion(+)
```

dev分支搞定，现在切回master分支：

```
$ git checkout master
Switched to branch 'master'
```

如果此时查看readme.txt，就会发现还是没有改变。因此，需要合并branch到master上。

```
$ git merge dev
Updating d46f35e..b17d20e
Fast-forward
 readme.txt | 1 +
 1 file changed, 1 insertion(+)
```

这里提到，本次合并是Fast-forward，因此，是直接将master指向dev的当前提交，合并速度快，但是也有其他的合并方式。

通常，合并分支时，如果可能，Git会用Fast forward模式，但这种模式下，删除分支后，会丢掉分支信息。

如果要强制禁用Fast forward模式，Git就会在merge时生成一个新的commit，这样，从分支历史上就可以看出分支信息。

合并完成后，就可以删除dev分支。

```
$ git branch -d dev
Deleted branch dev (was b17d20e).
```

总结：

```
查看分支：git branch

创建分支：git branch <name>

切换分支：git checkout <name>

创建+切换分支：git checkout -b <name>

合并某分支到当前分支：git merge <name>

删除分支：git branch -d <name>
```

## 解决冲突

创建feature1分支，然后在master，feature1都进行修改，在尝试快速合并时会出现冲突。

```
$ git merge feature1
Auto-merging readme.txt
CONFLICT (content): Merge conflict in readme.txt
Automatic merge failed; fix conflicts and then commit the result.
```

这个时候查看状态：

```
$ git status
On branch master
Your branch is ahead of 'origin/master' by 2 commits.
  (use "git push" to publish your local commits)

You have unmerged paths.
  (fix conflicts and run "git commit")
  (use "git merge --abort" to abort the merge)

Unmerged paths:
  (use "git add <file>..." to mark resolution)

	both modified:   readme.txt

no changes added to commit (use "git add" and/or "git commit -a")
```

这个时候查看readme.txt的内容：

```
Git is a distributed version control system.
Git is free software distributed under the GPL.
Git has a mutable index called stage.
Git tracks changes of files.
<<<<<<< HEAD
Creating a new branch is quick & simple.
=======
Creating a new branch is quick AND simple.
>>>>>>> feature1
```

可清晰看出不同分支的区别，这个时候就可以手动修改文件，解决冲突。

git log也可以查看分支合并情况。

最后，删除feature1就可以了：

```
$git branch -d feature1
```

## 分支管理策略

1. fast-forward
2. `--no-ff` (强制禁用fast-forward)

```
$ git merge --no-ff -m "merge with no-ff" dev
Merge made by the 'recursive' strategy.
 readme.txt | 1 +
 1 file changed, 1 insertion(+)
```

因为本次合并要创建一个新的commit，所以加上-m参数，把commit描述写进去。
合并分支时，加上–no-ff参数就可以用普通模式合并，合并后的历史有分支，能看出来曾经做过合并，而fast forward合并就看不出来曾经做过合并。

```
$ git log --graph --pretty=oneline --abbrev-commit
*   e1e9c68 (HEAD -> master) merge with no-ff
|\  
| * f52c633 (dev) add merge
|/  
*   cf810e4 conflict fixed
```

## Bug分支

修复bug时，我们会通过创建新的bug分支进行修复，然后合并，最后删除；

当手头工作没有完成时，先把工作现场git stash一下，然后去修复bug，修复后，再git stash pop，回到工作现场；

在master分支上修复的bug，想要合并到当前dev分支，可以用git cherry-pick <commit>命令，把bug提交的修改“复制”到当前分支，避免重复劳动。

比如此刻在dev工作，但是master出现了bug，要切换到master然后开一个BUG分支。这就要求dev分支commit。但是，dev的工作还没有做完，不想将它commit，因此用git stash将工作区封存。

```
$ git stash
Saved working directory and index state WIP on dev: f52c633 add merge
```

将dev工作区封存，此时可以开始修改bug

```
$ git checkout master
Switched to branch 'master'
Your branch is ahead of 'origin/master' by 6 commits.
  (use "git push" to publish your local commits)

$ git checkout -b issue-101
Switched to a new branch 'issue-101'

$ git add readme.txt 
$ git commit -m "fix bug 101"
[issue-101 4c805e2] fix bug 101
 1 file changed, 1 insertion(+), 1 deletion(-)

$ git checkout master
Switched to branch 'master'
Your branch is ahead of 'origin/master' by 6 commits.
  (use "git push" to publish your local commits)

$ git merge --no-ff -m "merged bug fix 101" issue-101
Merge made by the 'recursive' strategy.
 readme.txt | 2 +-
 1 file changed, 1 insertion(+), 1 deletion(-)
 
 $ git branch -d issue-101
Deleted branch issue-101 (was b17d20e).
```

将bug修改完毕以后可以开始重回dev工作。第一件事就是找回封存区的文件：

```
$ git stash list
stash@{0}: WIP on dev: f52c633 add merge
```

发现文件被存在某个地方，第二件事就是恢复文件。

一是用git stash apply恢复，但是恢复后，stash内容并不删除，你需要用git stash drop来删除；

```
$ git stash apply stash@{0}
$ git stash drop stash@{0}
```

另一种方式是用git stash pop，恢复的同时把stash内容也删了：

```
$ git stash pop
On branch dev
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

	new file:   hello.py

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

	modified:   readme.txt

Dropped refs/stash@{0} (5d677e2ee266f39ea296182fb2354265b91b3b2a)
```

再用git stash list康，发现stash已经空了。

这个时候dev可以继续工作了。但是我们想要将我们在master上做的bug修改复制到dev上。当然，我们可以重复操作，不过没必要。同样的bug，要在dev上修复，我们只需要把4c805e2 fix bug 101这个提交所做的修改“复制”到dev分支。

注意：我们只想复制4c805e2 fix bug 101这个提交所做的修改，并不是把整个master分支merge过来。

为了方便操作，Git专门提供了一个**cherry-pick**命令，让我们能复制一个特定的提交到当前分支：

```
$ git branch
* dev
  master
$ git cherry-pick 4c805e2
[master 1d4b803] fix bug 101
 1 file changed, 1 insertion(+), 1 deletion(-)
```

## feature分支

一个新功能，最好用feature分支实验，如果实验失败，可以直接删除。
这个时候，因为分支没有合并，所以需要强行删除：

```
$ git branch -D feature-vulcan
Deleted branch feature-vulcan (was 287773e).
```

## 多人协作

多人协作的工作模式通常是这样：

1. 试图用git push origin <branch-name>推送自己的修改；
2. 如果推送失败，则因为远程分支比你的本地更新，需要先用git pull试图合并；
3. 如果git pull提示no tracking information，则说明本地分支和远程分支的链接关系没有创建，用命令git branch --set-upstream-to <branch-name> origin/<branch-name>。
4. 如果合并有冲突，则解决冲突，并在本地提交；
5. 没有冲突或者解决掉冲突后，再用git push origin <branch-name>推送就能成功！

基本指令：

1. 查看远程库信息，使用git remote -v；
2. 本地新建的分支如果不推送到远程，对其他人就是不可见的；
3. 从本地推送分支，使用git push origin branch-name，如果推送失败，先用git pull抓取远程的新提交；
4. 在本地创建和远程分支对应的分支，使用git checkout -b branch-name origin/branch-name，本地和远程分支的名称最好一致；
5. 建立本地分支和远程分支的关联，使用git branch --set-upstream branch-name origin/branch-name；
6. 从远程抓取分支，使用git pull，如果有冲突，要先处理冲突。

<1>查看远程库
查看远程库信息：

```
$ git remote
origin
```

查看远程库详细信息：

```
$ git remote -v
origin  git@github.com:michaelliao/learngit.git (fetch)
origin  git@github.com:michaelliao/learngit.git (push)
```

如果没有推送权限，就看不见push地址

<2>推送分支
推送本地master到远程库

```
$ git push origin master
```

推送本地dev到远程库

```
$ git push origin dev
```

- master分支是主分支，因此要时刻与远程同步；
- dev分支是开发分支，团队所有成员都需要在上面工作，所以也需要与远程同步；
- bug分支只用于在本地修复bug，就没必要推到远程了，除非老板要看看你每周到底修复了几个bug；
- feature分支是否推到远程，取决于你是否和你的小伙伴合作在上面开发。

<3>抓取分支

将远程库克隆：

```
git clone git@github.com:Unalian/learngit.git
$ git branch
* master
```

此时只有master。（git branch q 退出）

要在dev分支上开发，就必须创建远程origin的dev分支到本地：

```
$ git checkout -b dev origin/dev
```

现在，他就可以在dev上继续修改，然后，时不时地把dev分支push到远程：

```
$ git commit -m "add env"
[dev 7a5e5dd] add env
 1 file changed, 1 insertion(+)
 create mode 100644 env.txt

$ git push origin dev
Counting objects: 3, done.
Delta compression using up to 4 threads.
Compressing objects: 100% (2/2), done.
Writing objects: 100% (3/3), 308 bytes | 308.00 KiB/s, done.
Total 3 (delta 0), reused 0 (delta 0)
To github.com:michaelliao/learngit.git
   f52c633..7a5e5dd  dev -> dev
```

这个push的时候，如果发现有一个人也同时将自己的更改push了，此时，就会发生冲突。因此，我们需要pull最新版本的提交，并与我将要push的版本进行冲突处理，最后再进行push。

将最新的提交从origin/dev上抓取下来：

```
$ git pull
There is no tracking information for the current branch.
Please specify which branch you want to merge with.
See git-pull(1) for details.

    git pull <remote> <branch>

If you wish to set tracking information for this branch you can do so with:

    git branch --set-upstream-to=origin/<branch> dev
```

这里显示抓取失败，原因是没有指定本地dev分支与远程origin/dev分支的链接，根据提示，设置dev和origin/dev的链接：

```
$ git branch --set-upstream-to=origin/dev dev
Branch 'dev' set up to track remote branch 'dev' from 'origin'.
```

再pull：

```
$ git pull
Auto-merging env.txt
CONFLICT (add/add): Merge conflict in env.txt
Automatic merge failed; fix conflicts and then commit the result.
```

pull成功但是合并出现冲突,手动处理冲突，再次push。（pull的同时会发生合并）

```
$ git commit -m "fix env conflict"
[dev 57c53ab] fix env conflict

$ git push origin dev
Counting objects: 6, done.
Delta compression using up to 4 threads.
Compressing objects: 100% (4/4), done.
Writing objects: 100% (6/6), 621 bytes | 621.00 KiB/s, done.
Total 6 (delta 0), reused 0 (delta 0)
To github.com:michaelliao/learngit.git
   7a5e5dd..57c53ab  dev -> dev
```

## rebase

rebase操作可以把本地未push的分叉提交历史整理成直线；

rebase的目的是使得我们在查看历史提交的变化时更容易，因为分叉的提交需要三方对比。

在上一节我们看到了，多人在同一个分支上协作时，很容易出现冲突。即使没有冲突，后push的童鞋不得不先pull，在本地合并，然后才能push成功。

每次合并再push后，分支变成了这样：

```
$ git log --graph --pretty=oneline --abbrev-commit
* d1be385 (HEAD -> master, origin/master) init hello
*   e5e69f1 Merge branch 'dev'
|\  
| *   57c53ab (origin/dev, dev) fix env conflict
| |\  
| | * 7a5e5dd add env
| * | 7bd91f1 add new env
| |/  
* |   12a631b merged bug fix 101
|\ \  
| * | 4c805e2 fix bug 101
|/ /  
* |   e1e9c68 merge with no-ff
|\ \  
| |/  
| * f52c633 add merge
|/  
*   cf810e4 conflict fixed
```

注意到Git用(HEAD -> master)和(origin/master)标识出当前分支的HEAD和远程origin的位置分别是582d922 add author和d1be385 init hello，本地分支比远程分支快两个提交。

现在我们尝试推送本地分支：

```
$ git push origin master
To github.com:michaelliao/learngit.git
 ! [rejected]        master -> master (fetch first)
error: failed to push some refs to 'git@github.com:michaelliao/learngit.git'
hint: Updates were rejected because the remote contains work that you do
hint: not have locally. This is usually caused by another repository pushing
hint: to the same ref. You may want to first integrate the remote changes
hint: (e.g., 'git pull ...') before pushing again.
hint: See the 'Note about fast-forwards' in 'git push --help' for details.
```

很不幸，失败了，这说明有人先于我们推送了远程分支。按照经验，先pull一下：

```
$ git pull
remote: Counting objects: 3, done.
remote: Compressing objects: 100% (1/1), done.
remote: Total 3 (delta 1), reused 3 (delta 1), pack-reused 0
Unpacking objects: 100% (3/3), done.
From github.com:michaelliao/learngit
   d1be385..f005ed4  master     -> origin/master
 * [new tag]         v1.0       -> v1.0
Auto-merging hello.py
Merge made by the 'recursive' strategy.
 hello.py | 1 +
 1 file changed, 1 insertion(+)
```

再用git status看看状态：

```
$ git status
On branch master
Your branch is ahead of 'origin/master' by 3 commits.
  (use "git push" to publish your local commits)

nothing to commit, working tree clean
```

加上刚才合并的提交，现在我们本地分支比远程分支超前3个提交。

用git log看看：

```
$ git log --graph --pretty=oneline --abbrev-commit
*   e0ea545 (HEAD -> master) Merge branch 'master' of github.com:michaelliao/learngit
|\  
| * f005ed4 (origin/master) set exit=1
* | 582d922 add author
* | 8875536 add comment
|/  
* d1be385 init hello
...
```

提交历史分叉就很不方便了，我们想把这些化成一条直线。
输入命令**git rebase**

```
$ git rebase
First, rewinding head to replay your work on top of it...
Applying: add comment
Using index info to reconstruct a base tree...
M	hello.py
Falling back to patching base and 3-way merge...
Auto-merging hello.py
Applying: add author
Using index info to reconstruct a base tree...
M	hello.py
Falling back to patching base and 3-way merge...
Auto-merging hello.py
```

再git log

```
$ git log --graph --pretty=oneline --abbrev-commit
* 7e61ed4 (HEAD -> master) add author
* 3611cfe add comment
* f005ed4 (origin/master) set exit=1
* d1be385 init hello
...
```

这就是rebase操作的特点：把分叉的提交历史“整理”成一条直线，看上去更直观。缺点是本地的分叉提交已经被修改过了。

最后，通过push操作把本地分支推送到远程：

```
Mac:~/learngit michael$ git push origin master
Counting objects: 6, done.
Delta compression using up to 4 threads.
Compressing objects: 100% (5/5), done.
Writing objects: 100% (6/6), 576 bytes | 576.00 KiB/s, done.
Total 6 (delta 2), reused 0 (delta 0)
remote: Resolving deltas: 100% (2/2), completed with 1 local object.
To github.com:michaelliao/learngit.git
   f005ed4..7e61ed4  master -> master
```