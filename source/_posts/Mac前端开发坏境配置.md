---
title: Mac前端开发环境配置
date: 2018-10-03 11:55:57
tags: [Mac]
categories: ["其他"]
cover_img: https://ws4.sinaimg.cn/large/006tNbRwly1fxzdjzf0ctj316f0u0b29.jpg
---

# Mac

## 安装 Homebrew

前提是科学上网
[ShadowsocksX-NG-R](https://github.com/wzdnzd/ShadowsocksX-NG-R/releases)

# 安装 Xcode 命令行工具

2019.6.6 -下边的命令已经~~失效~~ ,会查找软件失败，需要自己去 [developer apple 下载安装](https://developer.apple.com/download/more/)

```bash
$ xcode-select --install

```

# 安装 homebrow

```bash
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

## 安装必备工具

```bash
brew install coreutils vim git wget
```

### 使用 nvm 管理 node

```bash
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.34.0/install.sh | bash
```

建议使用 nvm 安装和管理 node

在 ~/.bashrc, ~/.profile, or ~/.zshrc 中放入下列代码

```bash
export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"  # This loads nvm
[ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion"
# This loads nvm bash_completion
```

```bash
nvm install node
# 安装最新的版本
mvm ls
# 列出所有安装的node版本
nvm use
# xxx切换node版本
```

<!--more-->

### npm

```bash
# 换淘宝源
npm config set registry https://registry.npm.taobao.org

# 恢复
npm config set registry https://registry.npmjs.org/

# 检测是否成功

# // 配置后可通过下面方式来验证是否成功
npm config get registry
# // 或
npm info express
```

## yarn

注意 brew 会在安装 yarn 的时候安装 node,因为我们使用 nvm 来管理 node，所以不要用用 brew 安装 node,所以

```bash
brew install yarn --ignore-dependencies
```

## git 和 github

### github 配置

打开命令行运行

```bash
ssh-keygen -t rsa -b 4096 -C "你的邮箱"
```

按回车三次，然后运行

```bash
cat ~/.ssh/id_rsa.pub
```

复制得到的字符串，然后打开 github，
进入[ssh keys](https://github.com/settings/keys)
点击 `New SSH Key` ，粘贴到 key 里，点击`add key`,完成
然后在命令行运行

```bash
ssh -T git@github.com
```

查看是否成功，yes 回车

### git 配置

```bash
git config --global user.name 你的英文名字
git config --global user.email 你的常用邮箱
git config --global push.default simple
git config --global core.quotepath false
git config --global core.editor "vim"
```

搞定

## iterm2&oh my zsh

参考[田园里的蟋蟀](https://www.cnblogs.com/xishuai/p/mac-iterm2.html)
和 github 文档

## hexo 博客迁移

clone 你的博客备份
进入你的博客的根目录

```bash
#安装hexo
npm install hexo-cli -g
#安装依赖
npm install
# 如果不能正常安装就使用yarn

# //下边的如果需要就安装
npm install hexo-deployer-git --save
npm install hexo-generator-feed --save
npm install hexo-generator-sitemap --save
```

现在就可以正常使用 hexo 写博客啦

## 其他

### node-sass

这东西会被墙，所以在 `~/.bashrc`中加入

```bash
export SASS_BINARY_SITE="https://npm.taobao.org/mirrors/node-sass"
```

### PATH 写法

```bash
export PATH="目录的绝对路径:$PATH"
```

### other

mac 安装了 conda 后，前面会有一个(base),通过下面的方法去掉

```
conda config --set auto_activate_base false
```
