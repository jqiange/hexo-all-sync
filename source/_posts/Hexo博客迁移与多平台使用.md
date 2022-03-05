---
title: Hexo博客迁移与多平台使用
categories: 工具
HTML格式实现标签样本: <center><font size=5 face="黑体">文本区</font></center>
mathjax: true
date: 2022-03-05 01:26:56
top:
tags:
---



> 当我们**换电脑**或者需要**多台电脑维护** GitPages博客时，可以采用以下方案执行。

# 一、在旧电脑上的操作

## 1.1 准备工作

1、在GitHub仓库上新建一个仓库XXSS，用来放置博客文件目录Hexo/ 下所有文件

2、配置SSH Key：将本机的 id_rsa.pub 文件内容复制到GitHub上相应位置

3、将博客文件目录Hexo/变成Git仓库并push，cmd执行

```
git init
git remote add origin git@github.com:XXSS.git
git add --all
git commit -m 'save file to GitHub'
git push origin master
```

以上就完成了。



# 二、在新电脑上的操作

## 2.1 安装工具

1、安装 git for windows

2、安装 node.js

3、配置SSH Key：将本机的 id_rsa.pub 文件内容复制到GitHub上相应位置

- **注意在生成SSH key的时候，一定要先确认下本机是否已存在**，避免覆盖。如本地需多个key（对应不同域名网站）,参照2.3章节处理

- 验证是否配置成功：`ssh -T git@github.com  -i  ~/.ssh/id_rsa` 出现...successfully authenticated...即可

4、安装 hexo：在cmd 执行 `npm i -g hexo`

5、创建博客目录：如Hexo/，以后这里就是博客的工作目录

- 初始化：在博客工作目录下，右键Git Bash中执行 `hexo init`

5、配置

```
$ git config --global user.name "liuxianan"        // 你的 github 用户名，非昵称
$ git config --global user.email  "xxx@qq.com"     // 填写你的 github 注册邮箱
```

> 注意：此时博客文件夹并不是一个git仓库。

以上新电脑上的基本配置已经做完了。

## 2.2 导入更换文件

### 2.2.1 更新文件

在旧电脑上时，我们已经将所有的博客目录下的文件上传到XXSS仓库里了，接下来就要用远程文件更新本地的文件。

目前我们的博客目录Hexo/只是一个普通文件夹，下面需要将其变成git本地仓库：

执行如下命令：

```
git init
git remote add origin git@github.com:XXSS.git       //添加目标远程仓库
git pull origin master                              //拉取文件
```

  这时，如果显示：Permission denied....fatal: Could not read from remote repository. 按照2.3章节处理。

由于我们是准备强制覆盖本地文件的，可以使用：

```
git fetch --all
git reset --hard origin/master
```

> git fetch从远程下载最新的，而不尝试合并或rebase任何东西。
>
> git reset将主分支重置为您刚刚获取的内容，--hard选项更改工作树中的所有文件以匹配origin/master中的文件。

这时候我们可以检查是否全部更新完毕。

### 2.2.2 善后工作

我们可以执行 `hexo s -g` 进行本地预览，发现可能还有问题，需要进一步处理。

执行 `npm install`，(由于仓库有一个.gitignore文件，里面默认是忽略掉 node_modules文件夹的，也就是说仓库的hexo分支并没有存储该目录[也不需要]，所以需要install下)。

这时候基本可以本地预览了，但是可能会报错，如： Error: Cannot find module 'object-assign'

本着缺啥补啥的原则，直接安装它：`npm i object-assign`

至此，所有工作基本已经做完。

### 2.2.3 图床

Typora新版本已经支持图床，在偏好设置里面：上传服务选择PicGo(app)。这样就不用本地存储图片，可以更好的管理图片了。



## 2.3 处理多个本地Key

一般情况下，我们本地只需要一个Key就行。但是有时候，我们的机器上会有很多的Git Host，比如公司GitLab、我们自己的GitHub，我们需要用不同的key来区分。于是我们在生成Key的时候，可以命名不同的名字加以区分。

如默认的  `id_rsa.pub`和`id_rsa`；还可以命名其他名字：`id_rsa_mee.pub`和`id_rsa_mee`

为了让不同的Host区分使用不同的SSH Key，需要做点额外配置（否则会Permission denied）：

在 ~/.ssh/ 密钥同级目录下，新建文件名为config，写入：

```
# 1 github setting 
Host github.com                      
HostName github.com                  #ip也可
User xxx                             #这里写git config user.name
IdentityFile ~/.ssh/id_rsa
 
# 2 gitlab setting
Host gitlab.com                       
HostName gitlab.com
User qiangjiang
IdentityFile ~/.ssh/id_rsa_mee
```

-  `Host` ： 相当于一个别名，这里可以使用任意字段或通配符。访问ssh的时候如果服务器地址能匹配上这里Host指定的值

- `HostName` ：真正连接的服务器地址

- `User`：自定义的用户名

- `PreferredAuthentications` 指定优先使用哪种方式验证，支持密码和秘钥验证方式，默认可以不写
  - publickey, password, keyboard-interactive，支持这三种

- `IdentityFile`：指定本次连接使用的密钥文件

配置完保存即可。









