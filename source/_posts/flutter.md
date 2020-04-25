---
title: flutter
date: 2019-06-07 22:22:42
tags: [flutter]
cover_img: https://i.loli.net/2019/08/21/VJnIgoRUBht6kbP.jpg
feature_img:
description:
keywords: [flutter]
---

## flutter

学习 flutter 已经很长时间，最近升级 mac os 10.15 出来很多问题，毕竟黑苹果
所以重装了 10.14.5,由于 6 月份的网络问题，在执行 pod setup 时 clone 速度慢的令人发指，
不得不找别的方法，所以写了这个文章。

1. 执行 `pod setup` 需要 clone 这个仓库 (git clone `https://git.coding.net/CocoaPods/Specs.git`)
   由于某些原因只有 10k/s,在尝试各种方法之后，发现了这个仓库（`https://git.coding.net/CocoaPods/Specs.git`）
   满速下载，美滋滋.这好像是腾讯的东西，5 人 (含) 以下团队免费使用。
2. clone 完仓库之后还没完，需要把文件夹重命名为`master` 放在 `~/.cocoapods/repos` 路径就可以了，然后执行`flutter doctor`看看是否配置成功

3. 突然发现 flutter 官方大中文文档已经可以访问了，质量还不错

4. 配置好环境变量发现`fluter doctor`没法执行
   原来是我的路径写的是`~/development`,`~`他没识别出来,改成`/Users/用户名`就好了
5. 由于众所周知的原因 `gradle`迟迟没下载好，导致卡在 Initializing gradle... 的过程中,这就需要手动下载了
   到[这个网站](http://services.gradle.org/distributions/)下载 gradle-4.10.2-all.zip 不解压放到对应的目录就好了
   （.gradle>wrapper>dists>gradle-4.10.2-all>很长的一堆字母，如果解压方队 dists 目录他还是会下载的
6. 成功执行到 Resolving dependencies... 又卡住了，还是那个原因，google。。。换成阿里的源就行了

   ```dart
   buildscript {
       repositories {
           // google()
           // jcenter()
           maven{ url 'https://maven.aliyun.com/repository/google' }
           maven{ url 'https://maven.aliyun.com/repository/jcenter' }
           maven{url 'http://maven.aliyun.com/nexus/content/groups/public'}
       }
       dependencies {
           classpath 'com.android.tools.build:gradle:3.2.1'
       }
   }
   ```

7. 当使用最新的 API29（Android 9.+）的模拟器时，会报错

```dart
Error connecting to the service protocol: HttpException: Connection closed before full header was
received, uri = http://127.0.0.1:53661/fjwk-3cJQB0=/ws
```

换成 API28 就可以愉快的热更新了
