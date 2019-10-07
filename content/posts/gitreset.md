---
title: "Git Reset"
date: 2019-07-26T17:41:51+08:00
draft: false,
categories: ["Git"]
---

> **背景：**  
惊了！本来想撤销上一次commit，但保留工作区的修改，但是手残加了 --hard 参数，修改全没了！！！  
后来，运行了一遍 `git reflog branchName` , 查看了下该分支上的历史记录，找到reset操作之前的历史，执行 `git reset --hard reset之前的记录点` 回来了！！！  

[参考文章](https://git-scm.com/book/zh/v2/Git-%E5%B7%A5%E5%85%B7-%E9%87%8D%E7%BD%AE%E6%8F%AD%E5%AF%86)  

## 三棵树  

|树|用途|
|:--|:--|
|HEAD引用|上一次提交的快照，下一次提交的父结点|
|索引文件|预期的下一次提交的快照|
|work directory|工作目录|

### HEAD  

HEAD是当前分支引用的指针，指向当前分支的最后一次提交。是上一次提交的快照。不过我在其他文章里看到说这棵树应该是“提交记录”。Emmmm，也可以，提交记录就可看成是一个链表，HAED一直都是头指针，每个分支都有各自的提交记录。

### INDEX  

索引即暂存索引树，预期的下一次提交，是Git的暂存区（一个文件被git add后就给暂存索引树添加一条）

### 工作目录  

工作目录就是实际的工作区，就是你敲代码的地方。上面两个都是放在了 .git 目录中，不太直观。在添加到暂存区和提交之前，你可以在工作目录里随意修改，可看成沙盒。

## 版本控制的工作流程  

首先，进入一个只有一个file.txt文件的目录。执行 git init，初始化一个仓库。此时HEAD指向master分支，但是master分支还未创建。工作目录中只有一个file.txt文件。
![](https://git-scm.com/book/en/v2/images/reset-ex1.png)

执行 git add file.txt，将其纳入版本控制，即将该文件添加进了暂存区（索引）中。
![](https://git-scm.com/book/en/v2/images/reset-ex2.png)

接着执行 git commit，它会从暂存索引中取出内容，并将其保存为一个永久的快照，然后创建一个指向该快照的提交对象，该提交对象保存在提交记录树中，然后更新master指向该提交对象。此时如果我们执行 git status，会发现什么也没有，因为此时这三棵树的状态相同。
![](https://git-scm.com/book/en/v2/images/reset-ex3.png)

然后我们修改file.txt。
![](https://git-scm.com/book/en/v2/images/reset-ex4.png)

修改完后执行 git status，会发现提示“Changes not staged for commit”，因为此时暂存索引树的状态和工作目录不一样了。然后我们再调用 git add，将修改后的文件添加进暂存索引中。
![](https://git-scm.com/book/en/v2/images/reset-ex5.png)

再执行 git status，发现提示“Changes to be committed”，因为此时暂存索引树的状态和提交记录树不一样了（即现在预期的下一次提交与上一次提交不同），最后执行 git commit完成提交。
![](https://git-scm.com/book/en/v2/images/reset-ex6.png)

再执行 git commit，发现什么也没有，因为此时三棵树又都一样了。

## git reset 命令  

***git reset - 操作上面的三棵树***  

### git reset --soft  

**仅移动HEAD的指向**，reset操作就到此为止了。不会修改暂存索引树和工作目录树，也就是说如果在执行reset操作前，你修改了工作目录中的文件甚至是执行了 git add，这些都不会受到影响。仅仅将HEAD指向了一个特定的提交记录。
![](https://git-scm.com/book/en/v2/images/reset-soft.png)

### git reset --mixed  

reset操作的默认执行方式。在移动HEAD指向的基础上，**更新索引**。即使用HEAD所指向的快照的内容来更新（覆盖）暂存索引中的文件，reset操作到这一步就结束了。也就是说，--mixed 会撤销掉添加（git add）进暂存区的内容，而工作目录中的修改结果不变（如果有修改的话）。
![](https://git-scm.com/book/en/v2/images/reset-mixed.png)

### git reset --hard  

**更新工作目录**，在上面两个的基础上，将暂存索引中的内容拿出来更新工作目录。也就是说，HEAD、暂存索引、工作目录此时都是一样的，都是某个特定的历史提交的内容。也就是说未提交的修改和暂存都会被覆盖掉。
![](https://git-scm.com/book/en/v2/images/reset-hard.png)

**Git取消对某个文件的跟踪**  
1. `git rm --cached readme.md`    仅删除readme.md的跟踪，并保留在本地。  
2. `git rm --f readme.md`    删除readme.md的跟踪，并且删除本地文件。
