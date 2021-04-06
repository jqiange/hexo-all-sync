---
title: python之Matplotlib可视化
categories: 数据分析
HTML格式实现标签样本: <center><font size=5 face="黑体">文本区</font></center>
date: 2020-02-20 20:28:44
top:
tags: Matplotlib
typora-root-url: ..
---



# 1. Matplotlib库入门

`Matplotlib`是一个`Python`的`2D`绘图库，通过`Matplotlib`，开发者可以仅需要几行代码，便可以生成折线图，直方图，条形图，饼状图，散点图，箱线图等，是`python`中最为常见的平面绘图库。

库安装：

```python
//CMD命令行下
pip install matplotlib
```

库导入：

```python
import matplotlib.pyplot as plt
plt.rcParams['font.sans-serif'] = ['SimHei'] // 指定默认字体
plt.rcParams['axes.unicode_minus'] = False  //解决图像中出现负号'-'显示为方块异常的问题
```

<font size=5 face="黑体">**官方文档：**</font>https://matplotlib.org/api/pyplot_summary.html

# 2.  线图

**主要包括折线图，曲线图。**

## 2.1 基本绘图函数

```python
//基本绘图函数
plt.plot(x, y, 'xxx', label=, linewidth=, marker= )
```

① `x`：位置参数，横坐标，可迭代对象（若省略，可自动生成0-n个数与y的长度对应）。

② `y`：位置参数，纵坐标，可迭代对象。

③ `'XXX'`：样式参数，可指定**线条样式**，**颜色**。

**线条样式linestyle**有实线，虚线，点线等，如`linestyle="--"`，linestyle=可以省略，具体表示方法如下：

<img src="https://image--1.oss-cn-shenzhen.aliyuncs.com/5-rwerweq ty11.png" style="zoom:100%;" />

**线条颜色color**表示方法有：color= `'r'`|	`'red'`	|`'#000100'`|	`(0.1, 0.5, 0.4)`，四种表示方法，其中前三种color=可以省略(color可以简写为c)。

④ `lable`：关键字参数，设置图例，需要调用 plt 或子图的 legend 方法，需要添加一行`plt.legend()`，图例标签才会显示。

⑤ `linewidth`：关键字参数，设置线的粗细。

⑥ `marker`：设置关键点显示方式。如`marker='o'`，设置关键点以实心圆点显示，`markersize=10`可指定圆点大小为10，`markerfacecolor='k'`，可设置关键点的颜色。

## 2.2 细节设置

```python
plt.xlabel('x 轴')           //设置x轴标签
plt.ylabel('y 轴')           //设置y轴标签
plt.title('折线图')          //设置图像标题
plt.xticks(range(0,10,2))　　//设置x轴刻度属性
plt.yticks(range(0,10,2))   //设置y轴刻度属性
plt.ylim(-2, 2)             //设置y轴的上下限
plt.grid()                  //设置网格背景
plt.legend()                //显示图例
plt.figure(figsize=(15,5))  //设置画布大小，要在画图之前设置，不然不起作用
plt.style.use("dark_background")  //设置绘图风格
plt.axis('off')                   //设置不显示坐标轴
plt.show()                  //显示图
```

**关于轴刻度：**

`plt.xticks(_xticks, [labels], *kwargs)`可设置【应当放置刻度的位置列表】，【在给定*位置*放置的显式标签的列表】，【控制标签的外观】（如rotation=45，以45度旋转显示；fontsize=10，设置刻度字体大小为10）。`_xticksy`与`[labels]`一一对应。y轴同理。

**关于画布：**

`plt.figure(num=None, figsize=None, dpi=None, facecolor=None, edgecolor=None, frameon=True)`来实现。 其中`num`是图的编号，`figsize`的单位是英寸，`dpi`是每英寸的像素点，`facecolor`是图片背景颜色，`edgecolor`是边框颜色，`frameon`代表是否绘制画板。

**关于图例：**

`legend((line1, line2, line3), ('label1', 'label2', 'label3'),*kwargs)`，前者指定绘制的图线，后者指定对应的样式。如都为空，则按照默认的显示。

其他参数`*kwargs`：位置`loc=`，值有：角落位置【`'upper left', 'upper right', 'lower left', 'lower right'`】，边缘中心【`'upper center', 'lower center', 'center left', 'center right'`】，中心【` 'center’`】。默认最佳位置【`'best'`】；位置`bbox_to_anchor=`，可传入2元组（横纵坐标）或4元浮点数（x, y, width, height）。

字体大小`fontsize=`

图例中元素排列方式`ncol=`，可指定按行排列。默认值为1，上下排列。

**关于绘图风格：**

`matplotlib`图片默认内置了几种风格。我们可以通过`plt.style.available`来查看内置的所有风格。

**关于坐标轴设置：**

`axis()`：返回当前的坐标轴范围 [xmin, xmax, ymin, ymax]

`axis(v)`：传参用来设置坐标轴范围

`axis('off')`：不显示坐标轴和坐标轴名称

`axis('equal')`：提高某一个轴的范围以保持图表大小及比例不变

`axis('scaled')`：设置轴的数值范围不变

`axis('tight')`：减少两个轴的数值范围，并尽量让数据居中

`axis('image')`：将图表比例按照图片的比例进行调整（缩放）

`axis('square')`：将图表变成正方形，并确保 (xmax-xmin) 与 (ymax-ymin) 相同

`axis('auto')`：恢复自动范围



### 举个例子

