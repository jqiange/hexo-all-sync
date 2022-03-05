---
title: Markdown文本编辑技巧
categories: 工具
HTML格式实现标签样本: <center><font size=5 face="黑体">文本区</font></center>
date: 2020-02-18 16:31:40
top:
tags: Markdown
typora-root-url: ..
---





**Markdown**是一种可以使用普通文本编辑器编写的标记语言，通过类似HTML的标记语法，它可以使普通文本内容具有一定的格式。但是它本身是不支持众多富文本功能的。

- **此文章的主要目的是：给markdown文本提供更多的编辑表现可能。**

# 1.开始前

## **认识网页结构**

网页一般由三部分组成：分别是：

- **HTML 超文本标记语言**，是整个网页的结构，相当于整个网页的框架

  常见的标签如下：

  ```
  <html>...</html>          表示标记中间的元素是网页
  <head>...</head>          用来申明使用的脚本语言，以及网页传输时使用的方式
  <body>...</body>          表示用户可见的内容
  <div>...</div>            表示框架
  <p>...</p>                表示段落
  <h1>...</h1>              表示标题
  <li>...</li>              表示列表
  <ul>...</ul>              表示无序列表
  <ol>...</ol>              表示有序列表
  <img>...</img>            表示图片
  <a href="   ">...</a>     表示超链接 
  ```

- **CSS 层叠样式表**，用来定义外观
- **JScript**，表示功能，交互的内容和各种特效都在里面。

**可以这样理解：如果用人体来比喻，HTML是人体的骨架，并且定义了嘴巴，眼睛，耳朵要长在那里，这是人的基本要素。CSS是人的外观细节，如嘴巴长什么样子，眼睛是单还是双眼皮，皮肤是黑还是白等。JScript表示人的技能，如会不会跳舞，唱歌，演奏乐器等。**

# 2. 字体，字号和颜色

```
<font color=#0099ff size=7 face="黑体">这是我的显示效果</font>
```

<font color=#0099ff size=7 face="黑体">这是我的显示效果</font>

颜色对照表https://blog.csdn.net/weixin_37998647/article/details/79428290

# 3. 背景色设置

```
<table><tr><td bgcolor=#D1EEEE>这是我的背景色</td></tr></table>
```

<table><tr><td bgcolor=#D1EEEE>这是我的背景色</td></tr></table>

# 4. 居中，左右对齐

```
<center><font size=5 face="黑体">这是我的显示效果</font></center>                    居中
<p align='left'><font size=5 face="黑体">这是我的显示效果</font></p align='left'>    左对齐
<p align='right'><font size=5 face="黑体">这是我的显示效果</font></p align='right'>  有对齐
```

<center><font size=5 face="黑体">这是我的显示效果</font></center>
<p align='left'><font size=5 face="黑体">这是我的显示效果</font></p align='left'>

<p align='right'><font size=5 face="黑体">这是我的显示效果</font></p align='right'>

# 5. 矩阵形式输入

**首先通过编辑器插入公式块，再添加内容**。

**不带括号**

```
$$
\begin{matrix}
1&2\\3&4\\5&6
\end{matrix}
```

![不带括号](https://image--1.oss-cn-shenzhen.aliyuncs.com/不带括号.jpg)

**带中括号**

```
$$
\left[\begin{matrix}
1&2\\3&4\\5&6
\end{matrix}\right]
$$

```

![带中括号](https://image--1.oss-cn-shenzhen.aliyuncs.com/带中括号.jpg)

**带大括号**

```
$$
\left\{\begin{matrix}
1&2\\3&4\\5&6
\end{matrix}\right\}
$$
```

![带大括号](https://image--1.oss-cn-shenzhen.aliyuncs.com/带大括号.jpg)

# 6. 更多

参考https://blog.csdn.net/silver1225/article/details/89036250?utm_source=distribute.pc_relevant.none-task





