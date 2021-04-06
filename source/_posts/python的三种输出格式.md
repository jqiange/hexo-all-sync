---
title: python的三种输出格式
categories: python
date: 2020-02-13 23:04:49
top:
tags:
typora-root-url: ..
---



# 拼接

- `+`加号拼接

  ```python
  name='zs'
  print('hello,'+ name)
  
  #输出结果：
  hello,zs
  ```

- `，`逗号拼接

  ```python
  name='zs'
  print('hello,', name)
  
  #输出结果：
  hello,zs
  ```



# 占位符方法

```python
name='zs'
print('hello,%s'%name)    #这里s%就是一个占位符，为“字符串”占位

#输出结果：
hello,zs
```

**常见占位符：**

![](https://image--1.oss-cn-shenzhen.aliyuncs.com/obytmcy9FdS91eEl3PT0.png)

**为输出设置限制条件：**

- %3s 表示输出的字符串类型最少3个字符，少于3个用空格填充
- %.5s 表示输出的字符串类型最多5个字符，超出3个会被删掉
- %3f 少于3位数，小数点后面用0填充
- %.3f 控制小数点后的位数，保留3位小数。

**下面补充一些相关知识：转义符** 

<img src="https://image--1.oss-cn-shenzhen.aliyuncs.com/kVrcUZTQzRBPT0.png" style="zoom: 80%;" />

# 格式化输出

## 第一种：

```python
name='zs'
print(f'hello,{name}')   

#输出结果：
hello,zs
```

## 第二种：

```python
name='zs'
print('hello,{}'.format(name))   

#输出结果：
hello,zs
```

- 在大括号里`{}`指定位置参数：

  ```python
  print('{1},{0},{1}'.format('abc',18))
  
  #输出结果：
  18,abc,18
  ```

- 格式控制信息还包括【填充】，【对齐】，【宽度】，【千位分隔符】，【精度】，【类型】等。

  ```python
  print('{0:30}'.format('python'))   #宽度
  #输出结果：
  python                             #python字符后面有空格的，总长度30
  
  print('{0:>30}'.format('python'))  #对齐 <，>，^分别表示左对齐，右对齐，居中
  #输出结果：
                          python
      
  print('{0:*^30}'.format('python'))  #填充
  #输出结果：
  ************python************
  
  print('{0:-^20,}'.format(123456456789))  #逗号为千位分隔符
  #输出结果：
  --123,456,456,789---
  
  print('{0:.2f}'.format(123.45645))   #精度控制
  #输出结果：
  123.46
  
  print('{0:.4}'.format('python'))   #字符分割
  #输出结果：
  pyth
  
  print('{0:.2%}'.format(3.14152))  #类型控制
  #输出结果：
  314.15%
  ```

更多用法详见官方文档：https://docs.python.org/zh-cn/3/library/string.html#format-string-syntax