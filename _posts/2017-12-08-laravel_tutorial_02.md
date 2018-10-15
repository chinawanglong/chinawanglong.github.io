---
layout: post
comments: true
title: Laravel系列之路由、控制器
date: 2017-12-08
tag: PHP
---

###  路由

什么是路由？ 简单点说就是路由是根据不同的URL地址展示不同的内容或页面。直面描述了在编程中路由的作用，追述起来，路由的概念出现在因特网协议栈中的网络层中。

laravel 的路由文件在`route/web.php`。我们访问项目的时候，请求首先到达`public/index.php`入口文件，经过`route/web.php`的路由分发，访问不同的控制器

有些框架的路由是自动绑定的，创建了控制器，路由也自动就有。laravel的每一个路由是需要需要手动定义的，也许会觉得这样很繁琐，但是手动定义这种解耦的方式有它的好处，以后重构项目的路由就简单方便的多。

![](/images/posts/laravel/route.png)

`Route`是一个类，访问类的静态方法是用`::`的形式，那么`get`就是Route类的一个静态方法。`get`静态方法可以传2个参数，第一个参数现在是`/`，第二个参数是一个闭包函数`function`，在这个闭包函数中return返回的东西就是我们请求到的内容。

那么laravel的路由规则总结如下：

* `Route::`后面可以跟一个请求方法，就是网络中的请求方式，常用的是`get`和`post`， 其他的如： `post`,`put`,`patch`,`delete`,`options`

* `view`函数可以直接定位到`resource/views`目录
*  程序运行之后，`routes`目录下的文件都会被加载，`web.php`被默认为系统的配置文件  


### 控制器

路由可以分发请求，路由可以引入html页面，看样在`route/web.php`中可以搞定一切，但是如果把业务逻辑都写入到路由中，那路由将庞大的难以维护。于是控制器就有了很明显的存在价值，把业务逻辑写在控制器中，路由只负责转发请求到指定的控制器即可。

laravel 是非常出色的PHP现代话框架，利用命令行就可以自动创建控制器。`artisan`是laravel的命令行接口，所以命令行开头是`php artisan`，比如生成一个控制器文件的命令如下

```bash
php artisan make:controller HelloController
```

如果执行没有问题，那么会生成一个`app/Http/Controllers/HelloController`文件。

打开之后

![](/images/posts/laravel/controller.png)


不但自动创建了文件，而且还定义了命令空间，继承好了父级控制器，直接写增删改查就可以了。

当然，还可以更进一步

```bash
php artisan make:controller IndexController --resource
```

打开之后

![](/images/posts/laravel/controller_01.png)

不仅是增删改查的方法都定义好了，连注释都写好了，这就是按照`RESTful`规范生成的格式。



### 路由关联控制器

路由和控制器都创建好了，下面就是怎么使两者关联起来



###  每日一言

* 在这个世界上，连专属于自己的姓名都会有重复的，就不难看出"做自己"是一件多么不容易的事。 ——《半山文集》

<br>

转载请注明： [王龙的博客](http://www.wanglong.org.cn)  >>  [Laravel系列之路由、控制器](http://www.wanglong.org.cn/2017/12/laravel_tutorial_02/) 