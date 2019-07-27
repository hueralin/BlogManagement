---
title: "Git分支"
date: 2019-07-26T16:45:41+08:00
draft: false
categories: ["Git"]
---

**分支就是一个指向单个commit的指针**  
![分支指向单个commit](/img/posts/fork-commit.jpg ''fork-commit'')

分支基本操作：  
1. 分支改名：`git branch -m oldName newName`  
2. 删除分支：`git branch -d feature`  
3. 从master签出feature：`git checkout -b feature master`  
4. 查看本地分支：`git branch`  
5. 查看远程分支：`git branch -r`  
6. 查看所有分支：`git branch -a`  
7. 合并分支：`git merge B` 要想将B分支合并到A分支，首先要切换回A分支，在执行merge命令  
