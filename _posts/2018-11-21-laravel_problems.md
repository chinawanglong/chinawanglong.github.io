---
layout: post
title: Laravel 框架中碰到的问题
comments: true
date: 2017-11-21
tag: Laravel
---

### 1. 419 Sorry, your session has expired. Please refresh and try again

form 以post方式提交后，出现 "419 Sorry, your session has expired. Please refresh and try again"， 这是因为没有加上`CSRF`验证。在form表单内加入 `@CSRF`即解决问题


### 2. undefined param "errors"

```php
@if ($errors->any())
    @foreach($errors->all() as $error)
	<li>{{ $error }}</li>    
    @endforeach
@endif

```
这是因为最新版本的laravel 已经把`$errors`参数移除全局变量，移到`web`中间件，在route中
```php
Route::group(['middleware' => ['web']]);
```
