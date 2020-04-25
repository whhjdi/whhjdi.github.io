---
title: 不如学MongoDB
date: 2018-12-10 15:24:48
tags: [MongoDB]
cover_img: https://i.loli.net/2018/12/26/5c2355ae23c22.jpeg
feature_img:
description:
keywords: [MongoDB]
---

# mongodb

## 安装配置

下载就不说了，说说配置
把下载的文件放到你想放到的目录（比如：/usr/local）
添加环境变量

```
export PATH=${PATH}:/usr/local/MongoDB/bin
```

无论是 zsh 还是 bash 编辑完成都要 source 一下

然后在根目录 新建一个文件夹,赋予权限

```
sudo mkdir -p /data/db
sudo chown `id -u` /data/db
```

然后就可以运行`mongod`了

再打开 一个命令窗口输入`mongo`就可以使用了

## 基础命令

```
show dbs
use 集合名称
show collections
db
<!-- 增删改查 -->
db.集合.insert({'age':'12'}) #集合是你定义的
db.集合.remove({'age':'12'})
db.集合.update({'age':'12'},{'age':'13'})
db.集合.find()
db.集合.findOne()
db.集合.drop()
db.dropDatabase()
```

## Schema

- schema ：用来定义表的模版，实现和 MongoDB 数据库的映射。用来实现每个字段的类型，长度，映射的字段，不具备表的操作能力。
- model ：具备某张表操作能力的一个集合，是 mongoose 的核心能力。我们说的模型就是这个 Model。
- entity ：类似记录，由 Model 创建的实体，也具有影响数据库的操作能力。

Schema 中的数据类型

```javascript
String;
Number;
Date;
Boolean;
Buffer;
ObjectId;
Mixed;
Array;
```