```python
import matplotlib.pyplot as plt
import numpy as np
plt.rcParams['font.sans-serif'] = ['SimHei'] 
plt.rcParams['axes.unicode_minus'] = False  

x1=np.linspace(0,25)
y1=np.sin(x)
plt.plot(x1,y1,marker='o',label='sin函数') 
plt.plot(range(20),[np.random.randint(0,5) for x in range(20)],'r:',linewidth=5,label='折线图')   //'r:'颜色和线型写在一块

plt.xlabel('x 轴')           
plt.ylabel('y 轴')           
plt.title('折线图')
plt.xticks(range(0,20,2))
plt.legend()
plt.grid()                  
plt.figure(figsize=(30,10))
plt.show()   //有时可以省略
```

<img src="https://image--1.oss-cn-shenzhen.aliyuncs.com/int i78 9t 8r00dex.png"  style="zoom:120%;" />

注意：对于有时候画图后不显示图，而需要show的时候，为了避免繁琐的show，可以在导入库之后加入一行代码`%matplotlib inline`就可以plot画图后直接输出图。

## 2.3 多图合一

**绘制多个子图的时候，我们可以使用`plt.subplot`或`plt.subplots`来实现。**

**1)`plt.subplot`** 

```python
plt.subplot(221)                           //2行2列第1个图
plt.plot(np.arange(10),c='r')
plt.subplot(222)                           //2行2列第2个图
plt.plot(np.sin(np.arange(10)),c='b')
plt.subplot(223)                           //2行2列第3个图
plt.plot(np.cos(np.arange(10)),c='y')
plt.subplot(224)                           //2行2列第4个图
plt.plot(np.tan(np.arange(10)),c='g')
```

![](https://image--1.oss-cn-shenzhen.aliyuncs.com/subplot1.png)

**2) 也可以使用`fig,axs=plt.subplots(rows,cols,figsize=,*kwargs)`来绘制多个图形，可以指定行列数，画布大小。返回值是一个元组，其中的`fig`参数是`figure`对象，`axs`是`axes`对象的`array`。**

```python
figure,axes = plt.subplots(2,2,sharex=True,sharey=True)
axes[0,0].plot(np.sin(np.arange(10)),c='r')
axes[0,1].plot(np.cos(np.arange(10)),c='b')
axes[1,0].plot(np.tan(np.arange(10)),c='y')
axes[1,1].plot(np.arange(10),c='g')
```

