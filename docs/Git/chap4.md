---
title: 进阶之Git分支
layout: default
parent: Git
nav_order: 4
---

# 章节4 Git分支
> "学而时习之，不亦说乎。"

接下来上场的是Git的核心特性。**分支**是Git最强大的功能之一，它让你可以在不影响主线工作的情况下进行开发。

## **什么是分支**
在前面的章节中，所有的提交都是线性排列的。每次提交会生成一个指向新快照的指针，而这个指针就是分支。

默认情况下，Git会创建一个名为`main`的主分支。你可以基于任意提交创建新的分支，新分支本质上只是一个指向某次提交的轻量级指针，创建速度极快，几乎不占用额外空间。

> 本教程鼓励频繁地创建和使用分支，这与一些传统版本管理工具有很大不同。

## 本项目使用分支
1. <mark>禁止</mark>在`main`分支开发；
2. 在开发时，必须<mark>新建</mark>自己命名的分支；
3. 提交分支只能提交到<mark>自己命名</mark>的分支；
4. 提交之后需要<mark>告知</mark>提交信息以供合并。
5. 项目开发<mark>禁止使用`notepad++`</mark>

## **分支的基本操作**
### 查看分支
使用`git branch`可以列出所有本地分支，当前分支前会标有`*`号：
```
 git branch
   dev
 * main
```

### 创建分支
使用`git branch "branch_name"`创建一个新分支，替换双引号及其中内容为分支名称：
```
 git branch dev
```
> 此命令只是*创建分支*，并*不会切换*到新分支。

### 切换分支
使用`git switch "branch_name"`切换到指定分支：
```
 git switch dev
 Switched to branch 'dev'
```
> 也可以使用旧版本的兼容命令`git checkout "branch_name"`切换分支，但并非最推荐方式。

**创建并立即切换**到新分支，可以使用`-c`参数：
```
 git switch -c dev
 Switched to a new branch 'dev'
```
> 此命令等价于`git branch dev` + `git switch dev`。

### 删除分支
当一个分支的工作完成并合并后，可以将其删除（**请确保你的内容已经被推送**）
```
 git branch -d dev
 Deleted branch dev (was a3f2b1c).
```
> 如何推送见随后的[推送分支](#推送分支)章节

## **合并分支**
开发完成后，需要将开发分支的工作合并回`main`分支。

### 合并操作
1. 首先切换回目标分支（通常是`main`）：
    ```
     git switch main
    ```
2. 使用`git merge "branch_name"`合并指定分支：
    ```
     git merge dev
    ```

### 合并方式
根据提交历史的不同，Git会采用不同的合并策略：

**快进合并（Fast-Forward）**  
当`main`分支在创建`dev`之后没有新的提交，合并时Git只需将`main`指针直接移到`dev`的最新提交即可，不会产生新的提交节点。
```
 git merge dev
 Updating 8c7d6e5..a3f2b1c
 Fast-forward
  examplefile | 1 +
  1 file changed, 1 insertion(+)
```

**三方合并（Three-Way Merge）**  
当两个分支各自都有新的提交时，Git会找到两个分支的共同祖先，进行三方合并，并生成一个新的合并提交。
```
 git merge dev
 Merge made by the 'ort' strategy.
  examplefile | 1 +
  1 file changed, 1 insertion(+)
```

## **解决冲突**
当两个分支修改了同一个文件的同一处内容，Git无法自动合并，就会产生冲突。

### 冲突的表现
合并时如果出现冲突，Git会提示：
```
 git merge dev
 Auto-merging examplefile
 CONFLICT (content): Merge conflict in examplefile
 Automatic merge failed; fix conflicts and then commit the result.
```
此时打开冲突文件，会看到类似如下标记：
``` bash
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

> 解决冲突需要根据实际情况或进行相关交流来决定保留哪一方或合并双方的修改。

## **远程分支**
在内网服务器上拥有远程仓库时，你也可以将本地分支推送到远程，或从远程拉取分支。

### 推送分支
使用`git push origin "branch_name"`将本地分支推送到远程：
```
 git push origin dev
```
> 建议手动指定分支名称，不建议使用--set-upstream-to
> 此命令会将本地的`dev`分支推送到远程仓库，并自动创建远程分支`dev`。

### 拉取分支
从远程拉取指定分支并合并到当前分支：
```
 git pull origin main
```
> `git pull`等价于`git fetch` + `git merge`，先抓取远程数据再自动合并。

### 跟踪分支
首次推送时，使用`-u`参数设置跟踪关系：
```
 git push -u origin dev
```
设置跟踪后，后续在`dev`分支上直接使用`git push`和`git pull`即可。

> 建议使用本设置，pull and push 可以更加便捷，无需再指定远程分支名。
> 此方法不等同于`--set-upstream-to`。

---

**最后更新日期：** 2026年7月7日
