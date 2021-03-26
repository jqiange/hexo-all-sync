---
title: Seaborn-Pygal-Pyecharts可视化
categories: 数据分析
HTML格式实现标签样本: <center><font size=5 face="黑体">文本区</font></center>
date: 2020-02-22 16:24:55
top:
tags: 
- Seaborn
- pygal
- Pyecharts
typora-root-url: ..
---



# 1.Seaborn 前言

**Seaborn和Matplotlib同为Python最强大的两个可视化库。Seaborn是在matplotlib的基础上进行了更高级的API封装，让你能用更少的代码去调用 matplotlib的方法，从而使得作图更加容易，在大多数情况下使用seaborn就能做出很具有吸引力的图，而使用matplotlib就能制作具有更多特色的图。应该把Seaborn视为matplotlib的补充，而不是替代物。**

**Seaborn：作图更容易，图像更漂亮。主要的缺点则是定制化能力会比较差，只能实现固化的一些可视化模板类型**。

**Matplotlib：可以实现高度定制化绘图的，高度定制化可以让你获得最符合心意的可视化输出结果，但也因此需要设置更多的参数，因而代码更加复杂一些。**

<font size=5 face="微软雅黑">Seaborn官方文档</font> http://seaborn.pydata.org/tutorial

建议阅读顺序：Plot aesthetics，Plotting functions，Multi-plot grids。

## **1.1 快速优化图形**

Seaborn以多变优雅的绘图风格出名，首先热个身：

```
import numpy as np
import seaborn as sns
import matplotlib.pyplot as plt

def sinplot(flip=1):
    x = np.linspace(0, 14, 100)
    for i in range(1, 7):
        plt.plot(x, np.sin(x + i * .5) * (7 - i) * flip)

sinplot()  //输出如图1

sns.set()
sinplot()  //输出如图2
```

![](/assets/Snipaste_2020-02-22_17-50-16.png)

使用 Seaborn 完成图像快速优化的方法非常简单。只需要将 Seaborn 提供的样式声明代码 `sns.set()` 放置在绘图前即可。

 **`sns.set()`还可进行细致化设置：**

```
sns.set(context=, style=, palette=, font=, font_scale=, color_codes=, rc=)
```

- `context=` 参数控制着默认的画幅大小，分别有 `{paper, notebook, talk, poster}` 四个值。其中，`poster > talk > notebook > paper`。
- `style=` 参数控制默认样式，分别有 `{darkgrid, whitegrid, dark, white, ticks}`，你可以自行更改查看它们之间的不同。默认为`darkgrid`。
- `palette=` 参数为预设的调色板。分别有 `{deep, muted, bright, pastel, dark, colorblind}` 等，你可以自行更改查看它们之间的不同。
- 剩下的 `font=` 用于设置字体，`font_scale=` 设置字体大小，`color_codes=` 不使用调色板而采用先前的 `'r'` 等色彩缩写。

**另外，还有两个直接控制风格的函数方法：`sns.set_style()`和`sns.axes_style()`。**

其包含两个参数`style=`，可直接设置样式；`rc=`，字典值，其参数映射将覆盖预设的seaborn样式字典中的值。对于`sns.axes_style()`，如没有进行赋值，则返回当前默认设置，字典格式数据。

## 1.2 去掉坐标轴

```
sns.despine(offset=,trim=,left=)
```

## 1.3 临时绘图风格

```
with sns.axes_style("white"):    //用来切换绘图风格
    ax = f.add_subplot(gs[0, 0])
    sinplot1()
    
sinplot2()          //sinplot1()和sinplot2() 绘图风格不一样
```

## 1.4 缩放图风格

```
sns.set()  //重置函数
//包含四个预设，相对大小的顺序，paper，notebook，talk，和poster
```

```
//包含四个预设，相对大小的顺序为paper，notebook，talk，和poster
sns.set_context("paper")
```

# 2. Seaborn 绘图 API

Seaborn 一共拥有 50 多个 API 类，相比于 Matplotlib 数千个的规模，可以算作是短小精悍了。其中，根据图形的适应场景，Seaborn 的绘图方法大致分类 6 类，分别是：

**关联图、类别图、分布图、回归图、矩阵图和组合图。**

而这 6 大类下面又包含不同数量的绘图函数。

Seaborn 中的 API 分为 Figure-level 和 Axes-level 两种。

Figure-level 和 Axes-level API 的区别在于，Axes-level 的函数可以实现与 Matplotlib 更灵活和紧密的结合，而 Figure-level 则更像是「懒人函数」，适合于快速应用。

