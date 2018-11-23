---
layout: post
comment: true
title: Laravel的登录验证
date: 2018-11-23
tag: Laravel
---

### Laravel Auth

laravel 原生的auth封装的很好，按照文档操作可以完成基本的登录验证，但是默认以`User`为验证基础，换张表以及不同页面登录就需要做些修改

* 用户在不登录的情况下自动跳转在登录页面

```php
<?
// routes/web.php

Route::group(['middleware' => ['auth']);
```

* 原生的登录认证写法

```php
<?
// LoginController.php
namespace App\Http\Controller;

use Illuminate\Http\Request;
use Illuminate\Support\Facades\Validator;
use Illuminate\Support\Facades\Auth;

public function login(Request $request){
	// 校验form
    $validator = Validator::make($request->all(), [
    	'user' => 'required|min:2',
    	'password' => 'required|min:6|max:18'    
    ]);
    if ($validator->fails()){
        return back()->withErrors($validator->errors())->withInput($request->only('password'));
    }
    // 验证
    if (Auth::attempt(['user'=>$request->input('user',''), 'password' => $request->input('password', '')])){
          return redirect()->intended('/index');
    }
    return back()->withErrors(['账号或密码错误'])->withInput($request->only('user'));
    
}

```

* tips: 验证的时候，密码不需要加密，框架会将密码自动hash加密与库中的密码比较

### 修改Laravel默认登录验证

在原生的验证方法中，读取的是`config/auth.php`中的配置

```php
<?
// config/auth.php
return [
    'defaults' => [
    	'guard' => 'web',
    	'passwords' => 'users'    
    ],
    
    'provider' => [
        'web' => [
            'driver' => 'session',
            'provider' => 'users'
        ],
        'api' => [
            'driver' => 'token',
            'provider' => 'users'
	] 
    ],
    
    'providers' => [
    	'driver' => 'eloquent',
    	'model' => App\User::class    
    ],
    
    'passwords' => [
    	'users' => [
    	    'provider' => 'users',
    	    'table' => 'password_reset',
    	    'expire' => 60	    
	],    
    ]
];
```

从上面的配置可以看到，看守器默认的是`web`配置，`web`对应的表是`User`,框架验证的时候会自动匹配`User`中的用户数据。即`Auth::attempt()`的完整是`Auth::guard('web')->attempt`.

接下来我们用`Admin`表替代`User`表进行后台用户的登录验证

```php
 <?
 // config/auth.php
 return [
     'defaults' => [
     	'guard' => 'web',
     	'passwords' => 'users'    
     ],
     
     'provider' => [
         'web' => [
             'driver' => 'session',
             'provider' => 'users'
         ],
         'web_admin' => [
             'driver' => 'session',
             'provider' => 'admins'        
	 ],
         'api' => [
             'driver' => 'token',
             'provider' => 'users'
 	] 
     ],
     
     'providers' => [
         'users' => [
  	      'driver' => 'eloquent',
              'model' => App\User::class     	
	],
	  'admins' => [
		'driver' => 'eloquent',
		'model' => App\Admin::class      
	]
     	    
     ],
     
     'passwords' => [
     	'users' => [
     	    'provider' => 'users',
     	    'table' => 'password_reset',
     	    'expire' => 60	    
 	],
 	'admins' => [
         	    'provider' => 'admins',
         	    'table' => 'admin',
         	    'expire' => 60	    
     	],    
     ]
 ];
```

在登录验证的时候略做修改

```php
<?php
// routes/web.php

Route::group(['middleware' => ['auth:web_admin']);
?>


<?php
// LoginController.php
namespace App\Http\Controller;

use Illuminate\Http\Request;
use Illuminate\Support\Facades\Validator;
use Illuminate\Support\Facades\Auth;

public function login(Request $request){
	// 校验form
    $validator = Validator::make($request->all(), [
    	'user' => 'required|min:2',
    	'password' => 'required|min:6|max:18'    
    ]);
    if ($validator->fails()){
        return back()->withErrors($validator->errors())->withInput($request->only('password'));
    }
    // 验证
    if (Auth::guard('web_admin')->attempt(['user'=>$request->input('user',''), 'password' => $request->input('password', '')])){
          return redirect()->intended('/index');
    }
    return back()->withErrors(['账号或密码错误'])->withInput($request->only('user'));
    
}
```

### 每日一言

* 人生最痛苦的时刻，也是我们展现最多力量和勇气的时刻


转载请注明： [王龙的博客](http://wanglong.org.cn) >> [Laravel Auth](http://wanglong.org.cn/2018/11/laravel_Auth/)