---
title: CSS选择器与Xpath数据提取
categories: 爬虫
HTML格式实现标签样本: <center><font size=5 face="黑体">文本区</font></center>
date: 2020-02-26 19:07:35
top:
tags: CSS-Xpath
typora-root-url: ..
---



# 1、CSS选择器

## 1.1 基本选择器

这里介绍标签选择器，类选择器，ID选择器。

一段html网页文件，都是包含着众多标签的，如：

```
<html>...</html>
<p>...</p>
<span>...</span>
etc...
```

我们可以直接选择标签以提取标签里面的内容。

看看如何使用：

```python
import requests
from parsel import Selector

html=requests.get(url).text
selector=Selector(html)

res=selector.css('XX')  //指标签名。如p,span等，返回带标签的列表
res=selector.css('.XX')  //指定类名。如class="XX"，style="XX"，返回带类名的列表
res=selector.css('#XX')  //指定id名。如id="XX"，返回带类名的列表


print(res.get())
print(res.extract())  //.get()和.extract()是一样的
print(res.getall)     //取到'标签/类/id名'所有的内容
```

属性提取器`::`

如`selector.css('XX::text')`，可提取XX里的文本内容。

`selector.css('XX::attr(属性名称)')`，可提取指定的属性内容

多层提取

`selector.css('XX.YY::text')`，意思是提取XX标签下的YY属性里的文本内容。

### 举个例子

有如下`maoyan.html`文件，需要提取里面的电影名称，演员，上映时间等信息。

![](https://image--1.oss-cn-shenzhen.aliyuncs.com/image-20210406155343374.png)

```python
from parsel import Selector
sel=Selector(html)

name=sel.css('p.name').css('a::text').getall()   
//首先提取p便签，属性为name内容，再进一步里面提取a标签里的文本

star=sel.css('p.star::text').getall()   
//首先提取p便签，属性为star内容，再提取文本

releasetime=sel.css('p.releasetime::text').getall()
//首先提取p便签，属性为releasetime内容，再提取文本
```

## 1.2 进阶选择器

### 伪类选择器

`:` 伪类选择器，通过索引选择。

![](https://image--1.oss-cn-shenzhen.aliyuncs.com/Snipaste_2020-02-27_19-17-23.png)

```python
dd5=selector.css('div.main dd:nth-child(4)')   //选择第五个标签内容
dd_last=selector.css('div.main dd:last-child') //选择最后一个dd标签
```

更多用法：https://www.runoob.com/css/css-pseudo-classes.html

# 2. XPath选择器

## 2.1 XML介绍

XML称为可扩展标记语言，XML是互联网数据传输的重要工具，它可以跨越互联网任何的平台，不受编程语言和操作系统的限制，可以说它是一个拥有互联网最高级别通行证的数据携带者。非常类似HTML。

HTML 和 XML的区别在于HTML主要用来显示数据，XML是用来传输数据。

XML都是标签闭合的。例如：`<bookstore> ... </bookstore> `成对出现。

- 正则：提取文字


- css选择器：提取网页


- xpath：xml节点


```python
<?xml version="1.0" encoding="utf-8"?>

<bookstore>

  <book category="奇幻">
    <title lang="ch">冰与火之歌</title>
    <author>乔治 马丁</author>
    <year>2005</year>
    <price>365.00</price>
  </book>

  <book category="童话">
    <title lang="ch">哈利波特与死亡圣器</title>
    <author>J K. 罗琳</author>
    <year>2005</year>
    <price>48.98</price>
  </book>
```

## 2.2 xpath

XPath (XML Path Language) 是一门在 XML 文档中查找信息的语言，可用来在 XML/HTML 文档中对元素和属性进行遍历，并提取相应元素。

**使用xpath提取元素，需要有标签节点才能实现！**

### 2.2.1 提取规则

| 符号                          | 含义                                                  |
| ----------------------------- | ----------------------------------------------------- |
| //                            | 当前节点下任意子孙节点下的内容                        |
| ./                            | 当前节点下直接子节点的内容                            |
| .//                           | 当前节点下任意子孙节点的内容                          |
| /                             | 选取直接子节点                                        |
| .                             | 当前节点                                              |
| ..                            | 当前节点的父节点                                      |
| [  ]                          | 中括号里指定需要选择的属性                            |
| @                             | 选择属性                                              |
| [@class='name']               | 选择属性名为class，且值为‘name’的内容                 |
| \|                            | 选择左右两个节点                                      |
| *                             | 替换任何标签                                          |
| book[1]                       | 选取第一个book元素                                    |
| //li[contains(@attrib,value)] | 选取任意节点下有li标签且包含attrib和value属性的内容   |
| //li[text()="xxx"]            | 选取任意节点下有li标签且其文本为xxx的内容             |
| //li/text()                   | 选取任意节点下li标签下的文本内容                      |
| //li/a/@href                  | 选取任意节点下li标签的子标签a属性名为href对应的属性值 |

### **举个例子**

**批量提取：**url=http://music.taihe.com/artist/2517

![](https://image--1.oss-cn-shenzhen.aliyuncs.com/Snipaste_2020-02-28_13-39-58.png)



```python
import requests
from parsel import Selector

response=requests.get(url,headers=headers)
response.encoding='utf-8'
html=Selector(response.text)
lis=html.xpath('//div[@class="song-list-wrap"]/ul/li') //首先取到li标签，进行粗提取
print(lis)
//输出结果
 15    //一个页面有15首歌曲，故也应该有15个li标签
```

再分析`li`标签内的内容，进行具体信息提取：

![](https://image--1.oss-cn-shenzhen.aliyuncs.com/Snipaste_2020-02-28_13-44-56.png)

上图截取了第一个`li`标签里的所有内容

```python
for li in lis:
    name=li.xpath('.//span[@class="songname"]/a/text()').getall()[0].strip()
    print(name)    
//打印歌曲名字
//其他内容提取方式相同...
```

# 3、异常处理

**css选择器遇见含空格类提取问题：**

**response.css(".position-label.clearfix .labels::text").extract() 即在position-label clearfix中的"空格"换成" . "** 

