---
title: 关于hexo使用过程中报错问题汇总
date: 2020-02-12 23:03:46
top:
tags: Hexo
categories: 工具
typora-root-url: ..
---

# 一. 用hexo进行初始化，文件加载不动，卡慢
在对文件执行git bash并且在git bash 中输入`hexo init`，代码如下：

     $ cd /f/Workspaces/hexo/
     $ hexo init

出现如下问题：其中部分文件下载不动
原因：github上不去，可以打开cmd窗口，输入`ping github.com`，出现`请求超时`。
参考解决方案：
在hosts("C:\Windows\System32\drivers\etc\hosts")文件末尾添加：

#github hosts
192.30.253.112 github.com
199.232.5.194 github.global.ssl.fastly.net

按照上述方案，基本可以解决此问题。
# 二. 创建文章时出现报错
输入`hexo new "title"`，返回如下结果：

![](https://image--1.oss-cn-shenzhen.aliyuncs.com/hexo error.jpg)
经查，已经使用`npm install hexo-generator-topindex --save`安装所需的插件，仍然报错。
原因：修改了scaffolds/post.md文件，我往里面添加了新参数，top:/tags:/categories:
其中把英文冒号打成了中文冒号，故导致无法通过post.md模板创建文章。

# 三.  Hexo文章上传本地图片在网页上无法显示的解决办法

## 3.1 使用坚果云编辑.md文件时

1. 找到`Hexo`下的`_config.yml`里的`post_asset_folder`，把这个选项从`false`改成`true`。
2. git bash安装插件：npm install https://github.com/7ym0n/hexo-asset-image --save（这是个修改过的插件，经测试无问题），使用这个插件来引入图片。
3. 在git bash里运行 `hexo n "title"`在/Hexo/source/_posts文件夹创建md文章，与此同时，此文件夹里会同时出现同名文件夹，如下图，将title里需要插入的图片先放入title文件夹，再进行插入文章，即可显示。
![](https://image--1.oss-cn-shenzhen.aliyuncs.com/Snipaste_2020-02-13_12-42-03.png))

## 3.2 使用Typora编辑器时

- 首先，在项目根目录下`source`文件下新建一个`assets`文件夹

- 设置Typora：Typora文件->偏好设置，图片插入的地方选择“复制到指定路径”，如图所示：

  ![](https://image--1.oss-cn-shenzhen.aliyuncs.com/Snipaste_2020-02-13_23-06-16.png)

- 设置 `格式—图像—设置图片根目录`，把目录设置到source    （每次插入图片前记得检查此项）

- 最后就可以愉快的插入图片了，格式如图：

  ![](https://image--1.oss-cn-shenzhen.aliyuncs.com/Snipaste_2020-02-13_22-41-42.png)