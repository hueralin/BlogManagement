---
title: "Git Reset"
date: 2019-07-26T17:41:51+08:00
draft: false,
categories: ["Git"]
---

惊了！本来想撤销上一次commit，但保留工作区的修改，但是手残加了 --hard 参数，修改全没了！！！  
后来，运行了一遍`git reflog branchName`, 查看了下该分支上的历史记录，找到reset操作之前的历史，执行`git reset --hard reset之前的记录点`回来了！！！  
# 回来了！！！！  

**Git取消对某个文件的跟踪**  
1. `git rm --cached readme.md`    删除readme.md的跟踪，并保留在本地。
2. `git rm --f readme.md`    删除readme.md的跟踪，并且删除本地文件。