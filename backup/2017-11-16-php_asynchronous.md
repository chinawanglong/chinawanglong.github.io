---
layout: post
comments: true
title: PHP异步请求实现
date:  2017-11-16
tag: PHP
---

业务场景描述： 调用多个API，每个API的运算时间不同，等待每一个返回结果再执行PHP脚本，等待时间就是所调用API运算时间之和，花费时间太长。

解决办法： 使用PHP异步请求，等待的时间就是运算时间最长的API返回结果的时间





###  每日一言

*

<br>


转载请注明： [王龙的博客](http://wanglong.org.cn/) >> [PHP的异步请求实现](http://wanglong.org.cn/2018/11/php_asynchronous/)