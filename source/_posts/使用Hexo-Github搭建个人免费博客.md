---

title: 使用Hexo+Github搭建个人免费博客
date: 2020-02-13 15:44:02
top:
tags:
categories: 工具
---
此文基本转自：http://blog.haoji.me/build-blog-website-by-hexo-github.html

 # 1. 前言

 使用github pages服务搭建博客的好处有：

> 1. 全是静态文件，访问速度快；
> 2. 免费方便，不用花一分钱就可以搭建一个自由的个人博客，不需要服务器不需要后台；
> 3. 可以随意绑定自己的域名，不仔细看的话根本看不出来你的网站是基于github的；
> 4. 数据绝对安全，基于github的版本管理，想恢复到哪个历史版本都行；
> 5. 博客内容可以轻松打包、转移、发布到其它平台；
## 1.1 准备工作
在开始一切之前，你必须已经：
 - 有一个github账号，没有的话去注册一个；
 - 安装了git for windows（或者其它git客户端）；
>  [下载地址](https://gitforwindows.org/)  
>  双击下载好的exe文件，一路next就好
 - 安装了node.js、npm，并了解相关基础知识
> Hexo是基于nodeJS环境的静态博客，会用到里面的npm工具。 
>
> [下载地址](https://nodejs.org/en/)（LTS为长期支持版，Current为当前最新版）
>
> 下载好msi文件后，双击打开安装，也是一路next，不过在Custom Setup这一步记得选 `Add to PATH`这样你就不用自己去配置电脑上环境变量了，装完在cmd中输入`path`可以看到你的node是否配置在里面（环境变量），没有的话你就自由发挥吧。
 - 安装hexo
> a. 先创建一个文件夹（用来存放所有blog的东西），然后`cd`到该文件夹下。
> b. 安装hexo命令：`$ npm i -g hexo` 
> c. 初始化命令：`$ hexo init` 
> `$ cd /f/Workspaces/hexo/`  
> `$ hexo init`
> 初始化完成之后打开所在的文件夹可以看到以下文件： 
![](%E4%BD%BF%E7%94%A8Hexo-Github%E6%90%AD%E5%BB%BA%E4%B8%AA%E4%BA%BA%E5%85%8D%E8%B4%B9%E5%8D%9A%E5%AE%A2_md_files/hexo-init.png?v=1&type=image)
> 其中：
>  node_modules：是依赖包 
> public：存放的是生成的html页面 ,这些文件将来都是要提交到github去的
> scaffolds：命令生成文章等的模板
> source：用命令创建的各种文章 
> themes：主题
> _config.yml：整个博客的基础配置 
> db.json：source解析所得到的 
> package.json：项目所需模块项目的配置信息

# 2. 搭建github博客
## 2.1 新建仓库
在你的个人github账户页面新建一个名为`username.github.io`的仓库（必须是你的用户名，其它名称无效），将来你的网站访问地址就是 `https://username.github.io`

由此可见，每一个github账户最多只能创建一个这样可以直接使用域名访问的仓库。

创建成功后，默认会在你这个仓库里生成一些示例页面，以后你的网站所有代码都是放在这个仓库里啦。
## 2.2 绑定域名
当然，你不绑定域名肯定也是可以的，就用默认的 `xxx.github.io` 来访问，如果你想更个性一点，想拥有一个属于自己的域名，那也是OK的。

创建域名后，然后到你的github项目根目录新建一个名为CNAME的文件（无后缀），里面填写你的域名。域名配置最常见有2种方式，CNAME和A记录，CNAME填写域名，A记录填写IP

注意：在你绑定了新域名之后，原来的`你的用户名.github.io`并没有失效，而是会自动跳转到你的新域名。
# 3 配置SSH key
你提交代码肯定要拥有你的github权限才可以，但是直接使用用户名和密码太不安全了，所以我们使用ssh key来解决本地和服务器的连接问题。

用git bash执行如下命令：

    $ cd ~/. ssh #检查本机已存在的ssh密钥

如果提示：No such file or directory 说明你是第一次使用git

    ssh-keygen -t rsa -C "邮件地址"

 然后连续3次回车，最终会生成一个文件在用户目录下，打开用户目录，找到`.ssh\id_rsa.pub`文件，记事本打开并复制里面的内容，打开你的github主页，进入个人设置 -> SSH and GPG keys -> New SSH key：
 ![](%E4%BD%BF%E7%94%A8Hexo-Github%E6%90%AD%E5%BB%BA%E4%B8%AA%E4%BA%BA%E5%85%8D%E8%B4%B9%E5%8D%9A%E5%AE%A2_md_files/2.png?v=1&type=image)
## 3.1 测试是否成功

    $ ssh -T git@github.com # 注意邮箱地址不用改
如果提示`Are you sure you want to continue connecting (yes/no)?`，输入yes，然后会看到：

> Hi liuxianan! You’ve successfully authenticated, but GitHub does not provide shell access.

看到这个信息说明SSH已配置成功！
此时你还需要配置：

    $ git config --global user.name "liuxianan"// 你的github用户名，非昵称
    $ git config --global user.email  "xxx@qq.com"// 填写你的github注册邮箱
# 4 发布
## 4.1 初始界面

    $ hexo g # 生成
    $ hexo s # 启动服务
`hexo s`是开启本地预览服务，打开浏览器访问 [http://localhost:4000](http://localhost:4000) 即可看到内容，很多人会碰到浏览器一直在转圈但是就是加载不出来的问题，一般情况下是因为端口占用的缘故，因为4000这个端口太常见了，解决端口冲突问题请参考这篇文章：

[http://blog.liuxianan.com/windows-port-bind.html](http://blog.liuxianan.com/windows-port-bind.html)

第一次初始化的时候hexo已经帮我们写了一篇名为 Hello World 的文章，默认的主题比较丑，打开时就是这个样子：
![](%E4%BD%BF%E7%94%A8Hexo-Github%E6%90%AD%E5%BB%BA%E4%B8%AA%E4%BA%BA%E5%85%8D%E8%B4%B9%E5%8D%9A%E5%AE%A2_md_files/3.png?v=1&type=image)

## 4.2 修改主题
既然默认主题很丑，那我们别的不做，首先来替换一个好看点的主题。这是 [官方主题](https://hexo.io/themes/)。

个人比较喜欢的2个主题：[hexo-theme-3-hexo](https://github.com/yelog/hexo-theme-3-hexo) 和 [hexo-theme-yilia](https://github.com/litten/hexo-theme-yilia)。

首先下载这个主题：

    $ cd /f/Workspaces/hexo/
    $ git clone https://github.com/yelog/hexo-theme-3-hexo.git themes/3-hexo
下载后的主题都在这里：
![](%E4%BD%BF%E7%94%A8Hexo-Github%E6%90%AD%E5%BB%BA%E4%B8%AA%E4%BA%BA%E5%85%8D%E8%B4%B9%E5%8D%9A%E5%AE%A2_md_files/4.png?v=1&type=image)
修改项目根目录下`_config.yml`中的`theme: landscape`改为theme: 3-hexo，然后重新执行`hexo g`来重新生成。

当然模板是作者提供的，可根据自己的喜好修改themes/3-hexo/_config.yml里面的内容

3-hexo使用说明：https://yelog.org/2017/03/23/3-hexo-instruction/

如果出现一些莫名其妙的问题，可以先执行`hexo clean`来清理一下public的内容，然后再来重新生成和发布。
## 4.2 上传到Github
在上传代码到github之前，一定要记得先把你以前所有代码下载下来（虽然github有版本管理，但备份一下总是好的），因为从hexo提交代码时会把你以前的所有代码都删掉。
如果你一切都配置好了，发布上传很容易，一句`hexo d`就搞定，当然关键还是你要把所有东西配置好。

首先，`ssh key`肯定要配置好。

其次，项目根目录配置`_config.yml`中有关deploy的部分：

正确写法：

    deploy:
      type: git
      repository: git@github.com:liuxianan/liuxianan.github.io.git
      branch: master

 上传中直接执行`hexo d`的话一般会报如下错误：


    Deployer not found: github 或者 Deployer not found: git
原因是还需要安装一个插件：

    $ npm install hexo-deployer-git --save
部署这个命令一定要用git bash，否则会提示`Permission denied (publickey).`
## 4.3 保留CNAME、README.md等文件
提交之后网页上一看，发现以前其它代码都没了，此时不要慌，一些非md文件可以把他们放到source文件夹下，这里的所有文件都会原样复制（除了md文件）到public目录的：
![](%E4%BD%BF%E7%94%A8Hexo-Github%E6%90%AD%E5%BB%BA%E4%B8%AA%E4%BA%BA%E5%85%8D%E8%B4%B9%E5%8D%9A%E5%AE%A2_md_files/5.png?v=1&type=image)
由于hexo默认会把所有md文件都转换成html，包括README.md，所有需要每次生成之后、上传之前，手动将README.md复制到public目录，并删除README.html。
# 5 常用hexo命令
常见命令：

    hexo new "postName" #新建文章
    hexo new page "pageName" #新建页面
    hexo generate #生成静态页面至public目录
    hexo server #开启预览访问端口（默认端口4000，'ctrl + c'关闭server）
    hexo deploy #部署到GitHub
    hexo help  # 查看帮助
    hexo version  #查看Hexo的版本

缩写：

    hexo n == hexo new
    hexo g == hexo generate
    hexo s == hexo server
    hexo d == hexo deploy

组合命令：

    hexo s -g #生成并本地预览
    hexo d -g #生成并上传

根目录下的_config.yml 里面都是一些全局配置，每个参数的意思都比较简单明了，所以就不作详细介绍了。

需要特别注意的地方是，冒号后面必须有一个空格，否则可能会出问题。
# 6 写博客
定位到我们的hexo根目录，执行命令：

    hexo new 'my-first-blog'
hexo会帮我们在`_posts`下生成相关.md文件

我们只需要打开这个文件就可以开始写博客了

注意：这里的.md文件是根据scaffolds里的post文件为模板生成的。

一般完整格式如下：

      ---
        title: postName #文章页面上的显示名称，一般是中文
        date: 2013-12-02 15:30:16 #文章生成时间，一般不改，当然也可以任意修改
        categories: 默认分类 #分类
        tags: [tag1,tag2,tag3] #文章标签，可空，多标签请用格式，注意:后面有个空格
        description: 附加一段文章摘要，字数最好在140字以内，会出现在meta的description里面
        ---
        
        以下是正文

写完之后，可通过一下命令预览及上传：

```
$ hexo s -g   #生成并本地预览
$ hexo d -g   #生成并部署到Github
```