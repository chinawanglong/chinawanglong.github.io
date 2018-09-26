---
layout: post
comments: true
title:  JS中var、let、const三者的区别 及 作用域
date: 2017-10-24
tag: Javascript
---

###  var 、let 、const

* var 定义的变量可修改，如果不初始化会输出undefined，不会报错


```javascript
var a = 1;  // var a; 也不报错
console.log(a);  // 1  ， 若未赋值： undefined

function change(){
    a = 2;
    console.log(a);   // 2
}   

change();
console.log(a);  // 2
```


* let 是块级作用域，函数内部使用let定义后，对函数外部无影响


```javascript
let b = 2;
console.log(b);  // 函数外定义输出：2

function change(){
    let b = 3; 
    console.log(b);  // 函数内定义输出：3
}

change();
console.log(b);  // 函数调用后，let定义不受函数内部定义影响，输出：2
```

*  const 定义的变量不能修改，而且必须初始化

```javascript
const c = 2;  // 正确， const c; 就是错误的
console.log(c);  // 输出：2
c = 3;   // const定义的变量不能修改，报错
```
 
###  JavaScript变量声明方式
 
#### JS变量声明提升(hoisting)

*  JavaScript 的变量声明具有hoisting机制，JavaScript引擎在执行的时候，会把所有变量的声明都提升到`当前作用域`的前面
```javascript
var v = "hello";
(funciton(){
    console.log(v);
    var v = "world";
})();
```

输出的结果并不是预想的`hello`, 而是`undefined`, 这说明了两个问题：

> 1. function 作用域的变量v遮盖了上层作用域变量v
> 2. 在function 作用域内，变量v的声明被提升了, 相当于


```javascript
 var v = "hello";
 (function(){
     var v ;  // 这就是hoisting机制的体现
     console.log(v);
     var v = "world";
 })();
```

#### 名字解析顺序

&nbsp;&nbsp;&nbsp;&nbsp;
JavaScript中一个名字（name）以四种方式进入作用域（scope），其优先顺序如下：
> * 语言内置： 所有的作用域中都有this和arguments 关键字
> * 形式参数： 函数的参数在函数作用域中都是有效的
> * 函数声明： function foo(){} 
> * 变量声明： var、let、const 


###  作用域与作用域链

* 函数作用域


```javascript
var scope = "global";
function foo(){
    console.log(scope); //  undefined
    var scope = "local";
    conosle.log(scope);  // local
}   
```


> tips ： JavaScript并没有块级作用域


```javascript
var name = "global";
if (true){
    var name = "local";
    console.log(name);   // local
}
```

如果JavaScript有块级作用域，if语句将创建局部变量name，并不会修改全局变量name, 可实际的输出结果高速我们，显然事实并不是如此。

*  变量作用域

```javascript
function t(flag){
    if (flag){
        s = 'ifscope';
        for(var i=0; i<2; i++){}
    }
    console.log(i);
}
t(true);
console.log(s);		// ifscope
```

> tips: 没有var声明的变量都是全局变量，而且是顶层对象的属性；当使用var声明一个变量时，创建的这个属性是不可配置的，也就是说无法通过delete运算符删除

* 作用域链

```javascript
name = "hello";
function t(){
    var name = "leilei";
    function s(){
        var name = "are you ok?";
        console.log(name);
    }
    function ss(){
        console.log(name);
    }
    s();
    ss();
}
t();

```
> 当执行s函数时，将创建函数s的执行环境（调用对象），并将该对象置于链表开头，然后将函数t的调用对象链接在之后，最后是全局对象。然后从链表开头寻找变量name，很明显name是"are you ok?"; 但执行ss()时，作用链是： ss()->t()->window, 所以name是"leilei";


*  with语句和try-catch语句的catch块


with 和catch语句主要是用来临时扩展作用域链，将语句中的对象添加到作用域链的头部。


```javascript
person = {name:'leilei', leight:'175', age:22, wife:{name:'hanmei',age:'20', height:'168'}};
with(person.wife){
    console.log(name);   // hanmei
}
```
> with 语句将person.wife添加到当前作用域链的头部，所以输出是hanmei,而不是leilei。 with语句结束后，作用域链恢复正常


###  每日一言

*  你的口渴止于一杯水，而非一片海。


<br>

&nbsp;&nbsp;&nbsp;&nbsp;
转载请注明： [王龙的博客](http://wanglong.org.cn) >> [点击阅读原文](http://wanglong.org.cn/2017/10/JS的var、let、const/)
  
 


