---
title: 理解python中的闭包与装饰器
categories: python
HTML格式实现标签样本: <center><font size=5 face="黑体">文本区</font></center>
date: 2020-03-21 17:34:45
top:
tags:
typora-root-url: ..
---



# 一、闭包

## 1.1 基本认识

闭包是python面向对象编程中一个很重要的知识点。

闭包函数有几个特点：

（1）有函数的嵌套（外函数包裹着内涵数）

（2）内涵数引用了外函数中的变量

（3）外函数的返回值是内函数的引用

以上三点不难理解。在第（2）条中，这里再强调一下，也就是说外函数的局部变量会传入到内涵中。

以下是一个普通的嵌套函数：

```python
def outer():
   x=1
   def inner():
      y=1+x
      print(y)
   inner()
   #return     函数必有返回，返回为空时可省略。

outer()
//输出结果
2
```

以下是一个闭包函数：

```python
def outer():
   x=1
   def inner():
      y=1+x
      print(y)
   return inner

outer()()
//输出结果
2
```

一般情况下，在我们认知当中，如果一个函数结束（`retrun`完之后），函数的内部所有东西都会释放掉，还给内存，局部变量都会消失。如下：


**而：**

**闭包中被内部函数引用的变量，不会因为外部函数结束而被释放掉，而是一直存在内存中，直到内部函数被调用结束。**

这是闭包函数所独有的特性。

## 1.2 重点把握

下面再看看两个对比的例子：

```python
def funcX():
    x = 2
    def funcY():
        y = 2 + x
        return y
    return funcY

print(funcX()())
#输出结果
4
```

以上的例子好理解。

下面却又是为啥呢？

```python
def funcX():
    x = 2
    def funcY():
        x = 2 + x
        return x
    return funcY

print(funcX()())
//输出错误
UnboundLocalError: local variable 'x' referenced before assignment
```

……………………………………………………………………………………………………………………………………

这是为啥呢？？？？

为啥？？？

这是由于Python语言采用的是”动态类型“技术，变量使用前不需要先声明，对变量的赋值即可自动创建变量。

当程序运行到子函数`funcY()`里面，对x进行x+2赋值时，首先，x作为被赋值对象，不管三七二十一，首先会在函数内部动态创建一个局部变量x（严格来讲是标签x），从而覆盖掉之前的父函数的x，这个x就成为了`funcY()`里面的局部变量，这时对x运行加法运算，当然会报错，因为这个x是新的x，没有被赋值，不能运算。

在其他静态语言中，也是要先定义变量，才能进行赋值运算。

解决办法是：在funcY()里面，添加一行：

```python
nonlocal x   //申明x为非局部变量
```

再举一个例子：

```python
def swap(a,b):
    a,b=b,a
    print(a,b)
a=1
b=2
swap(a,b)
//输出结果：
2 1
```

以上的例子是python中独特的交换值的代码写法，这里值得注意的是，a，b两个变量被传到swap函数里面，故在进行赋值操作时，a，b不会重新被定义。

## 1.3 全局变量与局部变量

对比举例：

```python
//例1：
def funcX():
    x = 2
    def funcY():
        return x
    funcY()

print(funcX())
//输出结果为：
None
-----------------------------------------------------------
//例2：
x = 2
def funcX():
    return x

funcX()

print(funcX())
//输出结果为：
2
```

出现以上结果的原因是：例1里面，x是在函数里定义，自带local属性，是局部变量，无法进入到 `funcY()`；而在例2里面，x自带global属性，可以进入到`funcX()`。

在看个例子：

```python
x=2
def funcX():
    x=x+2
    print(x)
funcX()
//输出结果为：
UnboundLocalError: local variable 'x' referenced before assignment
```

出现以上结果的原因是：x虽然是全局变量，但是在函数体内又对x进行了赋值操作，首先就要重新定义x，这个x为局部变量，**局部变量优先级高于全局变量**，局部变量x没有值，进行运算肯定报错。

```python
x=2
def funcX():
    print(x)
funcX()
//输出结果为：2
```

这里没有对x进行赋值，故碰到x直接用了全局变量的x，并进行输出。

## 1.3 重学赋值操作

```
x = 1
x = x+1
```

你觉得python是怎么执行上面代码的？

