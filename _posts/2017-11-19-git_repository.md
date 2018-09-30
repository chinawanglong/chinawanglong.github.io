---
layout: post
comments: true
title:  版本控制神器之Git
date: 2017-11-19
tag:  Linux
---

###  目录

* [Git起源](#origin)
* [集中式与分布式](#centralize_distributed)
* [安装](#install)
* [版本库](#repository)
* [Git常用操作](#base_operation)
* [远程仓库](#remote_repository)
* [分支](#dev)
* [标签管理](#manager_tag)


### <a name="origin"></a>git起源

大神Linus在1991年创建了开源的Linux， 之后， Linux不断发展，已成为最大的服务器系统软件。Linus虽然创建了Linux，但Linux的壮大是靠全世界热心的志愿者参与的，这么多人在世界各地为Linux编写代码，那Linux的代码是如何管理的？

事实上，在2002年以前， 世界各地的志愿者把源代码文件通过diff的方式发给Linus，然后由Linus本人通过手工方式合并代码！ 这是一项多么浩大的工程， 每天要研读那么多的代码，判断是否合适，然后在合并到Linux中， 一般人早崩溃了。 也许你会问， 为什么Linus不用版本控制软件，比如当时比较流行的`CVS`、`SVN`呢？ 因为LInus坚决地反对CVS和SVN，这些集中式的版本控制系统不但速度慢，而且必须联网才能使用。有一些商用的版本控制系统，虽然比CVS、SVN好用，但那是付费的，和Linux的开源精神不符。

这种2002年的时候得到改善，因为这个时候的Linux源码库已经非常大了，仅靠Linus一个人来维护，已经不现实，而且社区的开发者也对这种手动管理方式表达了强烈不满，于是Linus选择了一个商业的版本控制系统`BitKeeper`，BitKeeper的东家BitMover公司处于人道主义精神，授权Linux社区免费使用这个版本控制系统。

安定团结的大好局面在2005年被打破， 原因是Linux社区牛人聚集， 不免沾染了一些梁山好好的江湖习气。 开发Samba的Andrew试图破解BitKeeper的协议（这么干的其实也不只他一个），被BitMover公司发现了，于是BitMover要收回Linux社区的免费使用权。

于是，Linus花了两周的时间用C写了一个分布式版本控制系统，这就是Git！一个月内，Linux系统的源码已经由Git管理了。Git迅速成为最流行的分布式版本控制系统， 尤其是2008年，GitHub网站上线，它为开源项目免费提供Git存储，无数开源项目迁移至GitHub。

###  <a name="centralize_distributed"></a>集中式与分布式

上面提到的CVS和SVN都是集中式版本控制系统，Git是分布式版本控制系统。它们的区别是什么呢？

集中式版本控制系统， 版本哭是集中存放在中央服务器的，而干活的时候，用的都是自己的电脑，所以要先从中央服务器取得最新的版本，然后开始干活，干完之后，再把自己的代码推送到中央服务器。

 ![集中式版本控制系统示意图](/images/posts/git/svn.png)
 
 集中式版本控制系统最大的毛病就是必须联网才能工作，如果在局域网内还好，带宽勾搭，速度够快，可如果在互联网上，遇到网速慢的话，可能提交一个10M的文件就需要5分钟。而且当中央服务器出现问题，对于整个项目是毁灭性的打击。
 
 分布式版本控制系统， 和集中式版本控制系统相对，相对安全的多。分布式没有中央服务器，每个人的电脑上都是一个完整的版本库。在自己的电脑上就可以完成开发和管理。团队合作的时候，制药把自己本地仓库推送至远端仓库，实现同步就可以。 

![分布式版本控制系统示意图](/images/posts/git/git.png)

当然git还有很多有点，比如极其强大的分支管理，把SVN等远远抛在了后面。

CVS作为最早的开源而且免费的集中式版本控制系统，知道现在还有不少人使用。由于CVS自身设计的问题，会造成提交文件不完整，版本库莫名其妙损坏的情况。同样是开源而且免费的SVN修正了CVS的一些稳定性问题，是目前使用最多的集中式版本控制系统。

分布式版本控制系统除了Git以及BitKeeper之外，还有类似Git的Mercurial和Bazaar等。这些分布式版本控制系统各有千秋，但最快、最简单也最流行的依然是Git。

###  <a name="install"></a>安装

#### 在Linux上安装

```bash
# 测试系统是否安装了Git
$  git

# 使用Debian或者Ubuntu Linux
$ sudo apt-git install git 

``` 

老一点的Debian或者Ubuntu Linux，要把命令改为`sudo apt-get install git-core` , 因为以前有个软件也叫GIT（GUN Interactive Tools），所以git只能叫`git-core` 。 由于git名气太大了，后来GNU Interactive Tools改为`gnuit`, `git-core`正式更名为`git`.

源码安装： 先从Git官网下载源码，然后解压，依次输入`./config`,`make`,`sudo make install`就安装完成了。

####  在Mac上安装

推荐的安装方式： Homebrew

```bash
$ brew install git
```

####  设置用户信息

```bash
$ git config --global user.name "your name"
$ git config --global user.email "email@example.com"

```
`git config` 命令的`--global`参数，用了这个参数，表示这台机器上所有的Git仓库都会使用这个配置，当然也可以对某个仓库指定不同的用户名和email地址


###  <a name="repository"></a>版本库

版本库又名仓库， 可以理解为一个目录，这个目录里的所有文件都可以被Git管理起来，每个文件的修改、删除，Git都能跟踪， 以便任何时候都可以追踪历史，或者在将来某个时候可以还原。

```bash
# 创建版本库
$ mkdir learngit
$ cd learngit
$ git init

```

####  把文件添加到版本库

所有的版本控制系统， 其实只能跟踪文本文件的改动，比如TXT文件、网页、多有的程序代码等，Git也不例外。而图片、视频这些二进制文件，虽然也能由版本控制系统管理，但没法跟踪文件的变化，只能把二进制文件没次改动串起来，也就是知道图片从100KB改成了120KB，但是到底该了什么，版本控制系统不知道。

不幸的是，Microsoft的word是二进制格式，因此，版本控制系统没法跟踪word文件的改动，如果要真正使用版本控制系统，就要以纯文本方式编写文件。

文本是有编码的，比如中文常用的GBK，日文有Shift_JIS编码，如果没有历史遗留问题，强烈建议使用标准的UTF-8编码。

```bash
#  将 read.md 添加到仓库
$  git add read.md 

# 将read.md 提交到仓库
$ git commit -m'commit read.md'

```

###  工作区和暂存区

工作区就是能够看到的目录，一般就是项目所在的目录。工作区有一个隐藏目录`.git` ， 这个不算工作区，而是git的版本库。Git的版本库里存了很多东西，其中最重要的就是陈巍stage（或者叫index）的暂存区， 还有Git为我们自动创建的第一个分支`master`， 以及指向`master`的一个指针`HEAD` 。

![工作区、暂存区示意图](/images/posts/git/work_area.jpg)

* `git add `把文件添加进去，实际上就是把文件修改添加到暂存区
* `git commit`提交修改，实际上就是把暂存区的所有内容提交到当前分支

因为我们创建Git版本库时，Git自动为我们创建了唯一一个`master`分支，所以，`git commit` 就是往`master`分支上提交更改。






###  <a name="base_operation"></a>Git常用操作

```bash
# 克隆一个远程仓库
$ git clone repository_url   

# 查看当前仓库状态
$ git status

# 查看文件做了哪些修改
$ git diff filename

# 撤销工作区对文件的额修改 : 两种状态： 1。没有添加到暂存区，撤销修改之后的文件和版本库中的一样  2。 添加到了暂存区之后添加修改，撤销修改之后的文件和暂存区的一样
$ git checkout filename

# 查看提交历史
$ git log 

# 定制历史信息输出
$ git log --pretty=oneline

# 回退到上一个版本—— 版本库的回退
$ git reset --hard HEAD^

# 跳转至指定某个版本——版本库的回退
$ git reset --hard commit_id

# 将暂存区的文件回退到上一个版本
$ get reset HEAD filename

# 查看git命令历史
$ git reflog  

# 查看工作区文件与版本库最新版本的区别
$ git diff HEAD -- filename

```


###  <a name="remote_repository"></a>远程仓库

在GitHub上创建一个仓库`learngit`， 将本地仓库`learngit`关联新创建的GitHub仓库

```bash
$ git remote add origin git@github.com:yourname/learngit.git
```

添加之后，远程库的名字就是`origin` 。下一步，就可以把本地库的所有内容推送到远程库上（SSH key密钥要先上传GitHub）

```bash
$ git push -u origin master
```

把本地库的内容推送到远程， 用`git push`命令，实际上是把当前分支`master`推送到远程。由于远程仓库是空的，我们第一次推送`master`分支时，加上了`-u`参数，Git不但会把`master`分支内容推送到远程新的`master`分支，还会把本地`master`分支和远程`master`分支关联起来，在以后的推送或拉取就可以简化命令。


###  <a name="dev"></a>分支

每次提交，Git都会把他们串成一条时间线，每条时间线就是一个分支。截止目前，只有一条时间线，这个分支叫主分支，即`master`分支，`HEAD`严格来说不是指向提交，而是指向`master`，`master`才是指向提交的，所以，`HEAD`指向的就是当前分支。

![head_master示意图](/images/posts/git/head_master.jpeg)

每次提交，`master`分支都会向前移动一步，随之不断提交，`master`分支线也越来越长。


当我们创建新的分支，例如`dev`时，Git新建了一个指针叫`dev`，指向`master`相同的提交，再把`HEAD`指向`dev`，就表示当前分支在`dev`。


![dev分支示意图](/images/posts/git/dev.png)


```bash
#  创建dev分支并跳转至dev分支
$ git checkout -b dev

# 创建dev分支，然互跳转至dev分支
$ git branch dev
$ git checkout dev

# 查看当前分支
$ git branch 

```

创建新的`dev`分支后，对工作区的修改和提交就是针对`dev`分支了，比如新提交一次后，`dev`分支向前移动一步，而`master`指针不变。

```bash
#  在dev分支作修改 并提交
$ git add .
$ git commit -m'new modify'

# 切换至master分支
$ git checkout master

#  这时是看不到提交的修改的，因为修改的部分在dev分支上，而master此刻的提交点并没有改变

```

假设我们的`dev`上的工作完成了，就可以把`dev`合并到 `master`上。怎么合并呢？  最简单的办法，就是直接把`master`指向`dev`的当前提交，就完成了合并。

![dev、master分支合并示意图](/images/posts/git/master_dev.png)


```bash
#  合并dev分支
$ git merge dev

# git merge 命令用于合并指定分支到当前分支，合并之后就可以查看刚才提交的修改了

```

所以Git合并也很快！就改改指针，工作区内容也不变。

合并完成后，甚至可以删除`dev`分支。删除`dev`分支就是把`dev`指针删除，删除后，我们就剩下一条`master`分支。

```bash
#  合并完成之后，删除dev分支
$ git branch -d dev
```


####  解决冲突

合并的过程中常常会遇到冲突的问题

分支切换提交时，很容易碰到冲突问题，Git用`<<<<<<`,`======`,`>>>>>>`标记不同分支的内容，修改手动修改，然后提交


####  分支管理

Git删除分支时一般采用的是`Fast forward`模式， 但这种模式下，删除分支后，会丢失分支信息。如果强制禁用`Fast forward`模式， Git就会在merge时生成一个新的commit，这样就可以从分支历史看出分支信息。

禁用`Fast forward`模式，在`git merge`命令中加入参数`--no-ff`

```bash
# 禁用 Fast forward模式合并dev分支
$ git merge --no-ff -m'merge with no-ff' dev
```

在实际开发中，我们应该按照几个基本原则进行分支管理
* `master`分支应该是非常稳定的，仅用来发布新版本，不能在上面干活
* 所有代码的提交应该建立一个类似的`dev`分支
* 开发组成员clone`dev`分支，所有代码的提交应提交到dev分支，要发布新版本时，将`dev`分支合并到`master`分支
* 每个开发成员应建立自己的特性分支，代码往`dev`合并

#### Bug分支

软件开发中，Bug的出现是不可避免的。 在Git中，由于分支是如此的强大，所以，每个Bug都可以通过一个新的临时分支来修复，修复后，合并分支，然后删除临时分支。

假如当前有个编号为110的bug修复任务，而当前`dev`的工作还没有完成，这时就要采用Git提供的`stash`功能。`stash`可以把当前工作现场储藏起来，等以后恢复现场后继续工作。

```bash
$ git stash
 Saved working directory and index state WIP on dev: f52c633 add merge
```

现在用`git status` 查看工作区，就是干净的，没有未提交的文件，一次可以放心地创建分支来修复bug。

首先确定要在哪个分支上修复Bug，假定需要在`master`分支上修复，就从`master`创建临时分支。

```bash
# 切换到master分支
$ git master

# 创建并切换至bug分支
$ git checkout -b issue-110

#  完成bug修复之后， 提交修改的文件
$ git add filename
$ git commit -m'fix bug 110'

# 切换回master，合并issue-110分支
$ git checkout master
$ git merge --no-ff -m'merge bug fix 110' issue-110

# 至此，完成bug110修复，回到dev的开发分支上继续开发任务
$ git checkout dev

# 该dev分支我们使用git stash封存了,查看封存的记录
$ git stash list
 stash@{0}: WIP on dev: f52c633 add merge
 
# 恢复封存的方法有两种，1。git stash apply恢复，但是恢复后，stash内容并不删除，需要用git stash drop来删除； 2。git stash pop， 恢复的同时把stash内容也删除了
$ git stash pop stash@{0}
 
```

#### 销毁分支

开发的过程中需求是不断变化，谁也不能保证需求一成不变，开发的每一个功能都能成功运用都真是环境中。正确的开发协作方式，应该是每一个需求都对应了一个特性分支，当某个需求被砍掉的时候，只要删除其对应的特性分支就可以了。

```bash
# 见一个特性分支，开发财务流程
$ git checkout -b finance-flow

# 开发完成，提交所有文件
$ git add .
$ git commit -m'finance flow finish'

# 这时被通知砍掉该需求，删掉finance-flow分支
$ git checkout dev
$ git branch -d finance-flow

# 这时Git会提醒， 由于finance-flow未合并，不能删除，需要要-D参数强制删除
$ git branch -D finance-flow
```


####  多人协作

在合作开发过程中，每个人开发的需求不一样，就会出现不同的特性分支，不同的特性分支推送到远程仓库，这就关联到分支推送合并和的问题

当我们从远程仓库克隆时，实际上Git自动把本地的`master`分支和远程的`master`分支对应起来（除非克隆的时候指定了分支）,远程仓库的默认名称是`origin`。

```bash
# 查看远程仓库的信息
$ git remote

# 查看更详细的信息
$ git remote -v
```

推送分支就是把该分支上的所有本地提交推送到远程库。推送时，要指定本地分支，这样，Git就会把该分支推送到远程库对应的远程分支上
```bash
$ git push origin master

# 如果要推送其他分支，比如dev
$ git push origin dev
```

但是，并不一定要把本地分支向远程推送，那么，哪些分支需要推动，哪些不需要？

* `master`分支是主分支，一次要时刻与远程同步
* `dev`分支是开发分支， 团队成员都需要在上面工作，所以也需要与远程同步
* bug分支只用于本地修复bug，没有必要推送到远程
* 特性分支，取决与是否与团队成员合作开发

```bash
# clone的时候没有指定分支，那么git默认本地是master分支对应远程的master分支，但是master分支一般来说是不能动的，只能在dev这个开发分支上进行开发，这时就要切换一个新分支对应远程的dev分支
$ git checkout -b dev origin/dev
```

多人协作的模式：

* 用`git push origin <branch-name>推动自己的修改
* 如果推送失败，则因为远程分支比本地的跟新，需要先`git pull`试图融合
* 如何合并有冲突，则解决冲突，并在本地提交
* 在用 `git push origin <branch-name>` 推送


如果 `git pull`提示`no tracking information` ，则说明本地分支和远程分支的链接关系没有创建，用命令`git push --set-upstream-to <branch-name> origin/<branch-name>`

 
###  <a name="manager_tag"></a>标签管理

在开发的一些关键时期，使用标签来记录这些关键时刻，比如发布版本，有重大升级，升级的时候，会使用标签记录这些时刻，开永久标记项目中的关键历史时刻

标签分类

* 轻量级标签（lightweight）： 轻量级标签的信息量很少，这种标签至代表某时刻代码的提交，相当于指向这个提交的指针

* 带注释标签（annotated）： 这种标签是一种校验和，包好标签名、邮箱、日期、标签信息、GPG签名和验证，它相当于一个对象，封装来这些信息 

####  创建标签

在Git中打标签非常简单，首先，切换到需要打标签的分支上

```bash
$ git branch
$ git checkout master

#  打个标签-轻量级标签
$ git tag v1.0

# 查看标签
$ git tag
```

默认标签是打在最新提交的commit上的。也可以指定commit_id，在指定提交上打标签。

```bash
# 指定标签名 -a  指定说明文字-m   指定提交 1049as
$ git tag -a v2.0 -m'version 2.0 released' 1049as
```


####  操作标签

如果标签打错了，也可以删除
```bash
$ git tag -d v1.0
```
因为创建的标签只存储在本地，不会自动推动到远程，所以，打错的标签可以在本地安全删除。

如果要推送某个标签到远程，使用命令`git push origin <tagbane>`

```bash
$ git push origin tag v2.0

# 一次性推送全部尚未推送到远程库的本地标签
$ git push origin --tags

# 如果标签已经推送到远程，要删除远程标签就麻烦些，先从本地删除
$ git tag -d v2.0

# 然后从远程删除，删除命令也是push
$ git push origin :ref/tags/v2.0
```

   

