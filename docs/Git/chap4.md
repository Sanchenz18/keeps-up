---
title: 进阶之Git分支
layout: default
parent: Git
nav_order: 4
---

# 章节4 Git分支
> "学而时习之，不亦说乎。"

本章节进入Git的核心特性——分支。分支是Git最强大的功能之一，它让你可以在不影响主线工作的情况下进行开发。

## **什么是分支**
在前面的章节中，所有的提交都是线性排列的。每次提交会生成一个指向新快照的指针，而这个指针就是分支。

默认情况下，Git会创建一个名为`main`（或`master`）的主分支。你可以基于任意提交创建新的分支，新分支本质上只是一个指向某次提交的轻量级指针，创建速度极快，几乎不占用额外空间。

> Git鼓励频繁地创建和使用分支，这与一些传统版本管理工具有很大不同。

## **分支的基本操作**
### 查看分支
使用`git branch`可以列出所有本地分支，当前分支前会标有`*`号：
```
$ git branch
  dev
* main
```
添加`-v`参数可以查看每个分支最后一次提交的信息：
```
$ git branch -v
  dev   a3f2b1c add login feature
* main  8c7d6e5 fix typo
```

### 创建分支
使用`git branch "branch_name"`创建一个新分支，替换双引号及其中内容为分支名称：
```
$ git branch dev
```
> 此命令只是创建分支，并不会切换到新分支。

### 切换分支
使用`git switch "branch_name"`切换到指定分支：
```
$ git switch dev
Switched to branch 'dev'
```
> 也可以使用`git checkout "branch_name"`切换分支，但`git switch`语义更清晰，是官方推荐的方式。

创建并立即切换到新分支，可以使用`-c`参数：
```
$ git switch -c dev
Switched to a new branch 'dev'
```
> 此命令等价于`git branch dev` + `git switch dev`。

### 删除分支
当一个分支的工作完成并合并后，可以将其删除：
```
$ git branch -d dev
Deleted branch dev (was a3f2b1c).
```
> 如果分支尚未合并，Git会拒绝删除。此时可以使用`-D`参数强制删除，请谨慎使用。

## **合并分支**
开发完成后，需要将开发分支的工作合并回`main`分支。

### 合并操作
1. 首先切换回目标分支（通常是`main`）：
    ```
    $ git switch main
    ```
2. 使用`git merge "branch_name"`合并指定分支：
    ```
    $ git merge dev
    ```

### 合并方式
根据提交历史的不同，Git会采用不同的合并策略：

**快进合并（Fast-Forward）**  
当`main`分支在创建`dev`之后没有新的提交，合并时Git只需将`main`指针直接移到`dev`的最新提交即可，不会产生新的提交节点。
```
$ git merge dev
Updating 8c7d6e5..a3f2b1c
Fast-forward
 login.py | 1 +
 1 file changed, 1 insertion(+)
```

**三方合并（Three-Way Merge）**  
当两个分支各自都有新的提交时，Git会找到两个分支的共同祖先，进行三方合并，并生成一个新的合并提交。
```
$ git merge dev
Merge made by the 'ort' strategy.
 login.py | 1 +
 1 file changed, 1 insertion(+)
```

## **解决冲突**
当两个分支修改了同一个文件的同一处内容，Git无法自动合并，就会产生冲突。

### 冲突的表现
合并时如果出现冲突，Git会提示：
```
$ git merge dev
Auto-merging login.py
CONFLICT (content): Merge conflict in login.py
Automatic merge failed; fix conflicts and then commit the result.
```
此时打开冲突文件，会看到类似如下标记：
```
<<<<<<< HEAD
print("Hello from main")
=======
print("Hello from dev")
>>>>>>> dev
```
- `<<<<<<< HEAD`到`=======`之间是当前分支（main）的内容
- `=======`到`>>>>>>> dev`之间是被合并分支（dev）的内容

### 处理冲突
1. 打开冲突文件，选择保留需要的内容，删除冲突标记（`<<<`、`===`、`>>>`）
2. 使用`git add "file"`标记冲突已解决
3. 使用`git commit`提交合并结果

> 解决冲突没有固定公式，需要根据实际情况决定保留哪一方或合并双方的修改。

## **远程分支**
在内网服务器上拥有远程仓库时，你也可以将本地分支推送到远程，或从远程拉取分支。

### 推送分支
使用`git push origin "branch_name"`将本地分支推送到远程：
```
$ git push origin dev
```
> 此命令会将本地的`dev`分支推送到远程仓库，并自动创建远程分支`dev`。

### 拉取分支
从远程拉取指定分支并合并到当前分支：
```
$ git pull origin dev
```
> `git pull`等价于`git fetch` + `git merge`，先抓取远程数据再自动合并。

### 跟踪分支
首次推送时，使用`-u`参数设置跟踪关系：
```
$ git push -u origin dev
```
设置跟踪后，后续在`dev`分支上直接使用`git push`和`git pull`即可，无需再指定远程分支名。

---

**最后更新日期：** 2025年11月4日
