---
layout:  post
comment: true
title:  PHP面试题集
date:  2017-11-12
tag: PHP
---

> 请列举PHP中一些正则函数（至少2个）

preg_match(), preg_match_all(), preg_replace(), preg_replace_callback()

> UTC是什么

通用协调时间， 全球标准时间

> 解释下php://input叫什么

php输入流

> 请说明在php.ini中safe_mode开启之后对于PHP系统函数的影响

safe_mode是提供一个基本安全的共享环境。在一个多用户共享的phpweb服务器上，开启了safe_mode模式，跟操作文件系统的有关函数会收到影响。比如：move_uploaded_file, mkdir, link, unlink等。同时， 一些php扩展函数也会收到限制，不能在程序中直接加载扩展，只能在php.ini中加载。

> php5中魔术方法有那几个

魔术方法：

__construct() 实例化对象时调用

__destruct() 删除一个对象或者对象操作终止时调用

__call() 调用对象不存在方法时调用

__get() 调用对象不存在的属性时调用

__set() 设置对象不存在的属性时调用

__toString() 打印一个对象时调用

__clone()  克隆一个对象时调用

__sleep()  serialize之前被调用，如对象比较大，想做一些删除在序列化，可以考虑使用

__wakeup() unserialize之前被调用，做些对象的初始化

 
 > 列举常见的两种mysql存储引擎，及他们的区别
 
 MyISAM, InnoDB
 
 区别：
 
 * MyISAM 是非事物安全型的，而InnoDB是事物安全的
 * MyISAM锁的粒度是表级，而InnoDB支持行级锁定
 * MyISAM支持全文类型索引，而InnoDB不支持全文类型索引
 * MyISAM相对简单，在效率上要优于InnoDB，小型应用可以考虑使用MyISAM
 * MyISAM表是保存称文件的形式，在跨平台的数据转移中使用MyISAM存储会少去不少麻烦
 * InnoDB比MyISAM更安全，可以在保证数据不会丢失的情况下，切换非事物表到事物表
 
> PHP中防Sql注入，防止XSS攻击常用的解决方案

防Sql注入：使用预处理语句和参数化的查询

防止XSS： 
* 直接过滤所有的JavaScript脚本
* 系统的扩展函数库提供了XSS安全过滤的remove_xss方法
* 转义Html元字符， 使用htmlentities、htmlspecialchars等函数


> 求两个日期的差数，例如2017-01-01～2017-05-06的日期差


```php
function diffDate($str1, $str2){
    $temp = explode('-', $str1);
    $time1 = mktime(0,0,0, $temp[1],$temp[2],$temp[0]);
    $temp = explode('-', $str2);
    $time2 = mktime(0,0,0, $temp[1], $temp[2], $temp[0]);
    return ($time2-$time1)/86400;
}
```

> 有一个网页地址，比如PHP开发资源主页：http://www.phpres.com/index.html,如何得到它的内容


```php
// method 1: 
$readContent = fopen("http://www.phpres.com/index.html", "rb);
$contents = stream_get_contents($readContent);
fclose($readContent);
echo $contents;

// method 2:
echo file_get_contents("http://www.phpres.com/index.html");
```

> session 和 cookie 的区别是什么， 请从协议、产生的原因与作用说明


* http无状态协议，不能区分用户是否从同一个网站上来，同一个用户请求不同的页面不能看作是同一个用户
* session存储在服务器端， cookie存储在客户端，session比较安全，cookie用某些手段可以修改，不安全。session的缺点：保存在服务器端，每次读取都从服务器进行读取，对服务器资源有消耗。

 
> HTTP状态码：302、403、500、200、502含义
 
 
 一二三四五原则：一、消息系列，二、成功系列， 三、重定向系列，四、请求错误系列， 五、服务器端错误系列
 
> include与require的区别
 
 
 * include()在执行文件时每次都要进行读取和评估,require()文件只处理一次，
 * require()通常放在PHP脚本程序的最前面，include()的使用和require()不一样，一般放在流程控制的处理区段，PHP脚本读到include()语句时，才将包含的文件读进来，可以把程序执行的流程简单化
 * require()包含的文件失败，程序停止执行，给出错误； include()执行失败，PHP脚本依然执行，返回一条警告
 
> 单引号和双引号
 
 * 单引号内部的变量不会执行， 双引号会执行
 * 单引号解析速度比双引号快
 * 单引号只能解析部分特殊字符，双引号可以解析所有特殊字符
 


### 每日一言
 
 * 清醒时做事，糊涂时读书，独处时思考，难过时睡觉，世上本无事，庸人自扰之。
 
 
 <br>
 
 转载请注明： [王龙的博客](http://wanglong.org.cn) >> [点击阅读原文](http://wanglong.org.cn/2017/11/php_job_interview/)