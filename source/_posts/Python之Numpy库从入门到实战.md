---
title: Python之Numpy库从入门到实战
categories: 数据分析
date: 2020-02-13 23:15:38
top:
tags: Numpy
typora-root-url: ..
---



官方中文文档：https://www.numpy.org.cn/user/

这里使用jupyter notebook进行演示。

安装：`pip install jupyter`

启动：`jupyter notebook`

# 1. 数组的创建

## 1.1 类型转变

从列表：![](/assets/Snipaste_2020-02-14_17-39-46.png)

从元祖：![](/assets/Snipaste_2020-02-14_17-41-58.png)

从字符串：![](/assets/Snipaste_2020-02-14_17-44-32.png)

## 1.2 从头创建

### 1.2.1 一维数组

按顺序生成：<img src="/assets/Snipaste_2020-02-14_17-55-39.png" style="zoom: 80%;" />

> 使用np.random模块可随机生成

按等间距生成：<img src="/assets/Snipaste_2020-02-15_01-12-07.png" style="zoom:80%;" />

生成全为“1”数组：<img src="/assets/Snipaste_2020-02-14_17-58-56.png" style="zoom:80%;" />

生成全为“0”数组：<img src="/assets/Snipaste_2020-02-14_18-02-02.png" style="zoom:80%;" />

对角线全为“1”数组：<img src="/assets/Snipaste_2020-02-14_18-05-21.png" style="zoom:80%;" />

注意：默认生成浮点型数值，可在括号里加参数`dtype=int`，使得生成整型数值。

归纳为以下：

![](/assets/Snipaste_2020-02-14_18-08-33.png)

### 1.2.2 多维数组



- `.reshape() `的使用：

<img src="/assets/Snipaste_2020-02-14_19-40-35.png" style="zoom:80%;" />

使用`.reshape()`方法可将n维数组变成任意指定形状的数组，但是元素个数要对的上。

**将n维数组变成一维：**

<img src="/assets/Snipaste_2020-02-14_20-14-20.png" style="zoom:80%;" />

**将一维行数据数据变成列数据：**

```python
>>> s=np.arange(5)
array([0, 1, 2, 3, 4])
>>> s.reshape(5,1)
array([[0],
       [1],
       [2],
       [3],
       [4]])
```



`.reshape()`和`.resize()`区别：前者不改变原形状，后者改变。

`.flatten()`和`.ravel()`区别：执行命令后再进行赋值操作，前者不改变元素，后者改变（此用法不多）。

- `size()`的使用：

<img src="/assets/Snipaste_2020-02-14_21-04-13.png" style="zoom:80%;" />

### 1.2.3 np.random模块

| 方法                                | 使用说明                                |
| ----------------------------------- | --------------------------------------- |
| `np.random.random((m,n))`           | 随机生成(0,1)之间的数组，m行n列         |
| `np.random.rand(m,n)`               | 随机生成 [0,1)之间的数组，m行n列        |
| `np.random.randint(x,y,size=(m,n))` | 随机生成整型数组，数字(x,y)之间，m行n列 |
| `np.random.uniform(x,y,size=(m,n))` | 随机生成浮点数组，数字[x,y)之间，m行n列 |
| `np.random.choice(data)`            | 随机采样生成数组，从data中              |
| `np.random.shuffle(data)`           | 随机按行打乱数组，从data中，改变原数组  |
| `np.random.permutation(data)`       | 随机按行打乱数组，从data中，不改原数组  |
| `np.random.seed()`                  | 调控随机数的生成，根据种子值            |
| `np.random.randn(m,n)`              | 随机生成标准正态分布数组，符合N(0,1)    |
| `np.random.normal(μ,σ,size=(m,n))`  | 随机生成正态分布数组，符合N(μ,σ)        |
| `np.random.poisson(lam,size=(m,n))` | 随机生成泊松分布数组，事件发生率lam     |



以上均可很据括号里传入维度的不同，可以创建1~N维数组。

## 1.3 数组属性

| 属性        | 使用说明                                                     |
| ----------- | ------------------------------------------------------------ |
| `.ndim`     | 秩，即数据轴的个数                                           |
| `.shape`    | 数组的维度                                                   |
| `.size`     | 元素的总个数                                                 |
| `.dtype`    | 数据类型                                                     |
| `.itemsize` | 数组中每个元素的字节大小                                     |
| `.T`        | 数组的转置，即行列进行交换 / `.transpose`作用一样，但它会改变原数组 |

使用以上方法不需要加括号。

举一个例子：<img src="/assets/Snipaste_2020-02-14_19-59-50.png" style="zoom:80%;" />



# 2. 数组的操作

## 2.1 索引

### 2.1.1 普通索引

位置索引：

<img src="/assets/Snipaste_2020-02-14_20-28-10.png" style="zoom:80%;" />

