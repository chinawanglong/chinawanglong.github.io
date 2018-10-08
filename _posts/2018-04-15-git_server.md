---
layout: post
comment: true
title:  Git服务器的搭建
date: 2018-04-15
tag: Linux
---

第一步：安装git

```bash
$ sudo yum install git
```

第二步： 创建一个git用户，用来运行git服务
```bash
$ sudo adduser git
```

第三步：创建证书登录

收集所有需要登录用户的公钥，就是其id_rsa.pub文件，把所有公钥导入到`/home/git/.ssh/authorized_keys`文件里，一行一个。

第四步： 初始化git仓库

先选定一个目录作为git仓库，假定是`/usr/git/sample.git`， 在`/usr/git`目录下
```bash
$ sudo git init --bare sample.git
```

git就会创建一个裸仓库，裸仓库没有工作区，服务器上的仓库纯粹是为了分享，所以不让用户直接登录到服务器上去该工作区，并且服务器上的git仓库通常是以`.git`结尾。把目录的工作组和所有者改为`git`
```bash
$ sudo chown -R git:git sample.git
```

第五步： 禁用shell登录

出于安全考虑，等二步创建的git用户不允许登录shell
```bash
$ sudo vim /etc/passwd

# 将下面这行
git:x:1001:1001:,,,:/home/git:/bin/bash
# 改为
git:x:1001:1001:,,,:/home/git:/usr/bin/git-shell
```

这样，`git`用户可以正常通过ssh使用git，但无法登录shell，因为设定`git-shell`每次一登录就自动退出。

下面进行正常的`git clone`就可以了。

###  每日一言

*  引路靠路人，走路靠自己，成长靠学习，成就靠团队。

<br>

转载请注明： [王龙的博客](http://wanglong.org.cn) >> [Git服务器的搭建](http://wanglong.org.cn/2018/04/git_server/)