---
layout: post
comments: true
title: Post/Redirect/Get pattern | PRG 模式
date: 2018-05-21
tag: Javascript
---

Post/Redirect/Get 是一种开发设计模式，用于防止表单的重复提交。


### 默认情况图示：

默认情况，提交Post请求到服务器后，如果直接刷新到浏览器，会重新在提交一次Post请求。

![默认情况图示](/images/posts/common/post_require.png)


### PRG设计模式

诸如下面的一些用户行为，都是表单重复提交可能的场景：

* 重新载入或刷新页面;
* 点击浏览器的后退按钮,接着点击前进按钮;
* 点击浏览器的后退按钮退回到前一页,再点一回提交按钮;

解决问题的一种思路就是将用户点击提交按钮从而发出POST请求, 页面提交跳转和显示结果页面这三个行为分离开. PRG正是遵循这种思路, Client用POST方法请求Server响应数据变更, Server用Redirect方法将response指定到另一个URL上的结果页面, Client所有对页面显示的请求都用GET方法告知Server. 解决方案如图2所示. 这样, "后退再前进"或刷新页面的行为都发出的是GET请求, 从而不会对server产生任何数据更改的影响, double submit problem得以解决.


![PRG设计模式](/images/posts/common/prg_require.png)



#### 完善网页收藏功能
Post/Redirect/Get 方式除了能防止 Post 请求的重复提交外，还可以完善网页收藏功能。把 Post 请求直接返回的网页收藏到书签是无效的，因为这个网页的重现依赖于 Post 请求以及当时提前的数据。采用 PRG 方式，用户正常情况下收藏到的是重定向后 GET 方法返回的网页，这样使得收藏有效了。




值得注意的是，PRG设计模式并不能使用所有的表单重复提交情况，如下几种情况

* 如果用户返回表单页面，重新提交表单的情况
*  用户在服务器端响应到达之前，多次点击提交按钮的时候。(可通过JavaScript控制提交按钮点击次数)

* 由于服务器响应缓慢，用户刷新提交POST请求造成的多次POST请求
*  恶意用户避开客户端预防多次提交手段，进行重复提交请求

###  其他防止重复提交的方法

####  在数据库添加唯一字段

在数据表建立的时候在ID字段添加主键约束，账号，名称的信息添加唯一性约束。确保数据库只能添加一条数据

####  用js添加禁用

当用户提交表单之后，可以使用js将提交按钮隐藏（disable属性）， 防止用户多次点击按钮提交数据。

tips： 如果客户端禁用js，则此方法无效


####  使用session设置令牌

产生页面时，服务器为每次产生的Form分配唯一的随机标示号，并且在form的一个隐藏字段中设置这个标示号，同时在当前用户的session中保存这个标示号。当提交表单时，服务器比较hidden和session中的标示号是否相同，相同则继续，处理完后清空session，否则服务器忽略请求。

tips： 恶意用户可利用这个一性质，不断重复访问页面，以致session中保存的标示号不断增多，最终严重消耗服务器内存。 可以采用在session中记录用户发帖的时间，然后通过一个时间间隔来限制用户连续发帖的数量来解决这一问题。

### 每日一言

* 成功与失败之间，也许就隔着"再试一次"的距离

<br>

转载请注明： [王龙的博客](http://www.wanglong.org.cn) >> [Post/Redirect/Get pattern | PRG 模式](http://www.wanglong.org.cn/2018/05/confirm_form_resubmission/)