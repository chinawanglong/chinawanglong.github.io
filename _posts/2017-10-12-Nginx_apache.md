---
layout: post
comment: true
title: Nginx 和 Apache 的前世今生
date: 2017-10-12
tag: Linux
---

&nbsp;&nbsp;&nbsp;&nbsp;`Nginx` 、 `Apache` 是当今两大主流web服务器软件， 做开发的不可避免的要接触和了解这两个web服务器，今天我们就来聊聊`Nginx`、`Apache`的前世今生，以及在相关服务器上的部署。

### 目录

* [Nginx、Apache简介](#brief-introduction)
* [Nginx、Apache集成开发环境](#inviroment)
* [Nginx在centos上的部署](#nginx-setup-on-centos)
* [Apache在centos上的部署](#apache-setup-on-centos)
* [Nginx在Ubuntu上的部署](#nginx-setup-on-ubuntu)
* [Apache在Ubuntu上的部署](#apache-setup-on-ubuntu)
* [以Nginx为例，部署Nginx+PHP+Mariadb开发环境](#nginx+php+mariadb)

### <a name="brief-introduction"></a>Nginx、Apache简介

[Nginx官网](https://www.nginx.com) , Nginx是一个高性能的`HTTP`和`反向代理`服务器，也是一个`IMAP/POP3/SMTP`服务器。其特点如下：
> * 轻量级，占用更少的内存及资源
> * 抗并发，nginx处理请求是异步非阻塞的，在高并发下nginx能保持低资源消耗高性能
> * 高度模块化的设计，编写模块相对简单
> * 社区活跃

[Apache官网](https://httpd.apache.org), Apache是世界使用排名第一的web服务器。 它可以运行在几乎所有广泛使用的计算机平台上，由于其跨平台和安全性被广泛使用，是最流行的Web服务器端软件之一。apache相对于nginx的优点
> *  发展时间长，模块齐全
> *  rewrite，apache比nginx强大
> *  稳定性更高，bug少

没有绝对的优缺点，性能都是相对的。选用服务器软件时要根据具体的需求。比如追求性能，网站可能面临高并发，那么选用nginx更加合理。如果追求稳定性，减少运维成本，apache就会成为理想的选择。就目前而言，在非`动态请求`环境下，nginx基本成为首选。

####   静态资源和动态资源的概念

静态资源： 一般客户端发送请求到web服务器，web服务器从内存在取到相应的文件，返回给客户端，客户端解析并渲染显示出来。

动态资源： 一般客户端请求的动态资源，先将请求交于web容器，web容器连接数据库，数据库处理数据之后，将内容交给服务器，web服务器返回给客户端解析渲染处理。

静态资源和动态资源的区别：

*  静态资源一般都是设计好的html页面，而动态资源依靠设计好的程序来实现按照需求的动态响应
*  静态资源的交互性差，动态资源可以依据需求自由实现
*  在服务器的运行状态不同，静态资源不需要与数据库参与程序处理，动态可能需要多个数据库的参与运算



### <a name="inviroment"></a>Nginx、Apache集成开发环境

[LNMP](https://lnmp.org)

[XAMPP](https://www.apachefriends.org/index.html)

### <a name="nginx-setup-on-centos"></a>Nginx在centos部署

### <a name="apache-setup-on-centos"></a>Apache在centos部署

### <a name="nginx-setup-on-ubuntu"></a>Nginx在Ubuntu部署

### <a name="apache-setup-on-ubuntu"></a>Apache在Ubuntu部署

### <a name="nginx+php+mariadb"></a>以Nginx为例，部署Nginx+PHP+Mariadb开发环境



###  每日一言

* 桃李不言，下自成蹊

<br>

&nbsp;&nbsp;&nbsp;&nbsp;转载请注明：[王龙的博客](http://wanglong.org.cn) >> [点击阅读原文](http://wanglong.org.cn/2017/10/Nginx_apache/)

