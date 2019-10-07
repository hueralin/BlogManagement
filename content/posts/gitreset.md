---
title: "Git Reset"
date: 2019-07-26T17:41:51+08:00
draft: false,
categories: ["Git"]
---

> **背景：**  
惊了！本来想撤销上一次commit，但保留工作区的修改，但是手残加了 --hard 参数，修改全没了！！！  
后来，运行了一遍 `git reflog branchName` , 查看了下该分支上的历史记录，找到reset操作之前的历史，执行 `git reset --hard reset之前的记录点` 回来了！！！  

## git reset 命令  

***git reset - 重置当前指针，指向特定的状态***  

也就是说，该命令只会改变指针的指向，并不会清除commit记录。

[译文]()

reset命令有3种方式：

1. `git reset --mixed`：默认方式，即`git reset`，它回退到某个版本，只保留源码，回退commit和index信息  
2. `git reset --soft `：回退到某个版本，只回退了commit的信息，不会恢复到index file一级。如果还要提交，直接commit即可  
3. `git reset --hard `：彻底回退到某个版本，本地的源码也会变为上一个版本的内容

**Git取消对某个文件的跟踪**  
1. `git rm --cached readme.md`    仅删除readme.md的跟踪，并保留在本地。  
2. `git rm --f readme.md`    删除readme.md的跟踪，并且删除本地文件。
