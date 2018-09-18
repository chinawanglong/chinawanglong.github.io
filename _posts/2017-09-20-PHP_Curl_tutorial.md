---
layout: post
comments: true
title: PHP curl详解
date: 2017-09-20
tag: PHP
---

`curl` 是一个非常强大的开源库，支持很多协议，包括HTTP、FTP、TELNET等，我们使用它来发送HTTP请求。它带给我们的好处是可以通过灵活的选项设置不同的HTTP协议参数，并且支持HTTPS。`curl`可以根据URL前缀是`HTTP`还是`HTTPS`自动选择是否加密发送。


### 使用CURL发送请求的基本流程

使用CURL的PHP扩展完成一个HTTP请求的发送一般有以下几个步骤：

* 初始化连接句柄
* 设置CURL选项
* 执行并获取结果

下面的是代码片段是使用CURL发送HTTP请求的典型过程

```php
<?php
	// 1.初始化
	$ch = curl_init();
	// 2.设置选项
	curl_setopt($ch,CURLOPT_URL, "http://wanglong.org.cn/");
	curl_setopt($ch,CURLOPT_RETURNTRANSFER, 1);
	curl_setopt($ch,CURLOPT_HEADER, 0);
	// 3.执行并获取HTML文档内容
	$result = curl_exec($ch);
	if ($result === false){
	    echo curl_error($ch);
	}
	// 4.释放curl句柄
	curl_close($ch);
```
 
 上述代码中使用到了四个函数
 * curl_init()和curl_close()分别是初始化CURL连接和关闭CURL连接
 * curl_exec()执行CURL请求，如果没有错误发生，该函数返回是对应URL返回的数据，以字符串显示
 
 
 ### 每日一言
 * 那时，我还没有懂得人性是如何的矛盾，我不知道真诚中有多少做作，高贵中有多少卑鄙，或者，邪恶中有多少善良。如今我是充分懂得了，小气与大方、怨怼与仁慈、憎恨与热爱，是可以并存于同一颗心的。
 
 
 <br>
 
 &nbsp;&nbsp;&nbsp;&nbsp;
 转载请注明： [王龙的博客](http://wanglong.org.cn)  >>  [点击阅读原文](http://wanglong.org.cn/2017/09/PHP_Curl_tutorial/)