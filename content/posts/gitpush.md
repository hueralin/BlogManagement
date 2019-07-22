---
title: "Git push 的使用"
date: 2019-07-22T14:32:37+08:00
draft: false
categories: ["Git"]
---

> 语法：git push <远程主机名> <本地分支名> ：<远程分支名>  

1. **git push origin master**  
最常用的一个写法，忽略了远程分支名，将本地master分支推送到origin中与之有追踪关系的对应分支（通常同名），若没有则创建一个对应的远程分支。  
2. **git push origin :master**  
忽略了本地分支名，意味着要推送一个本地的空分支到指定远程分支，即删除指定的远程分支，相当于`git push origin --delete master`  
3. **git push origin**  
如果当前分支和远程分支存在对应关系，则两个分支名都可省略掉，直接推送当前分支到对应的远程分支。  
4. **git push**  
如果只有一个远程主机，且只有一个与当前分支对应的远程分支，那么就可以全都省略，直接git push。  
5. **git push -u origin master**  
如果对应多个远程主机，-u 则指定一个默认主机，以后就可以直接使用`git push`推送当前分支到远程对应分支。  
6. **git push --all origin**  
不管是否存在对应的远程分支，将本地的所有分支全部推上去。  
7. **git push --force origin**  
一般情况下本地仓库比远程仓库旧的话会要求你先`git pull`进行更新，而 --force 表明不要更新，直接推上去。  
8. **git push origin --tags**  
不推送分支，只推送标签。  
---  
**关于追踪关系**  
追踪即本地分支跟进远程分支的变化，用于push、pull、merge等...  
*追踪远程分支*  
1. `git clone` 一般克隆下来的项目将自动创建master分支，并且自动关联到远程的master分支上  
2. `git branch -u 远程主机/远程分支名 本地分支名` 设置本地分支跟踪远程分支
3. `git checkout -b 本地分支名 远程主机/远程分支名` 从远程分支中签出本地分支  
4. `git branch -vv` 查看本地分支和远程分支的跟踪关系
