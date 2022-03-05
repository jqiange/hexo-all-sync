---
title: python pip 国内镜像大全及库的安装
categories: python
date: 2020-02-13 23:13:16
top:
tags: pip
---

**在使用python第三方库的时候，经常碰到pip安装失败的情况，链接超时情况，这时可采用国内镜像网站替换的办法。**

# 国内镜像

**阿里：**

https://mirrors.aliyun.com/pypi/simple/

**清华：**

https://pypi.tuna.tsinghua.edu.cn/simple/

**中国科学技术大学：**

http://pypi.mirrors.ustc.edu.cn/simple/ 

**豆瓣：**

http://pypi.douban.com/simple/ 

# **使用办法**

## 直接在线安装

在CMD命令行下，采用如下格式

`pip install [库名] -i [镜像地址]  `

如：

```
pip install seaborn -i https://mirrors.aliyun.com/pypi/simple/
```

## 本地安装

首先在镜像源网址上下载对应的库，放在本地电脑上，再执行本地安装。

如：

```
pip install D:\Desktop\pyecharts-1.6.2-py3-none-any.whl
```

