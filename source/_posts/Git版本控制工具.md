---
title: Git版本控制工具
categories: python
HTML格式实现标签样本: <center><font size=5 face="黑体">文本区</font></center>
date: 2020-05-17 23:23:45
top:
tags:
typora-root-url: ..
---



首先你要知道git的工作流程：

![](/assets/Snipaste_2020-05-17_23-09-34.png)

# 实战操作

# 1. 本地管理

## 1.1 初始化

首先先选一个工作目录，如 D/test

右键，Git Bash Here

执行命令：`git init` 则创建隐藏文件`.git`

暂存区和本地仓库就这个文件夹里面，`.git`里文件如下：

![](/assets/image-20201028211359921.png)

## 1.2 设置签名

用以区分不同开发人员身份。用以告诉git是谁提交的代码。

一般采用第二种。

（1）本地库级别设置签名命令：

`git config user.name zs`

`git config user.email zs@qq.com`

信息保存位置：./.git/config 文件(系统用户名文件里)

（2）一般直接设置系统级别的签名：

系统用户级别设置签名方式：

`git config --global user.name zs`

`git config --global user.email zs@qq.com`

信息保存位置：./.gitconfig 文件

## 1.3 忽略文件

文件夹里有些文件不需要上传到GitHub，可使用命令：

`touch .gitignore`生成忽略文件，并将其打开，写入忽略文件的格式，如`*.doc`，即为不上传后缀为.doc的文件到GitHub。

## 1.4 查看哪些文件未传

`git status`

## 1.5 上传到暂存区

`git add 文件名`

`git add --all`上传所有未提交文件

## 1.6 提交到本地仓库

`git commit -m "第一次代码提交"`

-m的意思是后面有提交说明文字。

## 1.7 查看提交日志

`git log`

## 1.8 版本回退

当前版本往上回退一个版本：`git reset --hard HEAD^`

回退n个版本：`git reset --hard HEAD~n`

通过id回退：`git reset --hard commit_id`

回退之后又想恢复到最新版本：`git reflog `查看每一次记录，通过id回退。

**git中的HEAD理解**：HEAD代表当前指针的意思，它是git内部用来追踪位置的，形象的记忆就是：你在哪，HEAD就指向哪。

上面加`--hard`的意思是：同时恢复到暂存区和工作区（工作区的新文件会被旧文件替代）；

加不加`--head`的区别：不加`--head`，在恢复的时候只恢复到暂存区。

`--soft` 和 `--hard `的区别：`--hard `会清空工作目录和暂存区的改动，而 `--soft`则会保留工作目录的内容，并把因为保留工作目录内容所带来的新的文件差异放进暂存区。

## 1.9 本地误删恢复

`git status`可以看到哪些文件被误删

依次执行：

```
git reset HEAD filename 
git checkout filename 
```

git reset HEAD filename 恢复文件就可以恢复被删掉的文件。

注意：另外补充一点，只有被commit后的文件才能被reset，若文件只是add到暂存区，则不能恢复。

这里举一个**例子**：代码1被add和commit；代码1在工作区被修改为代码2，代码2只是被add，忘记commit了，然后代码2被修改为代码3，此时又觉得代码3写了一半觉得不好，想回退到代码2，于是执行git reset --head HEAD，则工作区代码直接回退到代码1（因为本地仓库只记住了代码1的样子，回到当前代码，就是代码1了），代码2就永久消失了。

## 1.9 分支管理

每个开发者都有一个分支。通常还有div分支以及master分支。

### 1.9.1 查看当前分支

`git branch`

### 1.9.2 创建分支

`git branch zs` 创建zs分支

### 1.9.3 切换分支

`git checkout zs`

### 1.9.4 删除分支

`git branch -d zs`

### 1.9.5 创建的同时切换分支

`git checkout -b zs`

### 1.9.6 合并分支

把zs开发的代码合并到master主分支上，在master分支上执行：

`git merge zs`

# 2. 远程推送

## 2.1 准备工作

GitHub账号创建

仓库创建

### 2.1.1 密钥配置

在本地创建SSH：

​    在创建密钥之前，先看看用户目录下有无.ssh文件夹，再看看这个目录下有无id_rsa和id_rsa.pub这两个文件（分别对应私钥和公钥），如果没有，执行一下命令创建SSH Key：

`$ ssh-keygen -t rsa c "youremail@example.com"`

复制本地公钥到GitHub对应位置：

settings—SSH keys—New SSH Keys 

这样本地就可以和远程GitHub进行通信了！

![](/assets/image-20200620204431118.png)

## 2.2 添加远程仓库

先在Github上新建一个仓库，这个仓库就可以放置属于它的代码了。

`git remote add origin 远程仓库地址`  这里origin是给远程仓库取的名字

`git remote`可显示远程仓库的名字

若已有设置远程仓库：

`git remote -v`可显示远程仓库的地址

`git remote set-url origin  新仓库地址` 可修改远程仓库地址

## 2.3 推送到远程仓库

`git push -u XX 分支`

如：`git push -u origin master`

把当前分支master推送到远程master分支，-u会把本地master分支和远程master分支关联起来。

下一次再提交的话就可以省略-u

推送分支

切换到分支zs：`git checkout zs`

推送分支：`git push -u origin zs`

## 2.4 更新到本地

当从远程仓库拉取最新的代码到本地时，使用

```
git pull
或
git pull origin master
```

如果本地代码为最新，上述命令就没有作用，远程代码不会替换掉本地的。

只有远程是最新的时候，才会pull成功。

当远程是最新，发生冲突时，会以如下形式展示：

<img src="/assets/image-20200621170043414.png" style="zoom: 80%;" />

当远程和本地都是最新，发生冲突时，会以如下形式展示：

<img src="/assets/image-20200621171708361.png" style="zoom:80%;" />

当在本地把代码冲突解决了，才能重新本地commit。



代码下载：

`git clone 远程仓库地址`这里仓库地址可为其他人的。



`git branch -a`查看远程分支

![](/assets/image-20200518211946104.png)

# 3、一篇文章理解git的工作原理

https://www.lzane.com/tech/git-internal/