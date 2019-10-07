---
title: "Git Reflog"
date: 2019-10-07T22:13:09+08:00
draft: false
categories: ['Git']
---

# 是不是 git reset --hard 之后傻眼了~~~嗯？

来咯来咯~ git reflog 真的来咯~~  

### git reflog  

git reflog 工具会记录你每次对HEAD的更改，即当你提交、切换分支或者reset后，HEAD都会更改，reflog都会记录下来你的操作。

比如：
```javascript
// git log --oneline
b2d387d (HEAD -> master) add index.css
9515998 initial reset
29adb97 (origin/master, origin/HEAD) add branch_learn
1563c0f add log.md
53b7acc add What's commit/log.md
f318db2 git add --all

// 现在我们不小心回退到 1563c0f 这个提交
// git reset --hard 1563c0f
// git log --oneline
1563c0f (HEAD -> master) add log.md
53b7acc add What's commit/log.md
f318db2 git add --all
// 此时，暂存区和工作目录都回退到了指定的提交，那些未保存的修改都没了......

// 因为Git记录下来了我们每次对HEAD进行的更改，所以我们可以使用git reflog命令查看所有的HEAD改动记录。
// git reflog
1563c0f (HEAD -> master) HEAD@{0}: reset: moving to 1563c0f
b2d387d HEAD@{1}: commit: add index.css
9515998 HEAD@{2}: commit: initial reset
// 可以看到，第一条就是我们的reset命令。
// 第二条是最近的一次提交记录，即我们刚刚回退的记录，前面有它的散列值。
// 我们再次执行 git reset --hard，为什么要加上hard呢？因为我们要复原，暂存索引和工作目录都要。
// git reset --hard b2d387d
// git log --oneline
b2d387d (HEAD -> master) add index.css
9515998 initial reset
29adb97 (origin/master, origin/HEAD) add branch_learn
1563c0f add log.md
53b7acc add What's commit/log.md
f318db2 git add --all
// 看！又回来了！
```

其实，我只是为了简单介绍这个命令而举的这个例子，并不具有代表性，在实际过程中遇到的问题比这个复杂得多，所以还要随机应变，和其他命令配合。
