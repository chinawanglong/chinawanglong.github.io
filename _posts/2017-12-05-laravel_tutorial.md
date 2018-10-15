---
layout:  post
comments: true
title: Laravel系列一之简介和安装
date: 2017-12-05
tag: PHP
---

Laravel 对于我来说是既熟悉又陌生，在刚开始学习PHP的时候，就接触过这个框架，也用过这个框架来做过两个小项目，不过后面随着工作的变更， 新公司不再使用Laravel框架，眨眼间时间已经两年了，两年的时间里没有结果过了，关于这个框架的许多用法都已经遗忘的差不多了，我现在是作为一个新手重新开始学习下这个框架。目前为止，`Laravel`框架是PHP领域最好的框架，对提高变成水平有很多帮助。


###  简介

* Bundle 是laravel 的扩展包形式或称呼。 laravel的扩展包仓库已经非常成熟，可以很容易的帮你把扩展包安装到你的应用。可以选择下载一个扩展包然后copy到bundle目录，或者通过`artisan`自行安装

* 应用逻辑可以在控制球中实现，也可以直接集成到路由声明中。 laravel的设计理念是：给开发者以最大的灵活性，既能常见非常小的网站也能构建大型应用

* 反向路由赋予你通过路由名称创建链接的能力。只需使用路由能力，laravel就会自动帮助创建正确的uri

* Resultful控制器是一项区分GET和POST请求逻辑的可选方式。比如在一个用户的登录逻辑中，你声明一个get_login()动作来处理获取登录页面的服务，同时也设定了一个post_login()动作来校验表单POST过来的数据，并且在验证之后，做出重新转向登录页面还是控制台的决定

* 自动加载类简化了类的加载工作，以后就可以不去维护自动加载配置表和必须的组建加载工作了。当想加载任何组件或者模型的时候，就可以立即使用，laravel或帮助自动加载

* 视图组装器本质上就是一段代码，这段代码会在视图加载时自动执行。最好的例子就是博客中随机文章推荐，"视图组装器"中包含了加载随机文章推荐的逻辑，这样，只需加载内容区域的视图就行了，其他的laravel会帮着你自动完成

* 反向控制容器提供了生成新对象、随时实例化对象、访问单例对象的便捷方式。方向控制意味着几乎不需特意去加载外部的库，就可以在代码中的任意位置这些对象，并且不需要忍受繁杂、冗余的代码结构

* 迁移就像版本控制工具，不过，它的管理的是数据库范式，并且直接集成在了laravel中。可以使用`artisan`命令行工具生成、进行迁移指令

* 单元测试是laravel中很重要的部分，laravel本身就包含数以百计的测试用例，以保证任何一处的修改不会影响其他部分功能的功能，这就是为什么laravel在业内被认为是最稳定的版本之一

* 自动分页功能避免了在你的业务逻辑中混入大量无关分页配置的代码。方便的是不需要记住当前页，只需要数据库中获取总的条目数量，然后使用limit/offset获取选定的数据，最后调用paginate方法，让laravel将各页链接输出到指定的视图

### 环境要求

* PHP>=7.1.3
* OpenSSL PHP Extension
* PDO PHP Extension
* Mbstring PHP Extension
* Tokenizer PHP Extension
* XML PHP Extension


### 安装

* composer 安装方式

```bash
$ composer create-project --prefer-dist laravel/laravel  project_name
```

*  laravel 方式

```bash
$ composer global require "laravel/installer"
# 利用laravel框架创建个新项目blog
$ laravel new blog  
```

###  配置

安装之后，配置服务器的`document/web` root 指向laravel框架的`public`目录，入口文件`index.php`就在该目录下。

laravel框架的所有配置文件都在`config`目录内，可以根据需要，进行适当的修改。配置`storage`和`bootstrap/cache`目录是可写，这些是缓存文件。


###  服务器配置

#### Apache

laravel 在`public`目录下有个`.htaccess`文件，这个文件配置apache对URL读写规则。

```bash
Options +FollowSymLinks
RewriteEngine On

RewriteCond %{REQUEST_FILENAME} !-d
RewriteCond %{REQUEST_FILENAME} !-f
RewriteRule ^ index.php [L]
```

####  Nginx

假如你使用的服务器软件是Nginx， `public/.htaccess`文件就不起作用了，在`nginx.conf`中增加如下配置

```bash
location / {
    try_files $uri $uri/ /index.php?$query_string;	
}
```

###  每日一言

* 不管你用什么方式活着，我们只有一个目的，别违心，以及别后悔，还有，去他的人言可畏

<br>

转载请注明： [王龙的博客](http://wanglong.org.cn) >> [Laravel系列一之简介和安装](http://wanglong.org.cn/2017/12/laravel_tutorial/)