为了彻底弄懂python中变量与赋值的内在含义，下面有个经典的案例：

```
values=[0,1,2]
values[1]=values
print(values)
```

你觉得上面打印的结果是怎样的？

……………………………………………………………………………………………………………………………………

```
//结果如下：
[0, [...], 2]
```

出乎意料吧，结果被赋值了无数次。

**Python 没有赋值，只有引用！**

你这样相当于创建了一个引用自身的结构，所以导致了无限循环。为了理解这个问题，有个基本概念需要搞清楚。

**Python 没有「变量」，我们平时所说的变量其实只是「标签」，是引用。**

执行 `values=[0,1,2]` 的时候，python首先做的是分配一块内存空间，以创建一个列表对象 [0, 1, 2]，然后给它贴上名为 values 的标签。如果随后又执行 `values = [3, 4, 5]` ，python则会把刚才那张名为 values 的标签从前面的 [0, 1, 2] 对象上撕下来，重新贴到 [3, 4, 5] 这个对象上。

至始至终，并没有一个叫做 values 的列表对象容器存在，Python 也没有把任何对象的值复制进 values 去。

执行 `values[1]=values` 的时候，**Python 做的事情则是把 values 这个标签所引用的列表对象的第二个元素指向 values 所引用的列表对象本身。**执行完毕后，values 标签还是指向原来那个对象。列表的第2个值又指向列表本身，就这样循环嵌套。

![](/assets/Snipaste_2020-03-21_20-56-36.png)

为了避免以上的情况，可使用如下方法：

```
values=[0,1,2]
values[1]=values[:]    //利用浅复制
print(values)
//输出结果如下：
[0, [0, 1, 2], 2]
```

再回到问题的起点：

```
x = 1
x = x+1
```

先分配一块内存空间A，标签x指向内存空间A，把1放进去；再分配一块内存空间B，碰到x被赋值，x已经被声明为全局变量，就直接执行x+1运算，将其结果放进内存空间B的同时，x标签更改指向内存空间B。



# 二、装饰器

## 2.1 前话

### 2.1.1 A函数对象作为B函数形参

```python
import requests
import time

def get_html(url):
    response=requests.get(url)
    return response.text

def timer(func,url):
    start_time=time.time()
    html=func(url)
    print('网页请求时间为',time.time() - start_time)
    return html

timer(get_html,'https://www.baidu.com')
```

### 2.1.2 多形参

```python
import requests
import time

def get_html(url):
    response=requests.get(url)
    return response.text

def save_html(name,html):
    with open(name,mode='w',encoding='utf-8') as f:
        f.write(html)

def timer(func,*args,**kwargs):  
    start_time=time.time()
    html=func(*args,**kwargs)
    print('网页请求时间为',time.time() - start_time)
    return html

baidu_html=timer(get_html,'https://www.baidu.com')
timer(save_html,'baidu',baidu_html)
```

### 2.1.3 闭包封装

```python
import requests
import time

def get_html(url):
    response=requests.get(url)
    return response.text
    
def timer(func):
    def warpper(*args,**kwargs):
        start_time=time.time()
        html=func(*args,**kwargs)
        print('网页请求时间为',time.time() - start_time)
        return html
    return warpper

warp=timer(get_html)
warp('https://www.baidu.com')  //只需要改动这里网址即可实现计算不同网页打开时间
```

## 2.2 装饰器

**python装饰器本质上就是一个函数，它可以让其他函数在不需要做任何代码变动的前提下增加额外的功能，装饰器的返回值也是一个函数对象（函数的指针）**。装饰器函数的外部函数传入我要装饰的函数名字，返回经过修饰后函数的名字；内层函数（闭包）负责修饰被修饰函数。

### 2.2.1 装饰器

```python
import requests
import time

def timer(func):
    def warpper(*args,**kwargs):
        start_time=time.time()
        html=func(*args,**kwargs)
        print('网页请求时间为',time.time() - start_time)
        return html
    return warpper

@timer
def get_html(url):
    response=requests.get(url)
    return response.text

get_html('https://www.baidu.com')  
```

这里的`@`就是语法糖，代表一种特定用法，用来声明装饰器的。

