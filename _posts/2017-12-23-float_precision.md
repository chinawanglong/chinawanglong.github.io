---
layout: post
comments: true
title: 浮点数计算的精度问题
date: 2017-12-23
tag: Javascript
---

0.1+0.2 = 0.3; 这是小孩子都知道的算式，我们应认为理所当然的是这样。但是计算机可不这样认为。 在编程语言中，不加任何修饰的去判断`0.1+0.2 == 0.3`, 结果都是`false`。 这个并不是编程中的bug， 而是由计算机的本质决定的。

###  精度缺失的原因

计算机的所有数据都是二进制存储，直白一句话就是`计算机的二进制实现和位数限制有些数无法有限表示`。 在计算机中，01.+0.2的真实结果是0.30000000000000004。

0.1的二进制表示： 
0.1 => 0.0001 1001 1001 1001…（无限循环）

0.2的二进制表示：
0.2 => 0.0011 0011 0011 0011…（无限循环）

所以单独靠计算机是得不到正确的四则运算结果，必须采取一定的手段解决，不然类似财务数据根本实现不了。


###  Js处理精度的一般处理

编程语言中处理计算精度问题一般的思路是，把需要计算的数字乘以10的n次幂，换算成计算机能够精确识别的整数，然后再除以10的n次幂。

```javascript
// a + b 浮点数的精度计算
function addNum(a,b){
    var r1,r2,m;
    try {
       	r1 = a.toString().split('.')[1].length; 
    } catch (e){
        r1 = 0;
    }
    try {
        r2 = b.toString().split('.')[1].length;
    } catch (e) {
      	r2 = 0;
    }
    // 10 的n次幂
    m = Math.pow(10, Math.max(r1,r2));
    
    return (a*m + b*m)/m ;
}
```

### Js重写toFixed方法

前端的很多数据处理，比如保留几位小数，四舍五入等问题，会用到toFixed()函数。

```javascript
numObj.toFixed([fractionDigits]);
```

参数：

NumberObj: 一个number对象

franctionDigits: 可选项，小数点后的数字位数，其值必须在0～20之间。如果没有fractionDigits参数，或者该参数为undefined， toFixed方法假定该值为0。

但是该方法在四舍五入的时候并不总正确，有些浏览器不能正确的解析。
```html
<input onclick="alert(0.097.toFixed(2))" type="button" value="显示0.097.toFixed(2)">
```
在ie7浏览器下按钮会显示0.00，而不是预期的0.01

那怎么办呢？ 可以对toFixed做简单的处理。重写

```javascript
Number.prototype.toFixed = function(n){
    if ( n<0 || n>20 ){
        throw new RangeError('toFixed() digits argument must be between 0 and 20');
    }
    const number = this;
    if (isNaN(number) || number >= Math.pow(10, 21)){
        return number.toString();
    }
    if (typeof n === 'undefined' || n === 0){
        return (Math.round(number)).toString();
    }
    let result = number.toString();
    const arr = result.split('.');
    // 整数的情况
    if (arr.length < 2){
       result += '.';
       for (let i = 0; i < n; i += 1){
           result += '0';
       }
       return result;
    }
    
    const integer = arr[0];
    const decimal = arr[1];
    if (decimal.length === n){
        return result;
    }
    if (decimal.length < n){
        for (let i = 0; i < n - decimal.length; i += 1){
            result += '0';
        }
        return result;
    }
    result = integer + '.' + decimal.substr(0, n);
    const last = decimal.substr(n, 1);
    
    // 四舍五入，转为整数在处理，避免浮点精度的损失
    if (parseInt(last, 10) >= 5){
        const x = Math.pow(10, n);
        result = (Math.round((parseFloat(result) * x)) + 1) / x;
        result = result.toFixed(n);
    }
    
    return result;
    
    // return (parseInt(this * Math.pow(10, s) + 0.5) / Math.pow(10, s)).toString();
}
```

上面按钮在ie7下就能正确显示为0.01了。
 


###  PHP的精度处理

类似于javascript，PHP也是弱类型语言，但是PHP封装了方法处理浮点数计算的精度问题。

PHP为任意精度数学计算提供了二进制计算器（Binary Calculator）,它支持任意大小和精度的数字

* bcadd  -- 加法
* bcsub  --减法
* bccomp  --比较
* bcdiv  -- 相除
* bcmod  -- 求余数
* bcmul  --乘法
* bcpow  --次方
* bcpowmod  --先次方然后求余数
* bcscale  --给所有函数设置小数位精度
* bcsqrt  --求平方根

 

###  每日一言

* 人生中多数的不幸并非由厄运造成的，而是笨拙、倦怠和粗俗导致的  --《草叶集》 

转载请注明： [王龙的博客](http://www.wanglong.org.cn)  >>  [浮点数计算的精度问题](http://www.wanglong.org.cn/2017/12/float_precision/)