## 2.1 关联图

关联图的 Figure-level 接口是[`relplot`](http://seaborn.pydata.org/tutorial/relational.html)。【relating plot】

总函数：`sns.relplot()`，主要有**散点图**和**折线图** 2 种样式。

分函数：`sns.scatterplot()`，`sns.lineplot()`。

```
sns.relplot(x=, y=, data=,kind=, palette=, ax=, hue=, style=,  *kwargs)
```

- `x=, y=`：传`data=`中的数字自变量和因变量。
- `data=`：DataFrame类型的数据表。
- `kind=`：指定绘制的图像类型，选项为 `'scatter'`和`'line'`，与分函数对应。默认散点图。
- `palette=`：调色板，可指定内置的样式，色彩丰富。
- `ax=`：指定图标绘画在哪个坐标系。
- `hue=`：指定绘制数据的颜色映射集。
- `style=`：赋予不同类别的散点不同的形状映射集
- `*kwargs`：包含`size=`，指定图点大小映射集；`sizes=`，既可以是某个具体数值，也可以是映射集。

分函数用法与之相同，只是不用指定kind。

**如想水平绘图，交换xy数据就行。**

## 2.2 类别图

与关联图相似，类别图的 Figure-level 接口是 `catplot`。 【categorical plot】

总函数：`sns.catplot()`，主要有**分类散点图**，**分类分布图**，**分类估计图** 3类样式。

```
sns.catplot(x=, y=, kind=, data=, palette=, hue=)   //参数意义同关联图,默认kind="strip"
```

[**分类散点图：**](http://seaborn.pydata.org/tutorial/categorical.html#categorical-scatterplots)

`sns.stripplot()`(`kind="strip"`)：带状散点图，一个类别的所有点都将沿着与分类变量相对应的轴落在同一位置。

`sns.swarmplot()` (`kind="swarm"`)：带状散点图（散点不重叠），使用防止重叠的算法沿分类轴调整点。它仅适用于相对较小的数据集，但它可以更好地表示观测值的分布。

例：

![](/assets/image-20201030224405664.png)

[**分类分布图：**](http://seaborn.pydata.org/tutorial/categorical.html#distributions-of-observations-within-categories)

`sns.boxplot()` (`kind="box"`)：箱线图

![](/assets/image-20201030224446519.png)

`sns.violinplot()(kind="violin")`：小提琴图

![](/assets/image-20201030224536093.png)

`sns.boxenplot()` (`kind="boxen"`)：增强箱线图

<img src="/assets/image-20201030224607358.png" style="zoom:120%;" />



[**分类估计图：**](http://seaborn.pydata.org/tutorial/categorical.html#statistical-estimation-within-categories)

`sns.pointplot()`(`kind="point"`)：点线图

![](/assets/image-20201030224709044.png)

`sns.barplot()`(`kind="bar"`)：条形图

- 分类图中的条形图只是将点线图中的平均值以条形方框展示而已；置信区间还是以线的形式展示。

`sns.countplot()` (`kind="count"`)：计数条形图

## 2.3 分布图

分布图主要是用于可视化变量的分布情况，一般分为单变量分布和双变量分布。

### 2.3.1 jointplot()

[`sns.jointplot(x=, y=, data=,kind= color=, *kwargs)`](http://seaborn.pydata.org/generated/seaborn.jointplot.html?highlight=jointplot#seaborn.jointplot)

kind=“scatter” | “reg” | “resid” | “kde” | “hex” 

主要是用于绘制二元变量分布图。散点图，六边形图，核密度估计

### 2.3.2 pairplot()

[`sns.pairplot(data=, hue=, vars=， *kwargs)`](http://seaborn.pydata.org/generated/seaborn.pairplot.html?highlight=pairplot#seaborn.pairplot)

支持一次性将数据集中的特征变量两两对比绘图。默认情况下，对角线上是单变量分布图，而其他则是二元变量分布图。

### 2.3.3 distplot()

[`sns.distplot(a, bins=, hist=, kde=, color=, label=, *kwargs)`](http://seaborn.pydata.org/generated/seaborn.displot.html?highlight=displot#seaborn.displot)

绘制单变量分布。直方图，核密度估计，拟合参数分布

### 2.3.4 kdeplot()

[`sns.kdeplot(data, shade=, vertical=, *kwargs)`](http://seaborn.pydata.org/generated/seaborn.kdeplot.html#seaborn.kdeplot)

绘制二维核密度图

## 2.4 回归图

[lmplot()](http://seaborn.pydata.org/generated/seaborn.lmplot.html?highlight=lmplot#seaborn.lmplot)与[regplot()](http://seaborn.pydata.org/generated/seaborn.regplot.html?highlight=regplot#seaborn.regplot)

`sns.lmplot(x=, y=, hue=, data=, *kwargs)`

同样是用于绘制回归图，但 `lmplot` 支持引入第三维度进行对比

`sns.regplot(x=, y=, data=, *kwargs)`

绘制回归图时，只需要指定自变量和因变量即可，`regplot` 会自动完成线性回归拟合。

## 2.5 矩阵图

### 2.5.1 heatmap()

[`sns.heatmap(data, vmin=, vmax=, cmap=, center=, *kwargs)`](http://seaborn.pydata.org/generated/seaborn.heatmap.html?highlight=heatmap#seaborn-heatmap)

主要用于绘制热力图。

### 2.5.2 clustermap()

[`sns.clustermap(data, method=, metric=, *kwargs)`](http://seaborn.pydata.org/generated/seaborn.clustermap.html?highlight=clustermap#seaborn.clustermap)

支持绘制层次聚类结构图。

# 3. pygal库

Pygal 是另一个简单易用的数据图库，它以面向对象的方式来创建各种数据图，而且使用 Pygal 可以非常方便地生成各种格式的数据图，包括 PNG、SVG 等。

其最大的特点是能够生成**矢量图形**SVG，还可进行点击显示**交互**。

## 3.1 安装

```python
pip install pygal -i https://pypi.tuna.tsinghua.edu.cn/simple
```

## 3.2 使用

pygal有非常简单清晰的官方文档，需要用时直接查阅就好。

**Pygal官方文档**

http://www.pygal.org/en/stable/documentation/index.html

# 4. Pyecharts库

[Echarts](https://github.com/ecomfe/echarts) 是一个由百度开源的数据可视化，凭借着良好的交互性，精巧的图表设计，得到了众多开发者的认可。而 Python 是一门富有表达力的语言，很适合用于数据处理。

**特点：良好的交互性，支持链式调用，精巧的图表设计**

**官方文档是中文滴！**http://pyecharts.org/#/zh-cn/intro

要想使用直接阅读官方文档即可。其中的【[5 分钟上手](http://pyecharts.org/#/zh-cn/quickstart?id=_5-分钟上手)】初学者一定要看。

为了更快的读懂官方文档，在下面会做一些总结。

## 4.1 初识语法

首先给一段简单代码看看Pyecharts是怎么画图的：

```python
//条形图绘制
from pyecharts.charts import Bar

bar1 = Bar()  //首先创建一个条形图画板

bar1.add_xaxis(["衬衫", "羊毛衫", "雪纺衫", "裤子", "高跟鞋", "袜子"])
bar1.add_yaxis("商家A", [5, 20, 36, 10, 75, 90])
bar1.add_yaxis("商家B", [10, 25, 35, 6, 100, 70])

bar1.render()
//render 会生成本地 HTML 文件，默认会在当前目录生成 render.html 文件
//也可以传入路径参数，如 bar.render("mycharts.html")
```

打开生成的render.html文件如下：

![](/assets/Snipaste_2020-02-23_22-17-54.png)

**要想直接在jupyter notebook中直接显示，可采用如下方式：**

`bar1.render_notebook()`

以上画图方法简单吧。下面再看看怎么进行更多的设置。

## **4.2 会用官方文档**

### 4.2.1 会看图表说明

想要对图表进行进一步的设置，则看官方文档中对应的设置部分，对于条形图Bar：

![](/assets/Snipaste_2020-02-23_22-45-12.png)

上述中`def`定义了函数名`add_yaxis`，调用格式：`Bar().add_yaxis()`，括号的可以传入的参数和用法即为在画红下划线里的那些。有等于号=的通过赋值传参，没有等于号的直接传参。

**举个例子：**

```
from pyecharts.charts import Line

L=Line()
L.add_xaxis(['1月份','2月份','3月份','4月份','5月份','6月份'])
L.add_yaxis("商家A", [5, 20, 36, 10, 75, 90])
L.add_yaxis("商家B", [10, 25, 35, 6, 100, 70],
            is_smooth = True,
            linestyle_opts = opts.LineStyleOpts(type_='dotted'))  //给商家B搞点不一样

L.render_notebook()
```

![](/assets/can323132132131vvas.png)

上述中给商家B搞点不一样，就是**参照官方文档中的用法进行了设置。查看应用路径如下：**

![](/assets/Snipaste_2020-02-23_23-22-03.png)



**这下学会怎么看官方文档了吧！！！**

**点个赞，快！**

接下来如果想要添加【标题】或者修改【图例】等，需要在**全局配置项**中添加。

### 4.2.2 会用全局配置项

**全局配置项中可对这些元素进行配置：**

![](/assets/57307650-8a4d0280-7117-11e9-921f-69b8e9c5e4aa.png)

> **全局配置项可通过 `set_global_options` 方法设置。**

在使用全局变量时，需要再import一个方法：

```python
from pyecharts import options as opts //使用 options 配置项，在 pyecharts 中，一切皆 Options。
```

首先找到全局配置项的那一部分，看目录，如我需要给图表加标题，则找到【TitleOpts：标题配置项】。

![](/assets/Snipaste_2020-02-24_00-00-10.png)

如我需要给图表加视觉映射，则找到【VisualMapOpts：视觉映射配置项】。

![](/assets/Snipaste_2020-02-24_00-09-06.png)



看了这两个例子应该学会举一反三，配置任何全局变量了吧！

### 4.2.3 链式调用

**在使用Pyecharts过程中，一般都会采用链式调用的方法去写代码，当然怎么选择可凭个人习惯。**

先看段代码：

```python
from pyecharts import options as opts
from pyecharts.charts import Scatter

def scatter_spliteline() -> Scatter:     //这里是python中一种特殊写法，下面会有解释。
    c = (
        Scatter()                     //该函数的方法调用就是链式调用
        .add_xaxis(['1月份','2月份','3月份','4月份','5月份','6月份',])
        .add_yaxis("商家A", [80, 55, 35, 10, 100, 70])
        .set_global_opts(
            title_opts=opts.TitleOpts(title="Scatter-显示分割线"),
            xaxis_opts=opts.AxisOpts(splitline_opts=opts.SplitLineOpts(is_show=True)),
            yaxis_opts=opts.AxisOpts(splitline_opts=opts.SplitLineOpts(is_show=True)),
        )
    )
    return c

scatter_spliteline().render_notebook()
```

首先定义一个变量c，去接收画的图表；括号里传入图表类型函数Scatter()，代表要绘制散点图，再分别调用Scatter()里面的add_xaxis()，add_yaxis()，set_global_opts()三个函数，用点连接，代表方法调用。

**【如何理解链式调用】**

- 所谓链式调用，就是一个函数可以“叠加”使用其中的方法。比如普通类Car()中定义了三个方法，A，B，C，当使用其中的方法时，只能分别调用，如：

  ```
  Car().A()
  Car().B()
  Car().C()
  ```

  而链式调用，则可以直接：

  ```python
  Car().A().B().C()   #同时调用A，B,C三个函数
  
  #上面的画图函数就是这样写的：
  Scatter().add_xaxis()
           .add_xaxis()
           .set_global_opts()
  ```

  当然要实现这样的调用，类函数Car()中的方法要return本身。

  如：

  ```python
  class Chain():
  
      def __init__(self,name):
          self.name=name
  
      def A(self):
          print('%s正在调用方法A'%self.name)
          return self
  
      def B(self):
          print('正在调用方法B')
          return self
  
      def C(self):
          print('正在调用方法C')
          return self
  
  chain=Chain(name='author')
  chain.A().B().C()
  
  #输出结果为：
  author正在调用方法A
  正在调用方法B
  正在调用方法C
  
  #以上这就是链式调用
  ```

  以上过程可以这样理解：

  第一层：`chain.A()`调用方法A，打印出“author正在调用方法A”，随后`return`回`self`（就是`Chain()`本身，即`chain.A()`又相当于`Chain()`）

  第二层：`chain.A().B()`等价于`chain.B()`。依次类推。

**【如何理解python代码中的箭头`—>`】**

- 函数参数中的箭头指向是参数的`类型建议符`，以告诉程序员希望传入的实参的类型。是Python 3.5新加的功能。

- 值得注意的是，类型建议符并非强制规定和检查，也就是说即使传入的实际参数与建议参数不符，也不会报错。

- 可以把它看成一种注释即可。不编译，不执行。

  ```python
  def add(x, y) -> int:  # 这里 —> int 表示：告诉程序员这里x,y建议输入数据类型为整型
      return x+y
  ```

  

### 4.2.4 输出图片

Pyecharts默认的输出方式为html网页文件，要想输出图片，可参阅官方文档【[渲染图片](http://pyecharts.org/#/zh-cn/render_images)】



## 4.3 进阶用法

等到我需要用到的时候再进行更新吧。