意思是：执行`get_html('https://www.baidu.com')  `时，找到函数`get_html(url)`，发现其上面有`@timer`，等同于`timer(get_html(url))`。

案例：过滤url

```
urls = [
    'https://maoyan.com/board/4?offset=0',
    'https://maoyan.com/board/4?offset=10',
    'https://maoyan.com/board/4?offset=20',
    'https://maoyan.com/board/4?offset=30',
    'https://www.baidu.com',
    'https://www.sohu.com',
    'https://maoyan.com/board/4?offset=40',
    'https://maoyan.com/board/4?offset=50',
]

import requests

def filter_url(func):
    def wrapper(*args, **kwargs):
        if 'maoyan' in kwargs['url']:
            result = func(*args, **kwargs)
            print('下载成功', kwargs['url'])
            return result
        else:
            print('不是猫眼电影的网址', kwargs['url'])

    return wrapper

@filter_url
def download_maoyan(url):
    response = requests.get(url)
    return response.text

for url in urls:
    download_maoyan(url=urls)
```

### 2.2.2 带参数的装饰器

我们已经知道，下面两种是等价的：

```python
@dec
def func(...):
    ...
…………………………………………………………………………………………………………………………………………………………………………
func = dec(func)
```

我们可以把它当成是纯文本的替换，于是可以是这样的：

```
@dec(arg)
def func(...):
    ...
………………………………………………………………………………………………………………………………………………………………
func = dec(arg)(func)
```

这也就是我们看到的“带参数”的装饰器。可见，只要 `dec(arg)` 的返回值满足 “装饰器” 的定义即可。（接受一个函数，并返回一个新的函数）。

### 2.2.3 多层装饰器

以下是个带参数的多层装饰器

```python
import time

def level(level, *args1, **kwargs1):
    def timer(func):
        """第一步确定需要装饰的对象"""

        def warper(*agrs, **kwargs):
            """第二步 给对象穿衣服（装饰）"""
            start_time = time.time()
            print("当前的权限级别", level)
            print("当前的使用的对象", func)
            print("当前的参数", (agrs, kwargs))
            result = func(*agrs, **kwargs)
            time.sleep(0.0001)
            print(time.time() - start_time)
            return result

        return warper

    return timer

@level(5)
def sub(x, y=10):
    result = x - y
    return result

result=sub(3)
print(result)

//输出结果：
当前的权限级别 5
当前的使用的对象 <function sub at 0x000001AA13109400>
当前的参数 ((3,), {})
0.0030984878540039062
-7
```

### 2.2.4 类装饰器

如果说 Python 里一切都是对象的话，那函数怎么表示成对象呢？其实只需要一个类实现 `__call__` 方法即可。**__call__()是一个特殊方法，它可将一个类实例变成一个可调用对象**:

```python
class Timer:
    def __init__(self, func):
        self._func = func

    def __call__(self, *args, **kwargs):
        print('__call__方法被执行')
        result = self._func(*args, **kwargs)
        return result

@Timer
def add():
    print('add函数被执行')

add()          //Timer(add())
```

也就是说把类的构造函数当成了一个装饰器，它接受一个函数作为参数，并返回了一个对象，而由于对象实现了 `__call__` 方法，因此返回的对象相当于返回了一个函数。因此该类的构造函数就是一个装饰器。

**简单来理解：调用了Timer类，则必然会调用`__call__`方法。`__call__`就是调用执行的意思，如add()等同于`add.__call__()`。**

### 2.2.5 装饰器链

**一个python函数也可以被多个装饰器修饰。执行顺序是从近到远依次执行。**

```python
import time
class Timer:
    def __init__(self, func):
        self._func = func

    def __call__(self, *args, **kwargs):
        before = time.time()
        result = self._func(*args, **kwargs)
        after = time.time()
        print("elapsed: ", after - before)
        return result

def sum(func):
    def wrapper(*args, **kwargs):
        result=func(*args, **kwargs)
        print('sum被调用')
        return result
    return wrapper

@sum
@Timer
def add(x, y=100000):
    return x**y

results=add(3)
print(results)

//输出结果
elapsed:  0.0059871673583984375
sum被调用
1334971414230401469458914390489782292……
//从结果看，Timer装饰器先被调用，然后再调用sum
//Timer(add(3))、sum(add(3))
```






