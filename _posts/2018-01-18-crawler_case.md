--- 
layout: post
comments: true
title: PHP爬虫的简单实现
date: 2018-01-18
tag: PHP
---

想到爬虫，想到的第一门语言就是`python`，`python`写爬虫太过普遍，因为其自身优点使其成为写爬虫最流行的语言，但其实无论是Java还是PHP都能够轻松实现爬虫。下面就用PHP实现一个小的爬取网页案例。

###  准备工作

* 安装curl 扩展库
* 安装symfony 的dom-crawler API （composer安装）
* 了解XPath语法


### 简单案例

```php
public function crawler(){
    $url = "https://www.baidu.com/";
    header("Content-Type:text/html;charset=UTF-8");
    // curl 构造请求
    $ch = curl_init($url);
    curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
    curl_setopt($ch, CURLOPT_HEADER, false);
    $result = curl_exec($ch);
    
    if ($result == false){
    	echo "curl error:".curl_error($ch);
    }
    // 关闭curl
    curl_close($ch);
    
    // Xpath 页面数据提取
    $data = [];
    $crawler = new Crawler($content);
    try {
    	$data['title'] = $crawler->filterXPath('//title')->text();
    	// 提取重复dom元素内容
    	$crawler->filterXPath('//a')->each(function (Crawler $node, $i) use (&$data)){
    	    $data['notice'][$i] = $node->filterXPath('//a')->text();
    	}
    } catch (\Exception $e){
    
    } 
    
   var_dump($data);
   exit();
}
 
```

了解更多爬虫相关，推荐读读网友[《我用爬虫一天时间"偷了"知乎一百万用户，只为证明PHP是世界上最好的语言》](https://www.aliyun.com/jiaocheng/450712.html)


###  每日一言

*  年少有为不自卑

<br>

转载请注明： [王龙的博客](http://www.wanglong.org.cn) >> [PHP爬虫的简单实现](http://www.wanglong.org.cn/2018/01/crawler_case/)