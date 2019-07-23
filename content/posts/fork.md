---
title: "Fork工作流"
date: 2019-07-23T10:34:46+08:00
draft: false
categories: ["Git"]
---

**Fork工作流  实现协同开发**  
作为实习新人，在参与公司项目的开发时被要求使用Fork工作流，Fork工作流实际上就是从GitHub上fork一个原仓库，并与原仓库代码保持同步的一种工作方式。  
Fork工作流不再是只有一个中央代码库，而是给予每个人一个远程仓库（origin），外加一个唯一的中央仓库（upstream，或称为官方仓库）。点击Fork时就会生成一个自己的远程仓库，每次开发者提交时都会先将自己的贡献提交到origin，然后发起一个Pull Request请求给中央仓库，中央仓库的管理者会决定是否将你的代码提交到中央仓库，其他人都没这个权限。这使得管理者可以接受任何人的提交，而不需要给它们中央仓库的权限。  

---  
**具体流程**  
1. 从原仓库fork到自己的origin  
2. `git clone xxxxxx` 将origin克隆到本地  
3. `git remote add upstream xxxxx`  指向上游仓库，即原仓库  
4. `git checkout -b feature-xx origin/dev` 从远程分支检出本地分支进行开发  
5. 开发、修改。。。。提交。。。  
6. `git push origin feature-xx` 将本地feature分支push到origin  
7. 发起Pull Request请求管理员合并  
8. 上述合并可能会导致冲突，因为本地仓库可能比较旧了，需要更新  
9. `git checkout dev` 切回dev分支  
10. `git pull upstream dev` 从原仓库拉取dev分支的最新版本,更新本地的dev分支  
11. `git checkout -b feature-merge-dev dev` 从更新后的本地dev检出合并分支，并切换为该分支    
12. `git merge feature-xx` 合并你的修改，即feature-xx分支上的内容  
13. `git branch -m feature-merge-dev feature-xx` 将合并分支改名为feature-xx 覆盖原分支  
14. `git push origin feature-xx` push后再Pull Request就不会发生冲突了

