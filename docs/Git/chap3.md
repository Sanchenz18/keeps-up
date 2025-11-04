---
title: 基底要素
layout: default
parent: Git
nav_order: 3
---

# 章节3 Git基础
> “不积跬步无以至千里，不积小流无以成江海。”

对于Git这个工具的备餐环节已经完成了，现在是时候端上来了。  
本章节开始篇幅较长，但是也是精简过的版本，如果有不严谨的地方请参考[Progit](git_index)这本完全教程。

## **获取Git仓库**
### 具有两种方式
- 将尚未进行版本控制的本地目录转换为仓库。
- 从其他服务器**克隆**一个已经存在的项目仓库。

### 在已有的目录初始化仓库
1. 进入项目目录
2. 执行`git init`
3. 追踪所有的文件并进行初始化提交
```
git add *  # track all existing files under this directory
git status  # check current changes, but not necrssary
git commit -m 'initial commitment'  # leave a comment
```

> 步骤2命令实际上将创建一个名为`.git`的隐藏子目录。

### 克隆远程仓库
适用于团队合作或从**Github**等仓库托管平台下载仓库。

> 注意：此克隆(clone)会下载远程仓库每一个文件及其每一个版本，实际上是下载`.git`文件夹。

使用克隆命令`git clone "urls" `，替换引号中内容为仓库链接。
```
# 例如现在需要以"user"身份克隆项目仓库"192.168.x.x:example"
git clone user@192.168.x.x:example  # 可能需要密码
```

## **记录每次更新到仓库**
当本地拥有了一个真实的Git仓库，接下来描述如何继续开发。  

> 请回顾Git绪论中Figure1，了解基本流程。

在Git中，文件具有两种状态，*已追踪(Tracked)*与*未追踪(Untracked)*。前者表明此文件已被纳入版本管理，Git会监控它的变化并随提交被记录。后者则并不会被Git关心。

![]()

### 检查当前文件状态
使用`git status`可以查看那些文件处于Figure中的哪种状态。

1. 在一个新创建的仓库中使用`git status`则会得到以下输出，在当前分支没有任何提交。
    ```
    On branch main

    No commits yet

    nothing to commit (create/copy files and use "git add" to track)
    ```
2. 初次克隆远程仓库后，所有文件属于已追踪，且并未修改，此时如果使用`git status`则大概率会出现`Already up-to-date`字样。

### 追踪新文件
由于是新创建的新文件，它理所当然的不被Git追踪，若需要将它纳入版本管理，则使用`git add README`或者`git add *`来进行追踪。

1. 如果在一个新仓库中创建一个新的README文件，使用`git status`则有输出如下。
    ```
    On branch main

    No commits yet

    Untracked files:
      (use "git add <file>..." to include in what will be committed)
            README

    nothing added to commit but untracked files present (use "git add" to track)
    ```
    在当前的`main`分支没有提交(commits)，有一个名为`README`的文件没有被追踪(Untracked)，可以使用`git add`来追踪文件。
2. 追踪这个文件，输出如下。
    ```
    On branch main

    No commits yet

    Changes to be committed:
      (use "git rm --cached <file>..." to unstage)
            new file:   README
    ```
    可以看到`new file`字样表明`README`已经被追踪，并可以被提交。

### 暂存已修改的文件

### 移除文件

### 移动文件

### *忽略文件


## **查看提交历史**


## **撤消操作**
### 取消暂存的文件

### 撤消对文件的修改


## **远程仓库的使用**
### 查看远程仓库

### 添加远程仓库

### 从远程仓库中抓取与拉取

### 推送到远程仓库

### 远程仓库的重命名与移除

---

**最后更新日期：** 2025年11月4日
