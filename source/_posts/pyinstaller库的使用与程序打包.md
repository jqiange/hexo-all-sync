---
title: pyinstaller库的使用与程序打包
date: 2020-02-13 20:01:52
top:
tags:
categories: python

typora-root-url: ..
---

> **作用：**是第三方库，将py源码转换成无需源码的可执行文件（.exe，等）

![](https://image--1.oss-cn-shenzhen.aliyuncs.com/0.png)

> **用法：**

1. cmd命令行下

   ```
    pyinstaller  -F 文件名.py   # 即可生成 dist和 bulid 文件夹
   ```

​           -F ：在`dist`文件夹中只生成独立的打包文件

2. cmd命令行下

   ```
    pyinstaller  -F 文件名.py -i 图标文件名.ico 
   ```

​           -i : 指定打包程序使用的图标(.icon)文件，给.exe文件指定图标

3. cmd命令行下

   ```
   pyinstaller  -F 文件名.py -i 图标文件名.ico -w
   ```

​          -w:  打开可执行文件后不弹出cmd窗口
