---
title: Nginx是什么
date: 2019-01-16 20:51:01
tags: [Nginx]
cover_img: https://ws1.sinaimg.cn/large/006tNc79ly1fzbv3w7hevj30yg0f4k0x.jpg
feature_img:
description:
keywords: [Nginx]
---

# Nginx

作为一个前端可能没用过 Nginx，但绝对听过，那么他到底是什么呢

Nginx 是一款自由的、开源的、高性能的 HTTP 服务器和反向代理服务器；同时也是一个 IMAP、POP3、SMTP 代理服务器；Nginx 可以作为一个 HTTP 服务器进行网站的发布处理，另外 Nginx 可以作为反向代理进行负载均衡的实现。

## 安装

```zsh
brew install nginx
// 查看目前执行的服务
brew services list
// 启动nginx服务
brew services start nginx
// 停止nginx服务
brew services stop nginx
查看nginx的配置
vi nginx.conf
// 翻到最下面
添加.conf,让nginx识别.conf的配置
// 查看nginx所有的配置
nginx -T
// 重启动nginx服务
brew services restart nginx
```

## 正向代理和反向代理

- 正向代理
  FQ 就是一个典型的正向代理，我们要访问被 Q 的网站，需要将请求发送给代理服务器，代理服务器代替
  我们访问被 Q 的网站，然后把数据传给我们

- 反向代理

看图就好了
[tu](https://ws1.sinaimg.cn/large/006tNc79ly1fzbv3w7hevj30yg0f4k0x.jpg)

反向代理代理的是服务端，隐藏了服务端的信息，并且还能将请求分发给不同的服务器处理（负载均衡）

## 使用

进入/usr/local/Cellar/nginx
新建一个 test.json 文件

```json
{
  "name:"muxue"
}
```

打开http://localhost:8080/test.json就能看到了

### 配置

进入/usr/local/etc/nginx
就可以看到配置文件 nginx.cong.default
