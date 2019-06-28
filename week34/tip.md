## ARTS - Tip 补2019.2.27

## Ajax跨域

#### 背景

在对APP、服务端项目开发时候，由于APP使用了html5做页面，有一些ajax调用后台数据的需求，测试时候前后端部署一台机器上没问题，当两者分开部署时候，就产生了跨域问题。



#### 介绍：

+ 什么是AJAX跨域问题
+ 产生AJAX跨域问题的原因
+ 解决AJAX跨域的思路和方法

#### 什么是AJAX跨域

简单说就是前端调用后端服务接口时，如果服务接口不是同一个域，就会产生跨域问题。

#### AJAX跨域场景

* 前后端分离、服务化的开发模式

#### AJAX跨域原因

+ 浏览器限制： 浏览器安全校验限制
+ 跨域(协议、域名、端口任何一个不一样都会认为是跨域)
+ XHR请求

#### AJAX跨域问题解决思路

* 浏览器： 浏览器去掉跨域校验
* XHR： 不使用XHR，使用JSONP，有很多弊端（JsonP向Server提交URL的长度限制在8000字符，超过了则被浏览器拒绝，因此不采用。），无法满足现在开发要求
* 跨域：被调用方法支持跨域调用(指定参数)；调用方法修改跨域（基于代理）



#### 解决方法

后端要做的工作：

接口允许允许跨域请求：

```javascript
header('Access-Control-Allow-Origin:*');  //支持全域名访问，不安全，部署后需要限制为R.com

header('Access-Control-Allow-Methods:POST,GET,OPTIONS,DELETE'); //支持的http动作

header('Access-Control-Allow-Headers:x-requested-with,content-type');  //响应头 请按照自己需求添加。
```

前端发起跨域请求：

就是正常的$.ajax请求即可。



#### OPTION问题

正式跨域请求前，浏览器会根据需要发起一个PreFlight OPTION请求，用来让服务器返回允许的方法（如GET,POST），被跨域访问的来源(Origin),还有是否需要认证信息（Credentials）。

#### 三种场景：

1. 如果跨域的请求是Simple Request（简单请求 ），则不会触发“PreFlight”。

   Mozilla对于简单请求的要求是：

   以下三项必须都成立：
   1. 只能是Get、Head、Post方法

   2. 除了浏览器自己在Http头上加的信息（如Connection、User-Agent），开发者只能加这几个：Accept、Accept-Language、Content-Type、。。。。

   3. Content-Type只能取这几个值：

      * application/x-www-form-urlencoded`
      * `multipart/form-data`
      * `text/plain`

      

2. 其他会导致“PreFlight”的请求。条件基本上是简单请求的补集。

3. 如果是PreFlight request 并且是Redirect的, 浏览器直接拒绝