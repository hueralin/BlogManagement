---
title: "Git pull 的使用"
date: 2019-07-23T14:37:19+08:00
draft: false
categories: ["Git"]
---

> 语法：git pull <选项> <远程主机名> <远程分支名>:<本地分支名>  

注意：和git push的语法顺序稍有不同  
git push 的语法为：`git pull <远程主机名> <本地分支名>:<远程分支名>`  
git pull 意为从远程分支的最新版本拉取下来并于本地分支合并  

--- 
1. `git pull origin master:dev` 将origin的master分支拉取并合并到本地的dev分支  
2. `git pull origin master`  省略了本地分支名，即拉取并合并到本地分支  
3. `git pull origin`  若有 某远程分支与当前分支有追踪关系，则远程分支名也可以省略  
4. `git pull` 若只有一个远程主机，且只有一个远程分支与当前分支有追踪关系，则可以直接git pull  

---  
git pull相当于git fetch和git merge的简写  
比如：`git pull origin dev` 相当于 `git fetch origin dev` + `git merge origin/dev`

未完待续...
