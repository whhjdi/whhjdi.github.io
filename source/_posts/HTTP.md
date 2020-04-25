---
title: HTTP
date: 2018-05-18 10:55:04
tags: [JavaScript]
categories: ["其他"]
cover_img: https://ws3.sinaimg.cn/large/006tNbRwly1fxzf1w4n73j31hc0u07dn.jpg
---

多看阅读-[图解 HTTP](http://www.duokan.com/reader/www/app.html?id=a8c81c979f514cacb2e2435d85f6ba38)

> Web 使用一种名为 HTTP（HyperText Transfer Protocol，超文本传输协议）的协议作为规范，完成从客户端到服务器端等一系列运作流程。
> 通常使用的网络（包括互联网）是在 TCP/IP 协议族的基础上运作的。而 HTTP 属于它内部的一个子集。

## TCP/IP

TCP/IP 协议族按层次分别分为以下 4 层：应用层、传输层、网络层和数据链路层。

## URI/URL

与 URI（Uniform Resource Identifier,统一资源标识符）相比，我们更熟悉 URL（Uniform Resource Locator，统一资源定位符）。

<!--more-->

## 请求与响应

请求报文是由请求方法、请求 URI、协议版本、可选的请求首部字段和内容实体构成的。

```javascript
请求的格式
1 动词 路径 协议/版本
2 Key1: value1
2 Key2: value2
2 Key3: value3
2 Content-Type: application/x-www-form-urlencoded
2 Host: www.baidu.com
2 User-Agent: curl/7.54.0
3
4 要上传的数据
请求最多包含四部分，最少包含三部分。（也就是说第四部分可以为空）
第三部分永远都是一个回车（\n）
动词有 GET POST PUT PATCH DELETE HEAD OPTIONS 等
这里的路径包括「查询参数」，但不包括「锚点」
如果你没有写路径，那么路径默认为 /
第 2 部分中的 Content-Type 标注了第 4 部分的格式
```

接收到请求的服务器，会将请求内容的处理结果以响应的形式返回。
响应报文基本上由协议版本、状态码（表示请求成功或失败的数字代码）、用以解释状态码的原因短语、可选的响应首部字段以及实体主体构成。

```javascript
响应的格式
1 协议/版本号 状态码 状态解释
2 Key1: value1
2 Key2: value2
2 Content-Length: 17931
2 Content-Type: text/html
3
4 要下载的内容
```

## 状态码

```javascript
1xx(正在处理)
2xx(成功)
200 - OK
204 - No Content
206 - Partial Content
3xx(重定向)
301 - 永久重定向
302 - 临时重定向
303 - 应使用 GET
304 - Not Modified
307 - 临时重定向
4xx(请求错误)
400 - 请求报文中存在语法错误
401 - 请求要求身份验证
403 - Forbidden
404 - 请求的资源（网页等）不存在
5xx(服务器错误)
500 - 内部服务器错误
502 - 网关错误
503 - 服务不可用
504 - 网关超时
```

## HTTP 首部字段

根据实际用途分为以下四种，具有哪些就看书好了

1. 通用首部字段
2. 请求首部字段
3. 响应首部字段
4. 实体首部字段

## 缓存

1. Cache-Control(属于通用首部字段))
   max-age=30（优先级比 Expires 高）,首页不要设置缓存，其他的一般设置 10S
   可以把时间设置的很长（1 年，10 年等等），如果资源需要更新，换个 url(加查询参数或者随机数)

   ```javascript
   //单位是s
   Cache-Control: private, max-age=0, no-cache
   ```

2. Expires（属于实体首部字段）
   可以设置到期的具体时间

   ```javascript
   Expires: Wed, 04 Jul 2012 08:26:05 GMT
   ```

3. ETag / If-None-Match
   响应首部字段 ETag 能告知客户端实体标识。它是一种可将资源以字符串形式做唯一性标识的方式。服务器会为每份资源分配对应的 ETag 值。
   当再次请求这个资源时，会通过`If-None-Match：W/"xxxxxxx-xxx"`告诉服务器，服务器做一个对比，如果两个 ETag 相同就返回 304,否则就返回新资源

   ```javascript
   ETag: "82e22293907ce725faf67773957acd12";
   ```

4. Last-Modified / If-Modified-Since
   服务器在响应请求时，告诉浏览器资源的最后修改时间
   当再次请求相同资源时，会通过 `If-Modified-Since:Tue 24 Jan 2018 03:22:21 GMT`告诉服务器最后的修改时间，服务器做一个对比，如果服务器上的最后修改时间大于 If-Modified-Since 的，那么久说明修改了，就返回新的资源，否则返回 304

## Cookie

```javascript
服务器通过Set-Cookie头给客户端一串字符串
客户端每次访问相同域名的网页时，必须带上这段字符串
客户端要在一段时间内保存这个Cookie
Cookie 默认在用户关闭页面后就失效，后台代码可以任意设置 Cookie 的过期时间
大小大概在4KB之内
```

作用：
Cookie 主要用来分辨两个请求是否来自同一个浏览器，以及用来保存一些状态信息

使用场合：
对话（session）管理：保存登录、购物车等需要记录的信息。
个性化：保存用户的偏好，比如网页的字体大小、背景色等等。
追踪：记录和分析用户行为。

Cookie 的属性
Expires（指定一个具体的到期时间），
Max-Age（从现在开始 Cookie 存在的秒数），
Domain，Path，
Secure（用于 HTTPS），
HttpOnly（JS 读不到），
document.cookie（读写当前网页的 Cookie）

## Session

```javascript
服务器通过cookie给用户一个sessionId（随机数）
客户端访问服务器时，服务器读取SessionId
服务器里有一小块内存(hash)，保存了所有Session
通过SessionId我们可以得到对应用户的隐私信息，如ID、email等
这块内存（hash）就是服务器上的所有Session
```

## LocalStorage

```javascript
LocalStorage和HTTP无关
HTTP不会带上LocalStorage的值
只有相同域名的页面才能相互读取 LocalStorage
每个域名LOcalStorage 存储量是有限的
常用来记录有没有提示过用户
LocalStorage永久有效，除非用户手动清除
```

## SessionStorage

```javascript
SessionStorage和HTTP无关
HTTP不会带上SessionStorage的值
只有相同域名的页面才能相互读取 SessionStorage
每个域名SessionStorage 存储量是有限的
常用来记录有没有提示过用户
关闭浏览器后就失效
```

## 其他

Session 和 Cookie 的区别：
一般来说，Session 时基于 Cookie 实现的
LocalStorage 和 Cookie 的区别
存储的大小不同
LocalStorage 不会被传到服务器上

LocalStorage 和 SessionStorage 的区别
SessionStorage 关闭页面后（会话结束）就失效

GET 和 POST 的区别是什么？
参数。GET 的参数放在 url 的查询参数里，POST 的参数（数据）放在请求消息体里。
安全（扯淡）。GET 没有 POST 安全（都不安全）
GET 的参数（url 查询参数）有长度限制，一般是 1024 个字符。POST 的参数（数据）没有长度限制（扯淡，4~10Mb 限制）
包。GET 请求只需要发一个包，POST 请求需要发两个以上包（因为 POST 有消息体）
GET 用来读数据，POST 用来写数据，POST 不幂等（幂等的意思就是不管发多少次请求，结果都一样。）
