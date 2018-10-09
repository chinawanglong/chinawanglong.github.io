---
layout: post
comments: true
title:  PHP中定时任务的实现
date: 2018-03-12
tag: PHP
---

定时任务对于网站来说，是一个比较重要的功能，比如定时发布文档，定时清理垃圾信息，定时copy数据等。现在的web网站大多使用PHP开发，但是对于PHP来说，没有Java和.Net的AppServer的概念，而http协议是一个无状态的协议，PHP只能被用户触发，被调用，调用后会自动退出内存，没有常驻内存。


> 简单直接不顾后果型

```php
<?php 
    ignore_user_abort(); // 关闭浏览器，PHP脚本也可以继续运行
    set_time_limit(0); // 通过设定set_time_limit(0)可以让程序无限制的执行
    ini_set('memory_limit','512M'); // 内存限制
    $interval = 60*30; // 每隔半小时运行
    do {
        sleep($interval);
    }while(true);
?>
```

缺点： 启动之后， 便无法控制，除非终止PHP，真实环境中不要采用这种方式


> 简单可控型

```php
// config.php
<?php 
   return 1;
?>

// cron.php
<?php
   ignore_user_abort();	// 关闭浏览器，PHP脚本继续执行
   set_time_limit(0); // 通过set_time_limit(0)让脚本无限制执行
   $interval = 60*30; 
   do {
       $run = include 'config.php';
       if (!$run){
           die('process abort');
       }
       sleep($interval);
   }while(true);
  
?>

```

通过改变config.php的return, 来实现对脚本的控制， 一个可行的办法是config.php文件和某个特殊表单交互， 通过HTML页面设置一些变量来配置。

缺点： 占系统资源，长时间运行，会有一些意想不到的隐患，比如内存管理方面


> 简单改进型

```php
<?php
    $time = 15;
    $url = "http://".$_SERVER['HTTP_HOST'].$_SERVER['REQUEST_URI'];
    /**
    * function
    */
    sleep($time);
    file_get_contents($url);
?>
```

php脚本sleep一段时间之后通过访问自身的方式继续执行，这样就能保证每个PHP脚本执行时间不会太长，也就不受`time_out`的限制

因为每一次循环PHP文件都是独立执行的，避免了`time_out`的限制，但是最好和上边一样，加上控制代码config.php，以便能够终止进程


> Linux服务器上使用CronTab定时执行php

在Linux中，使用命令行，用CronTab来定时任务，是绝佳的选择，而且也是效率最高的选择

```bash
# 远程登录服务器, 键入以下命令
$ crontab -e
```
之后就会打开一个文件， 并且是非编辑状态，进入编辑状态。这个文件的每一行就是一个定时任务，新建一行，就是新建一条定时任务，当然是由书写格式

```bash
00**** lynx -dump https://www.yourdomain.com/script.php
```
 00****就是指当前时间的分钟数为00时，执行该定时任务，时间部分由5个时间参数组成，分别是`分 时 日 月 周` 。 
 
 第一列表示分钟1~59,每分钟用 */1表示，/n表示每n分钟， 例如 */8就是每8分钟的意思，下面也是类推
 
 第二列表示小时1～23（0表示0点）
 
 第三列表示日期1～31
 
 第四列表示月份1～12
 
 第五列表示星期0～6（0表示星期天）
 
`lynx -dump https://www.yourdamin.com/script.php` 通过`lynx`访问指定脚本。 我们在使用中主要使用`lynx` 、`curl`、`wget`来实现对url的访问，而如果要提高效率，直接用PHP去执行本地PHP脚本是最佳选择

```bash
00 */2 * * * /usr/local/bin/php /home/www/script.php
```

这条语句就可以在每2小时的0分钟，通过Linux内部PHP环境执行script.php，这里直接通过服务器环境来执行，效率高很多。

直接保存编辑的文件，定时任务设定完毕


> windows 上使用bat定时执行php

windows上有Linux上有一个类似的cmd和bat文件，bat文件类似shell文件，执行这个bat文件，相当于依次执行里面的命令（当然，还可以通过逻辑来实现编程）， 所以，我们可以利用bat命令文件在Windows服务器上面实现PHP定时任务。实际上Windows上的定时任务，原理和Linux是一样的，只不过方法和途径不同。
创建一个cron.bat文件  
```bash
D:\php\php.exe -q D:\website\test.php
```
这句话的意思就是，使用php.exe去执行test.php脚本，和Crontab一样

接下来就是设置定时任务来运行cron.bat.依次打开"开始-》控制面板-》任务计划-》添加任务计划"，在打开的界面中设置定时任务的时间、密码，通过选择，把cron.bat挂载进去。确定，这样一个定时任务就建立好了，在这个定时任务上右键，运行，这个定时任务就开始执行了，到点时，就会运行cron.bat处理，cron.bat再去执行php。



### 每日一言

* 山穷水复疑无路，柳暗花明又一村。

<br>

转载请注明：  [王龙的博客](http://www.wanglong.org.cn) >> [PHP中定时任务的实现](http://www.wanglong.org.cn/2018/03/php_timed_task.md/)