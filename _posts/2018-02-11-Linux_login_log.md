---
layout: post
comments: true
title: 如何找到隐匿于last和w命令中的ssh登录痕迹
date: 2018-02-11
tag: Linux
---

有时入侵者会将自己的ssh登录痕迹隐匿于w和last命令中，一些经验不足的运维人员，可能发现不了这些已经发生的非法登录行为，亦或者察觉出有异常，但是确不理解为什么能将ssh登录痕迹隐匿于w和last命令（这种隐匿不涉及修改相关的文件）

###  现象及原理

 首先有一个正常的登录(bastion 登录至code-audit)，从pts/0登录 ( 14:31分正常登录)。
 
 在code-audit 上输入 w 命令显示。
 
 ![](/images/posts/linuxSsh/ssh_login_1.png)
 
在code-audit 上输入 last 命令显示。

![](/images/posts/linuxSsh/pts_0_last.png)

然后我利用一个小trick 将我的ssh登录痕迹隐匿于w 和last 命令中，且看下图。

![](/images/posts/linuxSsh/hide_ssh_connect.png)

在code-audit 上输入 w 命令显示。

![](/images/posts/linuxSsh/same_pts_w.png)

在code-audit 上输入 last 命令显示。

![](/images/posts/linuxSsh/same_last.png)

这时候，我们就会有以下疑问。

1）为什么w和last都没有记录呢?

这是因为w 命令显示信息来源于utmp，last 来源于wtmp，并不是所有程序登录的时候都会调用utmp 和wtmp 日志记录接口，只有交互式会话，才会调用utmp 和 wtmp的日志记录接口，比如 通过tty 或者pts或者图形界面登录的都会调用utmp 和wtmp 日志记录接口，然后我们在使用w 和last 命令的时候就会发现登录信息

2）ssh -lroot 192.168.12.51 /usr/bin/bash 为什么不属于交互式会话

ssh -lroot 192.168.12.51 /usr/bin/bash 其实就相当于登录之后直接调用bash这个名，此时系统没有为其分配tty，不算一个完整交互式会话，只不过bash 接受输入，然后有输出，让我们误以为是交互式会话，其实不然，你可以将/usr/bin/bash 替换成/usr/bin/ls 试一下，就是简单执行以下就退出了

![](/images/posts/linuxSsh/ssh_ls.png)

还有一种看起来更像是交互式会话的，但实际却不是的一种登录技巧，这种方式也会将登录行为隐匿于w 和 last 命令

![](/images/posts/linuxSsh/hide_ssh_connect2.png)

-T 表示不分配伪终端 （正常的会话，在分配伪终端之后才会调用utmp和wtmp的日志接口）

/usr/bin/bash -i   表示在登录之后 调用bash命令

-i 表示是交互式shell


###  如何发现隐匿的ssh登录行为

如果是隐匿的ssh正在进行，可以通过lsof 或者 netstat 或者ps 命令发现

1）通过lsof发觉异常ssh登录

![](/images/posts/linuxSsh/lsof_ssh.png)

两条已建立的连接，一条是通过pts/0正常登录的（前文已交代），一条就是隐匿于w和last的ssh登录

2）通过ps命令发现异常ssh登录

![](/images/posts/linuxSsh/ps_notty.png)

这里有个notty的 sshd 进程，说明就是通过上文所述的trick隐匿于w和last命令的ssh登录行为

如果是历史ssh 隐匿登录行为，如何找出历史登录行为呢

通过分析/var/log/secure 日志（有的系统是/var/log/auth.log）

![](/images/posts/linuxSsh/secure_ssh.png)

 从 Accepted publickey for root from 192.168.12.54 一行可以得出ssh的登录时间
 
 从Disconnected from 192.168.12.54 port 43000 一行可以得出ssh的退出时间
 
 如果从secure中的分析结果和 last 对不上，那么这些对不上的登录行为有可能就是通过本文所介绍的隐匿方式登录的
 
###  总结

其实像scp 、sftp 等也涉及到ssh登录，但却不会在w 和last中留下日志的程序，也都是因为他们不输入交互式会话。


### 每日一言

* 我们的幸福的来源不是我们得到了什么，而是我们在期待什么、追逐什么


本文转载自： [安全运维之如何找到隐匿于last和w命令中的ssh登录痕迹](http://www.freebuf.com/articles/system/182860.html)










 
 
 