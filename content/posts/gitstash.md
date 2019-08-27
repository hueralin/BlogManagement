---
title: "Git Stash"
date: 2019-08-26T20:09:47+08:00
draft: false,
categories: ['Git']
---

### Emmmm 

最近在实习的项目中经常使用 git stash 这个命令，现在来做一些总结。  

你可能会遇到这样一种情况，就是当你在进行开发或修改某个BUG的时候突然来了一个新任务（新任务与当前的开发项目属于同一个项目），时间紧迫，你需要立即切换分支去执行，可是你当前的任务还没有做完，切分支的话会被拒绝，怎么办呢？有种方法是先add再commit，确实可以，但是我感觉这么做的话会添加一个毫无意义的提交记录，虽然以后可以修改，但是太过麻烦。那么，就来试试 git stash 吧。

[git stash 官方文档第一版](https://git-scm.com/book/zh/v1/Git-%E5%B7%A5%E5%85%B7-%E5%82%A8%E8%97%8F%EF%BC%88Stashing%EF%BC%89)  
[git stash 官方文档第二版](https://git-scm.com/book/zh/v2/Git-%E5%B7%A5%E5%85%B7-%E5%82%A8%E8%97%8F%E4%B8%8E%E6%B8%85%E7%90%86)  

### git stash 应用场景  

git stash 的作用是将当前为提交的修改存储起来，让仓库还原到最后一次提交的状态。  
使用场景：  
1. 工作未完成，需要切换分支，但不想提交（常用）。  
2. 开发一个feature分支（未完成），此时想要合并一下主分支，看看做了哪些修改，提前解决冲突。  

现在来看看场景二的解决方法：  
```javascript
// 假设我在feature分支上开发，突然得知主分支更新了，于是我想要将主分支合并到当前分支，看看有没有冲突，有的话直接解决。
git checkout master  // 报错！会提示当前某些修改未存储提交  
// 当然，我是不想提交的，毕竟还没做完...于是，
git stash save "先搁这儿，我去瞅瞅主分支，拉下代码～"   // save 类似于 commit 的 -m 选项，即注释信息  
git stash list // 列出来所有的存储信息，看看有木有～  
git checkout master  // 切换到主分支
git pull origin master // 将远程主机的master分支拉下来  
git checkout feature    // 切回feature分支  
git merge master    // 将master分支合并到feature（可能会有冲突）  
git stash pop   // 将先前feature分支做的修改释放出来（可能会有冲突）  
// 有冲突合并即可，合并完，feature拥有了master分支最新的修改以及当前的特性开发。
```

### git stash  

git stash 实际上是一个栈结构，它可以获取工作目录中的中间状态（包括对已追踪文件的修改和暂存的变更，使用 git status 查看），将它保存在一个栈结构中，方便以后使用。（注意！未被追踪的文件不在存储的范围内！）  

git stash 命令：  
1. git stash（git stash push的简写）将当前工作目录的中间状态保存在栈中。  
2. git stash list 列出所有的存储。  
3. git stash apply stash@{0} apply后面是名字，表示要应用的存储（省略名字则应用最近的存储到仓库中），apply只是应用，并不会将存储弹出栈。  
4. git stash pop 将最近的存储应用到仓库中（但会将存储弹出栈），也可以指定存储的名字，将指定的存储应用到仓库后弹出栈。  
5. git stash clear 清空存储栈。  
6. git stash drop 删除最近的存储，也可以指定名字（但不会被应用到仓库）。  
7. git stash branch name 根据最近的stash创建一个分支，然后删除最近的stash，也可以指定存储名。（如场景二，如果最后应用stash的时候发生了冲突，可以使用这个命令，牵引出一个未合并的分支做副本）。  

---

**讲解一下 git stash push 和 git stash save**  
这两个命令都可以存储当前工作区中被追踪文件的修改，但是也有些区别。  
语法：  
1. git stash push -m 'xxxxx' src/xxx  
2. git stash save 'xxxxx'  
其中，git stash push能指定路径，存储指定的文件，而指定路径对git stash save来说无效，它会存储工作区中所有的被追踪文件的修改。  

### 使用 git stash 出现的问题  

Q：“我新建了一个文件，用stash存储了一下，切分支后发现该文件被带进了另一个分支”  
A：前面说过，新建的文件，如果没有add进暂存区，那么是不受stash管理的，git也不会追踪。所以对于新建的文件，应该在使用stash前先add一下。

Q：“不是说我BUG还没改完切到其他分支会报错吗，要我必须提交修改。可是我没提交修改，切master分支成功了。但是！BUG修复的代码被带到master了！”  
A：Emmmm...
