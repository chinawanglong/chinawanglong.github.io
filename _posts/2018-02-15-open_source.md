---
layout: post
comments: true
title: 开源项目应该怎么写
date: 2018-02-15
tag: 博客
---

技术的成长，离不开开源项目这一条路， 但是怎么规范化的去写开源项目？ 规范化的项目结构很重要， 能让别人清楚的看到项目的相关情况，下面就介绍下开源项目应该注意的几点问题。

本文以jslib-base为例

> jslib-base 最好用的js第三方脚手架，赋能js第三方开源，让开发一个js库更简单，更专业


### 文档

所谓代码未动， 文档先行，文档对一个项目非常重要，一个项目的文档包括

* READNE.md
* TODO.md
* CHANGELOG.md
* LICENSE
* doc

#### READNE.md

READNE是一个项目的门面，应该简单明了的呈现用户最关心的问题，一个开源库的用户包括使用者和贡献者，所以一个文档应该包括项目简介，使用者指南，贡献者指南三部分

项目简介应该简单介绍项目功能，使用场景，兼容性的相关知识，这里重点介绍下徽章，相信大家都见过别人项目中的徽章

![](/images/posts/codeSource/jslib-base.webp)

徽章通过更直观的方式，将更多的信息呈现出来，还能提高颜值


#### TODO.md

TODO应该记录项目的未来计划，这对于贡献者和使用者都有很重要的意义，下面是TODO的例子

> -[ x ] 已完成
>
> -[  ] 未完成


#### CHANGELOG.md

CHANGELOG记录项目的变更日志，对项目使用者非常重要，特别是在升级使用版本时，CHANGELOG需要记录项目的版本，发版时间和版本变更记录

```bash
## 0.1.0 / 2018-10-6
- 新增XX功能
- 删除XX功能
- 更改XX功能 
```

#### LICENSE

开源项目必须要选择一个协议，因为没有协议的项目是没有人敢使用的，关于不同协议的区别可以看下面这张图（来自阮老师博客），

![](/images/posts/codeSource/gpl.webp)


#### doc

开源项目还应该提供详细的使用文档，一份详细文档的每个函数介绍都应该包括如下信息：

```bash
函数简单介绍
函数详细介绍
函数参数和返回值（要遵守下面的例子的规则）
- param {string} name1 name1描述
- return {string} 返回值描述
举个例子（要包含代码用例）
// 代码
特殊说明，比如特殊情况下会报错等
```

最后再送给大家一句话，开源一个项目，重在开始，贵在坚持

推荐： [手把手教你如何构建一个优秀的开源项目](https://laravel-china.org/articles/5265/how-to-build-an-open-source-project-that-breaks-thousands-of-star)

### 每日一言

* 重在开始，贵在坚持

转载请注明： [王龙的博客](http://wanglong.org.cn) >> [开源项目应该怎么写](http://wanglong.org.cn/2018/02/open_source/)