布尔索引：

<img src="/assets/Snipaste_2020-02-14_20-49-58.png" style="zoom:80%;" />



### 2.1.2 花哨索引

选取数据：

<img src="/assets/Snipaste_2020-02-15_00-25-31.png" style="zoom:80%;" />

选择块：

<img src="/assets/Snipaste_2020-02-15_00-40-09.png" style="zoom:80%;" />



<img src="/assets/Snipaste_2020-02-15_12-07-29.png" style="zoom:80%;" />

调换行：

<img src="/assets/Snipaste_2020-02-15_00-31-45.png" style="zoom:80%;" />

## 2.2 切片

连续的切片：

<img src="/assets/Snipaste_2020-02-14_20-41-33.png" style="zoom:80%;" />

不连续的切片：

<img src="/assets/Snipaste_2020-02-14_20-46-01.png"  style="zoom:80%;" />

## 2.3 值的替换

根据索引赋值

<img src="/assets/Snipaste_2020-02-14_21-01-18.png" style="zoom:80%;" />

## 2.4 数组拼接

![](/assets/Snipaste_2020-02-14_21-36-03.png)

## 2.5 数组切割

### 2.5.1 普通切割

![](/assets/Snipaste_2020-02-14_22-03-42.png)

### 2.5.2 复杂切割

![](/assets/Snipaste_2020-02-14_22-06-11.png)

## 2.6 数组运算

### 2.6.1 通用函数

![](/assets/Snipaste_2020-02-15_00-44-07.png)



![](/assets/Snipaste_2020-02-15_00-44-29.png)

**注意：这里的逻辑运算如逻辑与np.logical_and()是进行多元运算的。括号里传入的数组必须形状相同。**

![](/assets/Snipaste_2020-02-15_00-45-07.png)

NAN安全版本：为了应对数组里的缺失值`np.nan`的。如下：

<img src="/assets/Snipaste_2020-02-15_12-26-15.png" style="zoom:80%;" />

### 2.6.2 布尔数组函数

![](/assets/Snipaste_2020-02-15_00-55-07.png)

对传入的数组进行是否有空值判断。传入的数组也可以为布尔数组。只能进行一元判断。

## 2.7 数组排序

### 2.7.1 普通排序

![](/assets/Snipaste_2020-02-15_01-03-47.png)

如要 降序 排列，只需传入负号即可：`np.sort(-arr)`。

### 2.7.2 排序并返回唯一值

<img src="/assets/Snipaste_2020-02-15_01-23-06.png" style="zoom:80%;" />

对传入的数组中的元素排序，并返回对应元素出现的个数。

## 2.8 数组广播机制

**如果两个数组的后缘维度（trailing dimension，即从末尾开始算起的维度）的轴长度相符或其中一方的长度为1，则认为他们是广播兼容的。广播会在缺失和（或）长度为1的维度上进行。**

举例说明：

<img src="/assets/Snipaste_2020-02-15_01-29-58.png" style="zoom:80%;" />

# 3. 文件操作

## 3.1 文件保存

```
np.savetxt(frame, array, fmt='%.18e', delimiter=None)
```

* **frame** : 文件、字符串或产生器，可以是.gz或.bz2的压缩文件    如 "example.csv"
* **array** : 存入文件的数组
* fmt : 写入文件的格式，例如：%d  %.2f   %.18e                             可以省略
* delimiter : 分割字符串，默认是任何空格                                     根据需要是否要指定

## 3.2 读取文件

```
np.loadtxt(frame, dtype=np.float, delimiter=None, unpack=False)
```

* **frame**：文件、字符串或产生器，可以是.gz或.bz2的压缩文件。
* dtype：数据类型，可选。
* delimiter：分割字符串，默认是任何空格。
* skiprows：跳过前面x行。
* usecols：读取指定的列，用元组组合。
* unpack：如果True，读取出来的数组是转置后的。

**保存和读取文件举例：**

![](/assets/Snipaste_2020-02-15_16-26-28.png)

注意这里的 `skiprows=1` 的意思是忽略第一行数据。

## 3.3 numpy独有存储格式

1. 存储：`np.save('fname.np',array)`或`np.savez('fname.npz',array)`。后者`fname.npz`是经过压缩的。
2. 加载：`np.load(fname)`。

# 4. 补充知识点

## 4.1 Axis理解

方便大家理解，可以认为**`axis=0`代表的是行**，**`axis=1`代表的是列**。但其实不是这么简单理解的。这里具体解释一下这个`axis`轴的概念。

简单来说， **最外面的括号代表着 axis=0，依次往里的括号对应的 axis 的计数就依次加 1**：

![](/assets/1-1581749184285.png)

 **操作方式：如果指定轴进行相关的操作，那么他会使用轴下的每个直接子元素的第0个，第1个，第2个...分别进行相关的操作。**

