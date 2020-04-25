---
title: GitSheet
date: 2019-10-21 11:18:53
tags: [git]
cover_img:
feature_img:
description:
keywords:
---

## branches

```js
// 列出所有本地分支
git branch

// 列出远程和本地的分支
git branch -a

// 创建本地分支并切换到它
git branch -b branch_name

// 切换到现有的分支
git checkout branch_name

// 将分支推到远程
git push origin branch_name

// 重命名当前分支
git branch -m new_name

// 删除本地分支
git branch -d branch_name

// 删除远程分支
git push origin :branch_name
```

## logs

```js
// 以单行显示提交历史记录
git log --oneline

// 显示最近n次提交的提交历史记录。
git log -2

// 显示最近n次提交的提交历史记录（使用diff）。ˇ
git log -p -2

// 在工作树中显示所有本地文件更改。
git diff

// 显示对文件所做的更改。
git diff myFile

// 显示谁更改了文件中的内容和更改的时间。
git blame myFile

// 显示远程分支及其到本地的映射。
git remote show origin

```

## cleanup

```js
// 删除所有未跟踪的文件。
git clean -f

// 删除所有未跟踪的文件和目录。
git clean -df

// 撤消对所有文件的本地修改。
git checkout -- .

// 打开文件 Unstage a file.
git reset HEAD myfile
```

## tags

```js
// 列出所有标签。
git tag

//创建新标记。
git tag -a tag_name -m "tag message"

//将所有标签推到远程。
git push --tags
```

## stashes

```js
// 将更改保存到存储区
git stash save "stash name" && git stash

//列出所有stash
git stash list

// 应用一个stash
git stash pop

```
