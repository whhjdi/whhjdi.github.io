---
title: AJAX
date: 2018-07-20 18:22:02
tags: [AJAX]
categories: ["其他"]
cover_img: https://ws1.sinaimg.cn/large/006tNbRwly1fxzeb1zo5uj31dr0rsx6p.jpg
---

AJAX 全称 Asynchronous Javascript And XML，即异步的 JavaScript 和 XML，AJAX 允许以任意形式发送请求并且以任意形式展示。
符合如下技术的就叫做 AJAX：使用 XMLHttpRequest 发送请求,服务器返回 XML 格式的字符串,浏览器解析 XML，并局部更新页面

<!--more-->

## 自己封装一个类似 jQuery 的 ajax

```javascript
//封装
window.jQuery.ajax= function({method,url,body,headers}){
    return new Promise(function(reslove,reject){
        let xhr = new XMLHttpRequest()
        xhr.open(method,url)
        for(let key in headers){
            let value = headers[key]
            xhr.setRequestHeader(key,value)
        }
        xhr.send(body)
        xhr.onreadystatechange=funciton(){
            if(xhr.readyState===4){
                if(xhr.status>=200&&xhr.status<300){
                    reslove.call(undefined,xhr.responseText)
                }else if(xhr.status>=400){
                    reject.call(undefined,xhr)
                }
            }
        }
    })
}
//使用
window.jQuery.ajax({
  method: 'post',
  url: '/xxx',
  body: 'a=1',
  headers:{
      'Content-Type': 'x-www-foorm-urlencoded'
  }}).then(
    (responseText)=>{console.log(responseText)},
    (xhr)=>{console.log(xhr)}
  )
```

## axios

使用举例

```javascript
//get
axios
  .get("/xxx", {
    params: {
      id: 123
    }
  })
  .then(res => {
    console.log(res);
  })
  .catch(err => {
    console.log(err);
  });
// 或者
axios("/user/12345");
//post
axios
  .post("/xxx", {
    qqq: "asd",
    www: "sad"
  })
  .then(res => {
    console.log(res);
  })
  .catch(err => {
    console.log(err);
  });
// 并发请求（concurrency）
function getUserAccount() {
  return axios.get("/user/12345");
}
function getUserPermissions() {
  return axios.get("/user/12345/permissions");
}
axios.all([getUserAccount(), getUserPermissions()]).then(
  axios.spread(function(acct, perms) {
    //当这两个请求都完成的时候会触发这个函数，两个参数分别代表返回的结果
  })
);
```

## fetch

Fetxh 的出现是为了替换传统的 xhr。目前很多新的库都默认使用 fetch

```javascript
fetch(url)
  .then(response => response.json())
  .then(data => console.log(data))
  .catch(e => console.log("Oops, error", e));
```

支持 asynx await

```javascript
async function(){
    try {
        let response = await fetch(url);
        let data = response.json();
        console.log(data);
        } catch(e) {
        console.log("Oops, error", e);
        }
}
```
