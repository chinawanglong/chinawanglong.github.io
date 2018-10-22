---
layout: post
comments: true
title: Docker搭建开发环境
date: 2017-11-05
tag: Docker
---

### Docker 搭建php+nginx+mysql 开发环境

本地目录： docker/nginx/conf
		docker/www

下载PHP镜像

```bash
### 搜索官方镜像
$ docker search php

# 下载镜像指定镜像
$ docker pull phpdockerio/php-fpm 

```

创建php-fpm容器，并把项目目录挂载到php-fpm容器中，映射9000端口。

```bash
$ docker -d -p 9000:9000 --name php-fpm -v /www:/www phpdockerio/php-fpm
```

tips: 创建之前，保证9000口端未被占用


下载mysql镜像

```bash
# 搜索镜像
$ docker search mysql
# 下载指定镜像，我下载的是mariadb
$ docker pull mariadb

```

创建nginx容器
```bash
$ docker run -d -p 80:80 --name nginx -v /docker/nginx/conf/: /etc/nginx/conf.d/ -v /docker/www/:/www/ nginx
```


下载nginx镜像

```bash
$ docker pull nginx
```

