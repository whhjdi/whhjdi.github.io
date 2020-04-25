---
title: Hello World
date: 2018-03-01 15:47:45
tags: [其他]
categories: ["其他"]
cover_img: https://ws4.sinaimg.cn/large/006tNbRwly1fxzdy5gzjwj31400u0tog.jpg
---

最近手痒，忍不住下载了黑苹果，踏上了一段易燃易爆之路（我发誓以后一定要买了 MAC，字体渲染的真好看，4k 屏开 1080p 的 hidpi 那效果应该灰常好，我的显示器是 24 寸 2k 的，很尴尬）

迅雷这次非常给面子，很快就把将近 5G 的景象下完了。然后，用 transmac 刻录到 U 盘，非常顺利。考虑到正在写的一些项目都放到了 github 也就没有备份文件，刷刷刷，开始安装

心灵手巧，业务熟练的我进入 pe，已掩耳盗铃响叮当之势迅速格式化固态，重启，进入 mac 的安装，悲剧的是遇到了坑，我使用 mac 之前的磁盘格式竟然无法安装，网上也没有找到答案，所有的教程都没有推荐使用新的 APFS。我的天，十点多了，我这个人有时候真的厉害，突然想尝试 APFS，进度条 100%，重启，进度条。。。。。。哇竟然到头来，好耶！

<!-- more -->

再次自动重启后就进入了配置界面，分分钟进入桌面，完美 10.13.6,竟然能用之前的 clover 基本没 bug,出来 wifi，ar9485 的驱动好早之前就可以了，但是突然不行了，作者也很长时间没更新了，我升级了 clover,lilu 和其他的 kext，不行，尝试了所有能想到的方法，均以失败告终，遂放弃

安装了所有需要的软件，mac 上的视频软件都没广告的吗？win10uwp 的也没广告，不过感觉 mac 的更精致一些。

总觉得缺点什么，嗯，24 寸的 2k 屏太小了，改分辨率又有毛刺，所以决定尝试开启 HIDPI，不过并不确定我的 HD4600 能不能开到 1080p 的 HIDPI 也就大概 3840\*2160，业务如此熟练地我方法早已懒熟于心，搞定，成功开启 1080p 的 HIDPI，然后突然发现，俩显示器必须都开着，我我我真的不想这样，所以把笔记本的分辨率调到最低，亮度也调到最低（和关着效果一样，哈哈哈哈）

系统搞好了，那么就开始学习吧，悲剧发生了，我的博客呢，我这么长时间的心血的，都是精华，我我我我，爆炸，心态十分爆炸，markdown 没了，我就不敢 hexo g -d 了，不然所有东西就都没了，不更新博客还能保证之前的博客都能访问，郁闷，无助

第二天，我开始尝试使用 jekyll 搭建网站，其实不复杂，但是由于访问素的的原因，需要更换 gem 的源，换就是了，啪啪啪，这个淘宝的源早就停止服务了，啪啪啪，这个 xxxchina.org 正在维护。几个小时都在找问题，心累啊。gem 活该没人用。最后终于找到暂时使用.com 的域名。一番折腾下来，我真的不想使用 jekyll（羡辙大佬的 blog 真的好有创意） 了。

我灵机一动，大不了再写一遍，毕竟之前总结的也不是特别好。写这篇的文章的时候，我已经写了新的 5 篇博客了，又发现了自己之前的一些知识盲区，查漏补缺，这波不亏，哈哈哈，hexo 我来了(等我全部总结完就可以更新了 heox g -d)

夜深了，晚安

2018.9.5 10.14 Beta10 发布了，我还在 10.13.6 就想尝试一下更新，看看能不能正常使用
emmm，虽然遇到了写小问题，不过还好有惊无险，坐等正式版发布

2018.9.8 苦恼了很久的无线搜不到信号的问题解决了，哈哈哈哈哈哈，治好了我多年来的失眠，99%完美的黑苹果
Welcome to [Hexo](https://hexo.io/)! This is your very first post. Check [documentation](https://hexo.io/docs/) for more info. If you get any problems when using Hexo, you can find the answer in [troubleshooting](https://hexo.io/docs/troubleshooting.html) or you can ask me on [GitHub](https://github.com/hexojs/hexo/issues).

## Quick Start

### Create a new post

```bash
$ hexo new "My New Post"
```

More info: [Writing](https://hexo.io/docs/writing.html)

### Run server

```bash
$ hexo server
```

More info: [Server](https://hexo.io/docs/server.html)

### Generate static files

```bash
$ hexo generate
```

More info: [Generating](https://hexo.io/docs/generating.html)

### Deploy to remote sites

```bash
$ hexo deploy
```

More info: [Deployment](https://hexo.io/docs/deployment.html)
