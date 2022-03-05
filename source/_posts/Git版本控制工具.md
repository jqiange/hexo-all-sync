---
title: Git版本控制工具
categories: 工具
HTML格式实现标签样本: <center><font size=5 face="黑体">文本区</font></center>
date: 2022-03-05 23:23:45
top:
tags:
typora-root-url: ..
---



关于GitHub访问巨慢或者打不开的解决方案：[如何稳定访问github (nyakohub.github.io)](https://nyakohub.github.io/technology/how-to-access-github-stably.html)

推荐上文的第四种方案 --> [dotnetcore/FastGithub: github加速神器，解决github打不开、用户头像无法加载、releases无法上传下载、git-clone、git-pull、git-push失败等问题](https://github.com/dotnetcore/FastGithub)

------------------------------------------------------------------------------------------------------------------

正文开始介绍Git版本控制工具。

# 1、基本知识点

首先你要知道Git的工作流程：

![](https://image--1.oss-cn-shenzhen.aliyuncs.com/image-20220305113602583.png)

Git 仓库一共分为4个区：**工作区；暂存区；本地仓库，远程仓库**。前三个区都是在你电脑本地的，远程仓库我们一般在GitHub托管平台上。

首先我们看看一个Git仓库文件夹目录：

![](https://image--1.oss-cn-shenzhen.aliyuncs.com/image-20220305130607994.png)

- workspace/  就是**工作区**
- .git/index/     就是**暂存区**，暂存区只是一个简单的索引文件而已，包含的是文件的目录树。索引指向的是.git/objects下的文件。 
- .git/                就是**本地仓库**

# 2、创建GitHub账号

1. 登录GitHub官网：[GitHub](https://github.com/)，Sign up注册；Sign in 登录。

2. 新建仓库New repository，取好名字。

3. 复制SSH key到GitHub，将~/.ssh/id_rsa.pub 中的内容复制到GitHub---Setting---SSH and GPG keys---new SSH key里面即可。

   ```
   # 在cmd中执行以下命令创建SSH key：
   $ ssh-keygen -t ed25519 -C "your_email@example.com"
   # 或者
   $ ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
   ```

   完毕后，本地电脑就会新建~/.ssh/id_rsa.pub和id_rsa文件。

此时基本工作已经完毕。本地就可以和远程GitHub进行通信了！

> id_rsa和id_rsa.pub这两个文件，分别对应私钥和公钥

如有需要，本地存在多个SSH keys，可参照 [Hexo 博客迁移与多平台使用 | jqiange](https://jqiange.github.io/Hexo博客迁移与多平台使用/)   第2.3章节处理

# 3、本地实战

开始之前，要安装好Git管理工具：

- Windows下安装：[Git for Windows](https://gitforwindows.org/)
- Linux安装：`apt-get install git`

## 3.1 从本地到远程

### 3.1.1 初始化

首先选一个工作目录，如 D/test/，将其变成git本地仓库。

右键，Git Bash Here，执行命令：`git init` 则创建隐藏文件`.git`，暂存区和本地仓库就这个文件夹里面。

`.git`里文件如下：

![](https://image--1.oss-cn-shenzhen.aliyuncs.com/image-20201028211359921.png)

### 3.1.2 设置签名

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

### 3.1.3 添加远程仓库

`git remote add origin 远程仓库地址.git`  这里origin是给远程仓库取的别名

`git remote`可显示远程仓库的别名

若已有设置远程仓库：

`git remote -v`可显示远程仓库的地址

`git remote set-url origin  新仓库地址` 可修改远程仓库地址

### 3.1.4 保存并上传文件

1. 上传到暂存区

   `git add 文件名`

   `git add --all`上传所有未提交文件

2. 提交到本地仓库

   `git commit -m "第一次代码提交"`

   -m 后面加提交说明文字。

3. 推送到远程仓库

   `git push -u 仓库别名 分支`

   如：`git push -u origin master`

   把当前分支master推送到远程master分支，-u会把本地master分支和远程master分支关联起来。下一次再提交的话就可以省略-u。

   推送到分支

   切换到分支zs：`git checkout zs`

   推送分支：`git push -u origin zs`

### 3.1.5 忽略文件

文件夹里有些文件不需要上传到GitHub，可使用命令：

`touch .gitignore`生成忽略文件，并将其打开，写入忽略文件的格式，如`*.doc`，即为不上传后缀为.doc的文件到GitHub。

### 3.1.6 状态查看

`git status`   查看哪些文件未传

`git log`         查看提交日志

### 3.1.7 版本回退

> 只能回退到commit过的版本

当前版本往上回退一个版本：`git reset --hard HEAD^`

回退n个版本：`git reset --hard HEAD~n`

通过id回退：`git reset --hard commit_id`

回退之后又想恢复到最新版本：`git reflog `查看每一次记录，通过id回退。

`git reflog --date=iso`查看每次commit记录并显示时间。

---------------------------

**git中的HEAD理解**：HEAD代表当前指针的意思，它是git内部用来追踪位置的，形象的记忆就是：你在哪，HEAD就指向哪。

上面加`--hard`的意思是：同时恢复到暂存区和工作区（工作区的新文件会被旧文件替代）；

加不加`--head`的区别：不加`--head`，在恢复的时候只恢复到暂存区。

`--soft` 和 `--hard `的区别：`--hard `会清空工作目录和暂存区的改动，而 `--soft`则会保留工作目录的内容，并把因为保留工作目录内容所带来的新的文件差异放进暂存区。

### 3.1.8 本地误删恢复

> 只有被commit后的文件才能被恢复，若文件只是add到暂存区，则不能恢复。

`git status`可以看到哪些文件被误删

依次执行：

```
git reset HEAD filename 
git checkout filename 
```

这里举一个**例子**：文件1被add和commit；文件1在工作区被修改为文件2，文件2只是被add，忘记commit了，然后文件2被修改为文件3，此时又觉得文件3写了一半觉得不好，想回退到文件2，于是执行git reset --head HEAD，则工作区代码直接回退到文件1（因为本地仓库只记住了代码1的样子，回到当前代码，就是代码1了），代码2就永久消失了。

### 3.1.9 分支管理

每个开发者都可以有一个分支。通常还有div分支以及master分支。git所有分支之间彼此互不干扰，各自完成各自的工作和内容。可以在分支使用完后**合并到总分支(原分支)** 上，安全、便捷、不影响其他分支工作。

从项目创建之初，有且唯一的分支就是主分支，主分支被叫做`master`。

`git branch`                   查看当前分支

`git branch zs`             创建zs分支

`git checkout zs`         切换分支

`git branch -d zs`       删除分支

`git checkout -b zs`   创建的同时切换分支

`git merge zs`    合并zs分支，在master分支上执行

`git branch -a`查看远程分支

![](https://image--1.oss-cn-shenzhen.aliyuncs.com/image-20200518211946104.png)

## 3.2 从远程到本地

当从远程仓库拉取最新的代码到本地时，使用

```
git pull
或
git pull origin master
```

> 如果本地代码为最新，上述命令就没有作用，远程代码不会替换掉本地的。
>
> 只有远程是最新的时候，才会pull成功。

以上是本地仓库与远程仓库发生的互动。

但若本地不是仓库，只是个普通文件夹时，可以使用git clone获取到远程资源：

`git clone git@github.com:xxxx.git`

完成之后，就将整个仓库克隆下来了，文件名就是仓库名，同时我们可以发现，克隆下来的文件就变成仓库了，包含.git文件夹，git命令也有反应。

## 3.3 冲突

有时候，我们会遇到`git push` 或者`git pull`发生conflict的情况，一般来讲可能有以下几种原因：

### 3.3.1 git push 发生冲突

一般来讲，发生这种现象的原因是：

- 我们本地版本是旧版本v0，在没有拉取最新版本v1的情况下，我们在旧版本V0直接改动进行push v1'，这时v1就和v1'发生冲突；或者我们在改动本地之前拉取了最新版本v1，然后别人又push了新版本v2，等我们改动后进行push v2'的时候，这时v2就和v2'发生冲突；

**解决方案**：

```
git push origin master
# conflict...error
git pull --rebase
git push origin master
```

![](https://image--1.oss-cn-shenzhen.aliyuncs.com/image-20220305164424111.png)

- 还有一种情况是：V1和V1'改动的是同一处，这时候 git pull --rebase 会失败，因为Git肯定不知道到底要保存谁的。这时候就需要手动解决。

**解决方案**：依次执行

1. 打开本地文件，选择我们要保留的代码，然后再把`>>>>>, ======, <<<<<<`这些提示行给去掉。（git pull --rebase 后才有`>>>>===`）

2. 执行以下命令

   ```
   git add --a
   
   git commit -m 'resolve conflict'
   
   git rebase --continue
   
   git push origin master
   ```

### 3.3.2 git pull 发生冲突

这种情况一般发生在本地版本较新时，而我们又想放弃本地所有改动（不管有没add或commit），用远程文件强行覆盖。

```
git fetch --all
git reset --hard origin/master
```

一般来讲，我们常见的是选择版本回退的方法。参照 3.1.7 版本回退。

### 3.3.3 git push 冲突案例演示

1. 本地文件 test.txt

   ```
   print('basev0')
   ```

   Push后，远程仓库：

   ```
   print('basev0')
   ```

2. 直接在GitHub仓库修改远程文件并保存

   ```
   print('remotev1')
   ```

3. 修改本地文件

   ```
   print('localv1-')
   ```

   Push，发生冲突。

   执行 `git pull --rebase` ，失败。接下来最好是**手动解决冲突**：

   

4. 手动解决冲突

   此时本地打开该冲突的文件，内容为：

   ```
   <<<<<<< HEAD
   print('remotev1')
   =======
   print('localv1-')
   >>>>>>> dc10440 (localv1-)
   ```

   手动合并的方法很简单，就是我们选择我们要保留的代码，然后再把`>>>>>, ======, <<<<<<`这些提示行给去掉。最后重新add和commit。

   假设我们保留这个：

   ```
   print('localv1-')
   ```

   再执行：

   `git add --a`

   `git commit -m 'resolve conflict'`

   `git rebase --continue`

   `git push origin master`


即可大功告成！

# 3、深入阅读

[这才是真正的Git——Git内部原理 - LZANE | 李泽帆（靓仔）](https://www.lzane.com/tech/git-internal/)

[Git - Book (git-scm.com)](https://git-scm.com/book/zh/v2)

