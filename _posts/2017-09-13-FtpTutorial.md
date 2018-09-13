---
layout: post
comments: true
title:  搭建FTP服务器
date: 2017-09-13
tag: Linux
---

###  安装vsftpd
 
```
 # 检查系统是否安装了vsftpd
 rpm -qa | grep vsftpd
 vsftpd-3.0.2-22.el7.x86_64
 
 # 系统没有安装，执行命令
 yum install -y vsftpd
  
```

### 配置介绍

####  用户介绍

* 本地用户（local）: 在FTP服务器上拥有账号，且该账号为本地用户的账号，可以通过自己的账号和口令进行授权登录，登录目录为自己的home目录$HOME

* 虚拟用户（guest）: 用户在FTP上拥有账号，但该账号只能用于文件传输服务。登录目录为某一特定目录，通过可以用来上传和下载

* 匿名用户（anonymous）: 用户在FTP上没有账号，登录目录为/var/ftp 

####  链接模式

* 主动（Port）模式：当客户端C想服务器S连接后，使用的是Port模式。客户端C会发送一条命令给服务端S（客户端C在本地打开一个端口N等待S进行数据连接）， 当服务端S收到这个Port命令后，就会想客户端打开的端口N进行连接

* 被动（Pasv）模式： 当客户端C向服务端S连接后，服务端S会发送小心给客户端C，服务端S在本地打开一个端口M，等待客户端C去连接。  客户端C接收到消息，就会向服务端S的M端口进行连接，连接成功后，数据通道就建立了

####  防火墙管理

&nbsp;&nbsp;&nbsp;&nbsp;
tips: 对于Centos7 ，它的默认防火墙已改为FireWall进行管理，但是还有用iptables的，以及SELinux

* firewall 能够允许哪些服务可用，哪些端口可用。。。属于更高一层的防火墙。firewall 的底层是使用iptables进行数据过滤，建立在iptables之上。
 firewall 是动态防火墙，使用D-BUS方式，修改配置不是破坏已有的数据连接
 
*  iptables 用户过滤数据包，属于网络层防火墙。在设置iptables后需要重启iptables, 会重新加载防火墙模块，而模块的装载会破坏状态防火墙和确立的连接。 会破坏已经对外提供数据链接的程序，需要重启程序。

*  SELinux(Security-Enhanced Linux) 是美国国家安全局对于强制访问控制的是想， 是Linux历史上最杰出的新安全子系统。 它不是用来设置防火墙的， 但它对Linux系统的安全很有用， Linux内核从2.6就有了SELinux。SELinux是一种基于`域-类型` 模型（domain-type）的强制访问控制（MAC）安全系统，它由NSA编写并设计成内核模块包含到内核中，相应的某些安全相关的应用也被打了SELinux补丁，最后还有一个相应的安全策略。

###  配置vsftpd

```
  // 首先查看下vsftpd本身有什么属性
  cat /etv/vsftpd/vsftpd.conf | grep -v '#' | more
   
  // 打开vsftpd配置文件
  vim /etc/vsftpd/vsftpd.conf
  // 基本配置
  # 开启匿名登录
  anonymous_enable=YES
  # 允许使用本地账户进行FTP用户登录验证
  local_enable=YES
  # 可写
  write_enable=YES
  # 本地用户默认文件掩码022
  local_umask=022
  # 允许匿名上传
  anon_upload_enable=YES
  # 允许匿名创建目录
  anon_mkdir_write_enbale=YES
  # 当访问某个目录时可发送消息
  dirmessage_enable=YES
  #  开启上传下载记录
  xferlog_enable=YES
  # 数据链建立端口为20
  connect_from_port_20=YES
  # 允许其它用户上传匿名文件
  #chown_uploads=YES
  # 所有用户
  #chown_username=whoever
  # 日志保存到
  #xferlog_file=/var/log/xferlog
  # 日志标准输出
  xferlog_std_format=YES
  # 空闲会话时间
  #idle_session_timeout=600
  # 数据连接超时时间
  #data_connection_timeout=120
  # 隔离的安全用户
  #nopriv_user=ftpsecure
  # 开启异步数据线程
  #async_abor_enable=YES
  # 开启ASCII协议上传
  ascii_upload_enable=YES
  # 开启ASCII协议下载
  ascii_download_enable=YES
  # 开启邮箱验证
  #deny_email_enable=YES
  # 拒绝的邮箱列表
  #banned_email_file=/etc/vsftpd/banned_emails
  
  # 是否允许直接获取子目录信息
  #ls_recurse_enable=YES
  # 监听IPv4
  listen=NO
  # 监听IPv6
  listen_ipv6=YES
  # 虚拟用户启用pam认证
  pam_service_name=vsftpd
  # 用户组管理
  userlist_enable=YES
  # 访问控制
  tcp_wrappers=YES
  
```  

### 开启ftp服务

```
	# 开机自启
    systemctl enable vsftpd
    # 启动ftp服务
    systemctl start vsftpd
```

####  创建ftp账户

```
	useradd sk
	passwd sk

```


###  防火墙设置

```
	# 设置21端口提供ftp服务
    firewall-cmd --permanent --add-port=21/tcp
    firewall-cmd --permanent --add-service=ftp
    
    # 重载防火墙模块
    firewall-cmd --reload
    
    # SELinux
    setsebool -P ftp_home_dir on
   
```
&nbsp;&nbsp;&nbsp;&nbsp;
ftp服务器设置完成!


### How to install ftp on MAC ?

```
   brew install inetutils
```


### 每日一言

* 虽然辛苦，我还是会选择那种滚烫的人生。

<br>

&nbsp;&nbsp;&nbsp;&nbsp;
转载请注明： [王龙的博客](http://wanglong.org.cn) >> [阅读原文](http://wanglong.org.cn/2017/09/FtpTutorial/)