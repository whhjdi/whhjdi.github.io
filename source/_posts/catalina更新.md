---
title: catalina更新
date: 2019-10-05 17:49:53
tags: [macos]
cover_img:
feature_img:
description:
keywords:
---

# MacOS Catalina

10.5 更新了 gm 版本的 Mac OS 10.15,正式版不远了
主要解决的问题有以下几个

1. Hidpi 的问题 （已解决）
2. ar9485 无线网卡的问题（已解决）
3. 从 10.14.6 开始，外界显示器开启 hidpi 会影响关机（未解决）
4. Alfred 搜索显示俩个不同路径的相同结果（已解决）
   首先，catalina 更改了分区结构，就是一个容器的概念，系统变成了 readonly,所以之前的 hidpi 文件替换就不行了，好在 one-key-hidpi 的脚本已经支持了 catalina,问题 ① 解决

问题二，在把 airport40.kext 放到 Library/Extensions,重建缓存时出现报错
`Prelink failed for com.apple.driver.AirPort.Atheros40; omitting from prelinked kernel.`，不得已在 tonymacx86 上找到了一个支持 10.15 添加 airport40.kext 到 IO80211Family.kext 的安装包，前提是要用 hackintool 把系统盘变成可读写，完成之后，用 xcode 编译大佬改好的 wifi 驱动`https://github.com/athlonreg/ATH9KFixup`,把`ATH9KFixup.kext`放到 clover/kexts 中，`ATH9KInjector.kext`放到`Library/Extensions,`,重建缓存，重启电脑搞定

通过查询 Alfred 的 changeLog,在 Alfred 中键入 reload，即可解决，这其实就是 10.15 更改的路径，但缓存仍然存在而产生的 bug