**二维的数组求和：**

```python
>>>x = np.array([[0,1],
                 [2,3]])
>>>x.sum(axis=0)
array([2, 4])
>>>x.sum(axis=1)
array([1, 5])
```

按照`axis=0`的方式进行相加，那么就会把最外面轴下的所有直接子元素中的第0个位置`x[0][0]`与`x[1][0]`

进行相加，第1个位置`x[0][1]`与`x[1][1]`进行相加...依此类推。

按照`axis=1`的方式进行相加，那么就会把轴为1里面的元素拿出来进行求和，`x[0][0]`与`x[0][1]`进行相加，`x[1][0]`与`x[1][1]`进行相加...依此类推。

**用`np.delete`在`axis=0`和`axis=1`两种情况下删除元素：**

```python
>>>x = np.random.randint(0,10,size=(3,5))
>>> np.delete(x,0,axis=0)
array([[0, 4, 2, 5, 2],
       [2, 2, 1, 0, 8]])         #删除了行
```

`np.delete`是个例外。我们按照`axis=0`的方式进行删除，那么他会首先找到最外面的括号下的直接子元素中的第0个，然后删掉，剩下最后一行的数据。

**三维数组：**

<img src="/assets/2-1581750404175.png" style="zoom:80%;" />

按照`axis=0`的方式进行相加：![](/assets/3-1581750466746.png)

按照`axis=1`的方式进行相加：![](/assets/4-1581750497037.png)

## 4.2 NAN和INF值处理

**意义解释：**

1. `NAN`：`Not A number`，不是一个数字的意思，但是他是属于浮点类型的，所以想要进行数据操作的时候需要注意他的类型。
2. `INF`：`Infinity`，代表的是无穷大的意思，也是属于浮点类型。`np.inf`表示正无穷大，`-np.inf`表示负无穷大，一般在出现除数为0的时候为无穷大。比如`2/0`

**特点：**

1. NAN和NAN不相等。比如`np.NAN != np.NAN`这个条件是成立的。
2. NAN和任何值做运算，结果都是NAN。

**删除缺失值：**

<img src="/assets/Snipaste_2020-02-15_15-36-25.png" style="zoom:80%;" />

**替换缺失值：**

![](/assets/Snipaste_2020-02-15_16-34-56.png)

打开表格`scores.csv`可看到如下表格：

| 数学 | 英语 |
| ---- | ---- |
| 59   | 89   |
| 90   | 32   |
| 78   | 45   |
| 34   | nan  |
| nan  | 56   |
| 23   | 56   |



替换缺失值为0：

![](/assets/Snipaste_2020-02-15_16-37-04.png)

## 4.3 深浅拷贝

在操作数组的时候，它们的数据有时候拷贝进一个新的数组，有时候又不是。这经常是初学者感到困惑。下面有三种情况：

**不拷贝：**

如果只是**简单的赋值**，那么不会进行拷贝。示例代码如下：

```python
a = np.arange(12)
b = a #这种情况不会进行拷贝
print(b is a) #返回True，说明b和a是相同的
```

**View或者浅拷贝：**

有些情况，会进行**变量的拷贝**，但是他们所**指向的内存空间都是一样的**，那么这种情况叫做浅拷贝，或者叫做`View(视图)`。比如以下代码：

```python
a = np.arange(12)
c = a.view()
print(c is a) #返回False，说明c和a是两个不同的变量
c[0] = 100
print(a[0]) #打印100，说明对c上的改变，会影响a上面的值，说明他们指向的内存空间还是一样的，这种叫做浅拷贝，或者说是view
```

**深拷贝：**

将之前数据**完完整整的拷贝一份放到另外一块内存空间中**，这样就是两个完全不同的值了。示例代码如下：

```python
a = np.arange(12)
d = a.copy()
print(d is a) #返回False，说明d和a是两个不同的变量
d[0] = 100
print(a[0]) #打印0，说明d和a指向的内存空间完全不同了。
```

**图例说明：**

![](/assets/Snipaste_2020-02-15_16-54-54.png)



像之前讲到的`flatten`和`ravel`就是这种情况，`ravel`返回的就是View，而`flatten`返回的就是深拷贝。



# 5. Numpy实际应用

## 5.1 图像处理

以下简单举例Numpy数组在图像处理领域的应用：

**读取图片并将其转化成数组数据：**

<img src="/assets/Snipaste_2020-02-15_17-38-39.png" style="zoom: 80%;" />

**处理数组数据并还原成图片：**

<img src="/assets/Snipaste_2020-02-15_17-44-07.png" style="zoom:80%;" />



图像处理库Pillow(PIL)更多用法参考：https://www.cnblogs.com/yhjoker/p/10773444.html

官方文档：https://pillow.readthedocs.io/en/stable/index.html