![](https://image--1.oss-cn-shenzhen.aliyuncs.com/subplot2.png)

关于返回值，也可以这样接收：

```
figure,[ax1,ax2,ax3,ax4]= plt.subplots(2,2)
```

再将坐标系ax1-4通过画图函数传参交给所需的图，如

```python
bar1=plt.bar(x, height, ax=ax1)
hist2=plt.hist(x, bins, ax=ax2)
scatter3=plt.scatter(x, y, ax=ax3)
bo33=plt.boxplot(x,ax=ax4)
```

## 2.4 设置注释文本

在图形中的某个点标记或者注释一下，我们可以使用`plt.annotate(text,xy,xytext,arrowprops={})`来实现，其中`text`是注释的文本，`xy`是需要注释的点的坐标，`xytext`是注释文本的坐标，`arrowprops`是箭头的样式属性。

```python
ax = plt.subplot(111)           
x = np.arange(0.0, 5.0, 0.01)
y = np.cos(2*np.pi*x)
plt.plot(x, y,linewidth=2)

//设置注释文本
plt.annotate('local max', xy=(2, 1), xytext= (3,1.5),
                  arrowprops=dict(facecolor='black', shrink=0.05))

plt.ylim(-2, 2)  //设置y轴的上下限
```

![](https://image--1.oss-cn-shenzhen.aliyuncs.com/inuitiotuttdex.png)

## 2.5 图片保存

```python
 plt.savefig(path)
```

# 3. 条形图

## 3.1 基本条形图

```python
//基本绘图函数
 plt.bar(x, height, width=, bottom=, align=, color=, *kwargs)
```

① `x`：位置参数，条形图x轴的坐标点。

② `height`：位置参数，条形图y轴的坐标点。

③ `width`：条形宽度，取值在0-1之间。默认0.8。

④ `bottom`：条形的起始位置。默认为0。

⑤ `align`：条形的中心位置。默认中心'center'。还有边缘'edge'，靠边对齐，具体靠右边还是靠左边，看`width`的正负。

⑥ `color`：条形的颜色，“r","b","g","#123465"，默认“b"

⑦ `*kwargs`：包括边框颜色`edgecolor`，变宽`linewidth`，下标的标签`tick_label`，y轴使用科学计算法表示`log`，是竖直条还是水平条`orientation`(竖直：`"vertical"`，水平条：`"horizontal"`)，亮度深浅调整`alpha=0-1`。

**细节设置等其他设置同2.2。**

### 举个例子

```python
 movies = {
    "流浪地球":40.78,
    "飞驰人生":15.77,
    "疯狂的外星人":20.83,
    "新喜剧之王":6.10,
    "廉政风云":1.10,
    "神探蒲松龄":1.49,
    "小猪佩奇过大年":1.22,
    "熊出没·原始时代":6.71
}
 x = np.arange(len(movies))
 y=list(movies.values())

 plt.bar(x,y,width=0.3,align='edge',color='r')

 _xticks= np.arange(len(movies))    
 plt.xticks(_xticks,list(movies.keys()),rotation=45)  
//设置x轴要显示的刻度数，以及以movies.keys()文本显示，为了避免文本刻度拥挤，设置旋转显示

 _yticks=range(0,50,5)    //设置y轴显示刻度数
 plt.yticks(_yticks,['%d亿'%i for i in _yticks]) //  显示在y轴上，并在刻度上加上'亿'
 plt.grid()
 plt.figure(dpi=80)
```

<img src="https://image--1.oss-cn-shenzhen.aliyuncs.com/inuyuytresdex.png" style="zoom:120%;" />

## 3.2 横向条形图

```
//基本绘图函数
 plt.barh(y, width, height=, left=, align=, color=, *kwargs)
```

① `y`：位置参数，条形图y轴的坐标点。

② `width`：位置参数，条形图在`x`轴上的值。

③ `height`：条形宽度，取值在0-1之间。默认0.8。

④ `left`：条形的基线起始位置。默认为0。

⑤ `align`：条形的中心位置。默认中心'center'，还有边缘'edge'

⑥ `color`：条形的颜色，“r","b","g","#123465"，默认“b"

⑦ `*kwargs`：包括边框颜色`edgecolor`，边宽`linewidth`，下标的标签`tick_label`，y轴使用科学计算法表示`log`，是竖直条还是水平条`orientation`(竖直：`"vertical"`，水平条：`"horizontal"`)

**细节设置等其他设置同2.2。**

## 3.3 分组条形图

基本绘图函数同3.1和3.2。拿基本条形图（竖直条形图）来讲，只是x轴一个区间要分成几部分，放置同类型的条形图。

### 举个例子1

```
movies = {
    "流浪地球":[2.01,4.59,7.99,11.83,16],
    "飞驰人生":[3.19,5.08,6.73,8.10,9.35],
    "疯狂的外星人":[4.07,6.92,9.30,11.29,13.03],
    "新喜剧之王":[2.72,3.79,4.45,4.83,5.11],
    "廉政风云":[0.56,0.74,0.83,0.88,0.92],
    "神探蒲松龄":[0.66,0.95,1.10,1.17,1.23],
    "小猪佩奇过大年":[0.58,0.81,0.94,1.01,1.07],
    "熊出没·原始时代":[1.13,1.96,2.73,3.42,4.05]
}
```

需要将上面数据画成分组条形图，每部电影有5个数据，首先明确怎么画图：电影名称作为横坐标，需设置`len(movies.keys())`共8个坐标，每个坐标上有5个数据条，先统一画第一个坐标点的第一个数据条，再画第二个数据条……（内循环），画完之后画第二个坐标点的数据，以此循环（外循环）。

具体解法如下：

```python
//预处理
 width = 0.75                     //设置每部电影总条形图的宽度
 bin_width = width/5              //将宽度分成5部分，以用于5个数据
 movie_pd = pd.DataFrame(movies)  //将电影数据变成数表，便于索引取值
 ind = np.arange(0,len(movies))   //电影数为x轴坐标，分为5段
```

第一种解法：

```python
 first_day = movie_pd.iloc[0]   //(ind-bin_width*2)每段的第1个位置
 plt.bar(ind-bin_width*2,first_day,width=bin_width,label='第一天') //绘制每部电影第1天数据

 second_day = movie_pd.iloc[1]   //(ind-bin_width)每段的第2个位置
 plt.bar(ind-bin_width,second_day,width=bin_width,label='第二天') //绘制每部电影第2天数据

 third_day = movie_pd.iloc[2]    // ind每段的第3个位置
 plt.bar(ind,third_day,width=bin_width,label='第三天')      //绘制每部电影第3天数据

 four_day = movie_pd.iloc[3]     //(ind+bin_width)每段的第4个位置
 plt.bar(ind+bin_width,four_day,width=bin_width,label='第四天')   //绘制每部电影第4天数据

 five_day = movie_pd.iloc[4]    //(ind+bin_width*2)每段的第5个位置
 plt.bar(ind+bin_width*2,five_day,width=bin_width,label='第五天')  //绘制每部电影第5天数据

 plt.xticks(ind,list(movies.keys()),fontsize=20)   //设置x轴刻度标签
 plt.yticks(list(range(0,18,2)),fontsize=20)       //设置y轴刻度标签
 plt.ylabel('单位：亿',fontsize=25)
 plt.legend(bbox_to_anchor=(0.8,0.8),fontsize=20)  //指定图例的位置及字体大小
```

![](https://image--1.oss-cn-shenzhen.aliyuncs.com/indeqrwr rwq rqrq14421ex.png)

第二种解法：

```python
 for index in movie_pd.index:
     day_tickets = movie_pd.iloc[index]    //按行选取数据
     xs = ind-(bin_width*(2-index))        //定好位置
     plt.bar(xs,day_tickets,width=bin_width,label="第%d天"%(index+1))     //画图
     for ticket,x in zip(day_tickets,xs):     //在条形图上加数字标签
         plt.annotate(ticket,xy=(x,ticket),xytext=(x-0.1,ticket+0.1))

 plt.xticks(ind,list(movies.keys()),fontsize=20)   
 plt.yticks(list(range(0,18,2)),fontsize=20)       
 plt.ylabel('单位：亿',fontsize=25)
 plt.legend(bbox_to_anchor=(0.8,0.8),fontsize=20)  
```

![](https://image--1.oss-cn-shenzhen.aliyuncs.com/induuuuuuuuuuuuex.png)

### 举个例子2

要求：把年份当做x轴，报名人数当做y轴的值。

```python
>>> data = {
    "普通本科":[721,738,749,761,791],
    "中等职业教育": [620,601,593,582,557],
    "普通高中": [797,797,803,800,793]
}
>>> data_df=pd.DataFrame(data,index=['2014','2015','2016','2017','2018'])
 	  普通本科	中等职业教育　普通高中
2014 	721 	620 	797
2015 	738 	601 	797
2016 	749 	593 	803
2017 	761 	582 	800
2018 	791 	557 	793

>>> width=0.75
>>> bin_width=width/3
>>> ind=np.arange(0,5)　　　　　　　　　　　//一共5个横坐标刻度

>>> for col in range(3):　　　　　　　　　　//每个横坐标包含３个数据条
>>>     num=data_df.iloc[:,col]
>>>     xt=ind-bin_width*(1-col)
>>>     plt.bar(xt,num,width=bin_width,label=data_df.columns[col])
    
>>>     for n,x in zip(num,xt):
>>>        plt.annotate(n,xy=(x,n),xytext=(x-0.1,n+10))

>>> plt.xticks(ind,data_df.index)
>>> plt.ylim(0,1200)
>>> plt.legend(ncol=3)
>>> plt.show()
```

<img src="https://image--1.oss-cn-shenzhen.aliyuncs.com/inde555555x.png" style="zoom:120%;" />

## 3.4 堆叠条形图

堆叠条形图，是将一组相关的条形图堆叠在一起进行比较的条形图。基本绘图函数同3.1和3.2。拿基本条形图（竖直条形图）来讲，只需要对bottom参数进行设置，叠放同类型的条形图。

```python
 menMeans = (20, 35, 30, 35, 27)
 womenMeans = (25, 32, 34, 20, 25)
 groupNames = ('G1','G2','G3','G4','G5')
 xs = np.arange(len(menMeans))
 bar1 = plt.bar(xs,menMeans,width=0.5)
 bar2 = plt.bar(xs,womenMeans,bottom=menMeans,width=0.5)    //注意这里的bottom
 plt.xticks(xs,groupNames,fontsize=15)
 plt.legend((bar1,bar2),('男性平均值','女性平均值'))  //设置图例
```

![](https://image--1.oss-cn-shenzhen.aliyuncs.com/ind3333333333344444ex.png)

# 4. 直方图

直方图(Histogram)，又称质量分布图，是一种统计报告图，由一系列高度不等的条纹表示数据分布的情况。一般用横轴表示数据类型，纵轴表示分布情况。

```
//基本绘图函数
 plt.hist(x, bins, range,density=, cumulative=, *kwargs)
```

① `x`：需要划分的数组。直方图将会从这组数据中进行分组。

② `bins`：指定划分范围。如果是数字，代表的是要分成多少组；如果是序列，那么就会按照序列中指定的值进行分组。

③ `range`：元组或者None，如果为元组，那么指定`x`划分区间的最大值和最小值。如果`bins`是一个序列，那么`range`没有有没有设置没有任何影响。

④ `density`：频率统计参数。默认是`False`。如果等于`True`，那么将会使用频率分布直方图。

⑤ `cumulative`：频率统计参数。如果这个和`density`都等于`True`，那么返回值的第一个参数会不断的累加，最终等于`1`。

⑥ `*kwargs`：包括`color`，`edgecolor`，`label`，`align`，`bottom`，`orientation= {'horizontal', 'vertical'}`，等。

这个函数可以接收返回值：`n，bins, patches = plt.hist() `
`n`：数组，每个区间内值出现的个数；
`bins`：数组，区间序列；
`patches`：直方图每个图块序列的信息。

**细节设置等其他设置同2.2。**

### 举个例子

```python
 data = np.random.randint(10,100,size=(100))
 nums,bins,patches = plt.hist(data,bins=
                       (10,20,30,40,50,60,70,80,90,100),edgecolor='r')

 plt.xticks(bins,bins)
 for num,bin in zip(nums,bins):
     plt.annotate(num,xy=(bin,num),xytext=(bin+1.5,num+0.2))
 plt.ylim(0,20)

```

![](https://image--1.oss-cn-shenzhen.aliyuncs.com/indwwwwwwwwwwweqex.png)

# 5. 散点图

## 5.1 基本散点图

散点图也叫 X-Y 图，它将所有的数据以点的形式展现在直角坐标系上，以显示变量之间的相互影响程度，点的位置由变量的数值决定。通过观察散点图上数据点的分布情况，我们可以推断出变量间的相关性。

```
//基本绘图函数
 plt.sactter(x, y, s=, c=, marker=, *kwargs)
```

① `x,y`：分别是x轴和y轴的数据集。两者的数据长度必须一致。

② `s`：点的尺寸。可以是一个数字，也可以是一个序列。

③ `c`：点的颜色，可以为具体的颜色，也可以为一个序列或者是一个`cmap`对象。

④ `marker`：标记点，默认是圆点。

⑤ `*kwargs`：包括`edgecolor`，`label`，`alpha`等。

**关于颜色映射：**

`c=[nums], cmap=plt.cm.Reds`，将一个需要映射的序列传给c，cmap便会根据c值的大小给出相应颜色的深浅。

**细节设置等其他设置同2.2。**

### 举个例子

```python
 x=np.random.randint(1,20,size=(200))
 y=np.random.randint(10,50,size=(200))
 plt.scatter(x,y,s=10,c=y,cmap=plt.cm.Blues)
```

<img src="https://image--1.oss-cn-shenzhen.aliyuncs.com/indeqwert54x.png" style="zoom:120%;" />

## 5.2 回归方程

有一组数据后，我们可以对这组数据进行回归分析，回归分析可以帮助我们了解这组数据的大体走向。

回归分析按照涉及的变量的多少，分为一元回归和多元回归分析；

按照自变量的多少，可分为简单回归分析和多重回归分析；

按照自变量和因变量之间的关系类型，可分为线性回归分析和非线性回归分析。

如果在回归分析中，只包括一个自变量和一个因变量，且二者的关系可用一条直线近似表示，这种回归分析称为一元线性回归分析。如果回归分析中包括两个或两个以上的自变量，且自变量之间存在线性相关，则称为多重线性回归分析。

回归方程的绘制我们需要借助`scikit-learn`库，这个库是专门做机器学习用的，我们需要使用里面的线性回归类`sklearn.liear_regression.LinearRegression`。这里我们只是粗略了解下。

```python
from sklearn.linear_model import LinearRegression

//假如有两组数据
xtrain = male_athletes['Height']
ytrain = male_athletes['Weight']


model = LinearRegression()  //线性回归分析模型

xtrain = xtrain[:,np.newaxis]  //把因变量转换成1列多行的数据,也可用reshape函数

model.fit(xtrain[:,np.newaxis],ytrain)   //喂训练数据进去

model.coef_                    //斜率
model.intercept_               //截距
new_y = model.predict(new_x)   //根据回归方程计算出的y轴坐标
```

# 6. 饼图

饼图是一个划分为几个扇形的圆形统计图表，用于描述量、频率或百分比之间的相对关系的。 在`matplotlib`中，可以通过`plt.pie`来实现。

```
//基本绘图函数
plt.pie(x, labels=, explode=, autopct=, shadow=, textprops=, *kwargs)

```

① `x`：饼图的比例数据序列。

② `labels`：饼图上每个分块的名称文字。

③ `explode`：设置某几个分块是否要分离饼图。

④ `autopct`：设置比例文字的展示方式。比如保留几个小数等。

⑤ `shadow`：是否显示阴影。

⑥ `textprops`：文本的属性（颜色，大小等）。

⑦`*kwargs`：包括颜色序列`color`，饼图半径`radius`，标签与饼图距离`labeldistance`等。

这个函数可以接收返回值：`patches, texts, autotexts=plt.pie()`
`patches`：饼图上每个分块的对象；
`texts`：分块的名字文本对象；
`autotexts`：分块的比例文字对象。

### 举个例子

```python
oses = {'windows7':60.86,'windows10': 18.46,'windows8': 3.61,
        'windows xp': 10.3,'mac os': 6.78,'其他': 1.12}
names = oses.keys()
percents = oses.values()
plt.pie(percents,labels=names,autopct="%.2f%%",explode=(0,0.05,0,0,0,0))
plt.show()
```

<img src="https://image--1.oss-cn-shenzhen.aliyuncs.com/inde2333332wex.png" style="zoom:120%;" />

# 7. 箱线图

箱线图（Box-plot）又称为盒须图、盒式图或箱型图，是一种用作显示一组数据分散情况资料的统计图。因形状如箱子而得名。在各种领域也经常被使用，它主要用于反映原始数据分布的特征，还可以进行多组数据分布特征的比较。箱线图的绘制方法是：先找出一组数据的**上限值、下限值、中位数（Q2）和下四分位数（Q1）以及上四分位数（Q3）**；然后，连接两个四分位数画出箱子；再将最大值和最小值与箱子相连接，中位数在箱子中间。

<img src="https://image--1.oss-cn-shenzhen.aliyuncs.com/5-2222223.png" style="zoom:80%;" />

```
//基本绘图函数
 plt.boxplot(x, notch=, sym=, vert=, whis=, position=, *kwargs)
```

① `x`：需要绘制箱线图的数据。

② `notch`：是否展示置信区间，默认为False。

③ `sym`：异常点符号表示，默认小圆点。

④ `vert`：是否是垂直，默认True。

⑤ `whis`：计算上下限所用的的系数，默认1.5。如果数据本身就为序列，则无需用到。

⑥ `position`：设置每个盒子位置。

⑦`*kwargs`：还包括盒子宽度`widths`，盒子标签`labels`，`meanline`和`showmeans`都为True则指定绘制平均值线条。

此`plt.boxplot()`函数有返回值：返回一个字典，有以下`key`：

`boxes`：箱图的主体，显示四分位数和中位数的置信区间（如果启用）。

`medians`：每个框的中间的水平线。

`whiskers`：垂直线延伸到最极端的非异常数据点。

`caps`：晶须末端的水平线。

`fliers`：表示超出晶须（传单）的数据的点。

`means`：表示均值的点或线。

**细节设置等其他设置同2.2。**

### 举个例子

```python
df = pd.DataFrame(np.random.rand(10,5),columns=['A','B','C','D','E'])
//输出结果如下：
          A         B         C         D         E
0  0.504272  0.613087  0.463340  0.294463  0.437389
1  0.225515  0.792266  0.975446  0.450801  0.564991
2  0.617762  0.691476  0.948907  0.073688  0.821399
3  0.067783  0.149183  0.507391  0.458275  0.850159
4  0.304483  0.300143  0.073447  0.017097  0.728868
5  0.514748  0.735109  0.931701  0.489415  0.650087
6  0.153983  0.161155  0.633645  0.717268  0.250304
7  0.833904  0.890038  0.023813  0.812960  0.984342
8  0.793293  0.027440  0.842855  0.854892  0.589219
9  0.818400  0.981712  0.441924  0.257464  0.284837
```

**画图：**

```python
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt

plt.figure(figsize=(20,10))
plt.boxplot(df.T,sym='o')  //默认按行作图，所以这里转置了下，使得按列作图
plt.xticks(range(1,6),['A','B','C','D','E'])
plt.show()
```

![](https://image--1.oss-cn-shenzhen.aliyuncs.com/image-20200310173339531.png)

# 8. 雷达图

雷达图（Radar Chart）又被叫做蜘蛛网图，适用于显示三个或更多的维度的变量的强弱情况。比如英雄联盟中某个影响的属性（法术伤害，物理防御等），或者是某个企业在哪些业务方面的投入等，都可以用雷达图方便的表示。

```
//基本绘图函数
 plt.polar(theta, y, *kwargs)
```

① `theta`：对应数值的坐标弧度，根据数据量将2π分几个区间

② `y`：所需绘制的数组数据

③ `*kwargs`：其他参数可参照`plt.plot`



注意事项：

1）因为`polar`并不会完成线条的闭合绘制，所以我们在绘制的时候需要在`theta`中和`values`中在最后多重复添加第0个位置的值，然后在绘制的时候就可以和第1个点进行闭合了。

2）`polar`只是绘制线条，所以如果想要把里面进行颜色填充，那么需要调用`fill`函数来实现。

3）`polar`默认的圆圈的坐标是角度，如果我们想要改成文字显示，那么可以通过`xticks`来设置。

### 举个例子

```python
 properties = ['输出','KDA','发育','团战','生存']
 values = [40,91,44,90,95,40]         //增加数据40使首尾闭合
 theta = np.linspace(0,np.pi*2,6)     // 一定要写弧度值，个数与values对应
 plt.polar(theta,values,'y')
 plt.fill(theta,values,'g')           //填空颜色块
 plt.xticks(theta,properties)         //将刻度换成文本标签
```

![](https://image--1.oss-cn-shenzhen.aliyuncs.com/indeeeerqdfex.png)

# 9. 辅助线

## 9.1 有限垂直线

```
plt.vlines(x, ymin, ymax, colors='k', label='')
```

## 9.2 有限水平线

```
plt.hlines(y, xmin, xmax, colors='k', label='')
```

## 9.3 无限垂直线

```
plt.axvline(x=0, ymin=0-1, ymax=0-1, hold=None, **kwargs)
```

## 9.4 无限水平线

```
plt.axhline(y=0, xmin=0-1, xmax=0-1, hold=None, **kwargs)
```

## 9.5 垂直区域

```
plt.axvspan(xmin, xmax, ymin=0-1, ymax=0-1, hold=None, **kwargs)
```

## 9.6 水平区域

```
plt.axhspan(ymin, ymax, xmin=0-1, xmax=0-1, hold=None, **kwargs)
```

# 10. matplotlib绘图分析

## 图像解剖分析

![](https://image--1.oss-cn-shenzhen.aliyuncs.com/matplotlib图像剖析详解.jpg)

1. `Figure`：图形绘制的画板，他就相当于一个黑板，所有的图都是绘制在`Figure`上面。
2. `Axes`：每个图都是`Axes`对象。一个`Figure`上可以有多个`Axes`对象。
3. `Axis`：`x`轴、`y`轴的对象。
4. `Tick`：`x`轴和`y`轴上的刻度对象。每一个刻度都是一个`Tick`对象。
5. `TickLabel`：每个刻度上都要显示文字，这个文字的显示就是在`TickLabel`上。
6. `AxisLabel`：`x`轴和`y`轴的名称的文字显示。
7. `Legend`：图例对象。
8. `Title`：`Axes`图的标题对象。
9. `Line2D`：绘制在`Axes`上的线条对象，比如折线图等。
10. `Reactangle`：绘制在`Axes`上的矩形对象，比如条形图等。
11. `Marker`：标记点，比如绘制散点图上的每个点就是这个对象。
12. `Artist`：只要是绘制在`Figure`上的元素（包括Figure），都是`Artist`的子类。

## 10.1 Figure容器

`Figure`容器是最底层的容器，他几乎包含了这个图的所有对象。通过`add_subplot`和`add_axes`方法可以添加`Axes`对象，这两个方法添加的都是`Axes`及其子类的对象。添加完成后是存储在`figure.axes`中。

### 10.1.1 添加Axes对象

`Figure`只是一个黑板，如果想要绘图，需要先添加`Axes`。添加`Axes`可以通过`add_axes`和`add_subplot`来实现。示例代码如下：

```python
fig = plt.figure()          //创建一个figure对象
ax1 = fig.add_subplot(211)  // 添加一个Axes
ax2 = fig.add_axes([0.1,0.1,0.8,0.3])   // 添加一个Axes，其中参数是left,bottom,width,height
```

### 10.1.2  操作当前Axes对象

可以通过`figure.gca`以及`figure.sca`来设置和获取当前的`axes`对象。

```python
>>> fig = plt.figure()
>>> ax1 = fig.add_subplot(211)
>>> ax2 = fig.add_axes([0,0,1,0.3])
>>> print(fig.gca())
>>> print(fig.sca(ax1))
Axes(0,0;1x0.3)
AxesSubplot(0.125,0.536818;0.775x0.343182)
```

### 10.1.3  删除Axes对象

`Figure`上的所有`Axes`对象都是保存在`fig.axes`中，但是如果想要删除某个`Axes`对象，那么必须通过`delaxes`来实现。

```python
ig = plt.figure()
ax1 = fig.add_subplot(211)
ax2 = fig.add_axes([0,0,1,0.3])
fig.delaxes(ax1)
```

### 10.1.4 获取所有的axes

```python
for ax in fig.axes:
    ax.grid(True)       // 设置打开网格
```

### 10.1.5 Figure的属性汇总

![](https://image--1.oss-cn-shenzhen.aliyuncs.com/5-2242422222r8.png)

## 10.2 Axes容器

`Axes`容器是用来创建具体的图形的。比如画曲线，柱状图，都是画在上面。所以之前我们学的使用`plt.xx`绘制各种图形（比如条形图，直方图，散点图等）都是对`Axes`的封装。比如`plt.plot`对应的是`axes.plot`，比如`plt.hist`对应的是`axes.hist`。针对图的所有操作，都可以在`Axes`上找到对应的`API`。另外后面要讲到的`Axis`容器，是轴的对象，也是绑定在`Axes`上面。

### 10.2.1 设置x和y轴

```python
fig = plt.figure()  
axes = fig.add_subplot(111)  
axes.plot(np.random.randn(10))

axes.set_xlim(-2,12)  //设置x轴的最大值和最小值

axes.set_ylim(-3,3)   // 设置y轴的最大值和最小值
```

### 10.2.2  添加文本

之前添加文本我们用的是`annotate`，但是如果不是需要做注释，其实还有另外一种更加简单的方式，那就是使用`text`方法：

```python
data = np.random.randn(10)
fig = plt.figure()
axes = fig.add_subplot(111)
axes.plot(data)

axes.text(0,0,"hello"，ha='center')  //添加文本，比annotate更加方便
```

### 10.2.3 绘制双`Y`轴

```python
fig = plt.figure()
ax1 = fig.add_subplot(211)
ax1.bar(np.arange(0,10,2),np.random.rand(5))
ax1.set_yticks(np.arange(0,1,0.25))
ax2 = ax1.twinx()                     //克隆一个共享x轴的axes对象
ax2.plot(np.random.randn(10),c="b")
plt.show()
```

<img src="https://image--1.oss-cn-shenzhen.aliyuncs.com/indewr ww wex.png" style="zoom:150%;" />

## 10.3 Axis容器

`Axis`代表的是`x`轴或者`y`轴的对象。包含`Tick`（刻度）对象，`TickLabel`刻度文本对象，以及`AxisLabel`坐标轴文本对象。`axis`对象有一些方法可以操作刻度和文本等。

### 10.3.1 设置label的位置

```python
fig = plt.figure()
axes = fig.add_subplot(111)
axes.plot(np.random.randn(10))
axes.set_xlabel("x coordate")

axes.xaxis.set_label_coords(0,-0.1)  //设置x轴label的位置为(0.-0.1)
```

### 10.3.2 设置刻度格式

```python
import matplotlib.ticker as ticker
fig = plt.figure()
axes = fig.add_subplot(111)
axes.plot(np.random.randn(10))
axes.set_xlabel("x coordate")

formatter = ticker.FormatStrFormatter('%.2f')   // 创建格式化对象

axes.yaxis.set_major_formatter(formatter)       // 设置格式化对象
```

### 10.3.3 设置轴的属性

```python
fig = plt.figure()

ax1 = fig.add_axes([0.1, 0.3, 0.4, 0.4])
ax1.set_facecolor('lightslategray')

for label in ax1.xaxis.get_ticklabels():  //设置刻度上文本的属性
    # label是一个Label对象
    label.set_color('red')
    label.set_rotation(45)
    label.set_fontsize(16)

for line in ax1.yaxis.get_ticklines():    //设置刻度上线条的属性
    
    line.set_color('green')              //line是一个Line2D对象
    line.set_markersize(25)
    line.set_markeredgewidth(3)

plt.show()
```

## 10.4 Tick容器

`Tick`是用来做刻度的，包括刻度和网格对象。其中可操作的属性如下：

![](https://image--1.oss-cn-shenzhen.aliyuncs.com/5wwrwr43-31.png)

```python
import matplotlib.ticker as ticker

np.random.seed(19680801)

fig, ax = plt.subplots()
ax.plot(100*np.random.rand(20))

formatter = ticker.FormatStrFormatter('$%.2f')
ax.yaxis.set_major_formatter(formatter)

for tick in ax.yaxis.get_major_ticks():
    tick.label1On = False
    tick.label2On = True
    tick.label2.set_color('green')

plt.show()
```

![](https://image--1.oss-cn-shenzhen.aliyuncs.com/5-334242r2.png)

# 11. 多图布局

## 11.1 宽松布局

在一个`Figure`上面，可能存在多个`Axes`对象，如果`Figure`比较小，那么有可能会造成一些图形元素重叠，这时候我们就可以通过`fig.tight_layout`或者是`fig.subplots_adjust`方法来帮我们调整。假如现在没有经过调整，那么以下代码的效果图如下：

```python
import matplotlib.pyplot as plt
import numpy as np

def example_plot(ax, fontsize=12):
    ax.plot([1, 2])
    ax.set_xlabel('x-label', fontsize=fontsize)
    ax.set_ylabel('y-label', fontsize=fontsize)
    ax.set_title('Title', fontsize=fontsize)

fig,axes = plt.subplots(2,2)
fig.set_facecolor("y")
example_plot(axes[0,0])
example_plot(axes[0,1])
example_plot(axes[1,0])
example_plot(axes[1,1])

//加下面一行代码，可图变不拥挤
plt.tight_layout()
```

![](https://image--1.oss-cn-shenzhen.aliyuncs.com/ind241ssssssssex.png)

也可传入参数自定义调整：

```python
tight_layout(w_pad,h_pad)  //可设置水平，垂直方向图的间距
fig.subplots_adjust(left,bottom,righte,top,wspace=,hspace=)
```

## 11.2 自定义布局

如果布局不是固定的几宫格的方式，而是某个图占据了多行或者多列，那么就需要采用一些手段来实现。如果不是很复杂，那么直接可以通过`subplot`等方法来实现。示例代码如下：

```python
ax1 = plt.subplot(221)    //分成2x2，占用第1个，即第一行第一列的子图
ax2 = plt.subplot(223)    //分成2x2，占用第3个，即第二行第一列的子图
ax3 = plt.subplot(122)   //分成1x2，占用第二个，即第二列
```

![](https://image--1.oss-cn-shenzhen.aliyuncs.com/5-32222226.png)

但是如果实现的布局比较复杂，那么就需要采用`GridSpec`对象来实现：

```python
fig = plt.figure()

gs = fig.add_gridspec(3,3)  //创建3行3列的GridSpec对象

ax1 = fig.add_subplot(gs[0,0:3])
ax1.set_title("[0,0:3]")

ax2 = fig.add_subplot(gs[1,0:2])
ax2.set_title("[1,0:2]")

ax3 = fig.add_subplot(gs[1:3,2])
ax3.set_title("[1:3,2]")

ax4 = fig.add_subplot(gs[2,0])
ax4.set_title("[2,0]")

ax5 = fig.add_subplot(gs[2,1])
ax5.set_title("[2,1]")

plt.tight_layout()
```

![](https://image--1.oss-cn-shenzhen.aliyuncs.com/5-242342437.png)

也可以设置宽高比例。示例代码如下：

```
widths = (1,2,1)         //设置宽度比例为1:2:1
heights = (2,2,1)        //设置高度比例为2:2:1

fig = plt.figure()

// 创建GridSpec对象的时候指定宽高的比
gs = fig.add_gridspec(3,3,width_ratios=widths,height_ratios=heights)

for row in range(0,3):
    for col in range(0,3):
        fig.add_subplot(gs[row,col])
plt.tight_layout()
```

![](https://image--1.oss-cn-shenzhen.aliyuncs.com/5-312312318.png)

## 11.3 手动设置位置

通过`fig.add_axes`的方式添加`Axes`对象，可以直接指定位置。也可以在添加完成后，通过`axes.set_position`的方式设置位置。示例代码如下：

```
// add_axes的方式
fig = plt.figure()
fig.add_subplot(111)
fig.add_axes([0.2,0.2,0.4,0.4])

// 设置position的方式
fig,axes = plt.subplots(1,2)
axes[1].set_position([0.2,0.2,0.4,0.4])
```

## 11.4 实践

```
fig = plt.figure(figsize=(8,8))
widths = (2,0.5)
heights = (0.5,2)
gs = fig.add_gridspec(2,2,width_ratios=widths,height_ratios=heights)

// 顶部的直方图
ax1 = fig.add_subplot(gs[0,0])
ax1.hist(male_athletes['Height'],bins=20)
for tick in ax1.xaxis.get_major_ticks():
    tick.label1On = False

// 中间的散点图
ax2 = fig.add_subplot(gs[1,0])
ax2.scatter('Height','Weight',data=male_athletes)

// 右边的直方图
ax3 = fig.add_subplot(gs[1,1])
ax3.hist(male_athletes['Weight'],bins=20,orientation='horizontal')
for tick in ax3.yaxis.get_major_ticks():
    tick.label1On = False
    
fig.tight_layout(h_pad=0,w_pad=0)
```

![](https://image--1.oss-cn-shenzhen.aliyuncs.com/5-23123139.png)

# 12. Matplotlib配置

## 12.1 修改默认的配置

修改默认的配置可以通过`matplotlib.rcParams`来设置，比如修改字体，修改线条大小和宽度等。示例代码如下：

```python
import matplotlib.pyplot as plt

// 设置字体为仿宋
plt.rcParams['font.sans-serif'] = ['FangSong']

// 设置字体大小为20
plt.rcParams['font.size'] = 20

// 设置线条宽度
plt.rcParams['lines.linewidth'] = 2

// 设置线条颜色
plt.rcParams['axes.prop_cycle'] = plt.cycler('color', ['r', 'y'])
```

字体设置值对照表：

![](https://image--1.oss-cn-shenzhen.aliyuncs.com/5-4231231eww0.png)



## 12.2 自定义配置文件

有时候我们可能需要设置一大堆参数，并且这个配置在后面很多项目中可能都会用到，那么这时候我们就可以把这些配置信息放到文件中（可配置项见下），文件的命名规则为`[名称].mplstyle`，然后把这个文件放到`matplotlib.get_configdir()/stylelib`的目录中，在写代码的时候根据名称加载这个配置文件，示例代码如下：

```
plt.style.use("名称")
```

