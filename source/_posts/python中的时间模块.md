---
title: python中的时间模块
categories: python
date: 2020-02-13 23:01:16
top:
tags:
typora-root-url: ..
---

# 时间的表现形式

## 1. struct_time类型

它是一个具有命名元组接口的对象：可以通过索引和属性名访问值。存在以下值：

- index---attribute---values
- 0---tm_year---年份
- 1---tm_mon---月份range[1,12]
- 2---tm_mday---天数range[1,31]
- 3---tm_hour---小时range[0,23]
- 4---tm_min---分钟range[0,59]
- 5---tm_sec---秒数range[0,61]
- 6---tm_wday---星期range[0,6],0是星期日
- 7---tm_yday---一年中的一天range[1,366]
- 8---tm_isdst---tm_isdst可以在夏令时生效时设置为1，而在夏令时不生效时设置为0。值-1表示这是未知的。
- N/A---tm_zone---时区名称的缩写
- N/A---tm_gmtoff---协调世界时以东偏移，以秒为单位.

例如，time.struct_time(tm_year=2019, tm_mon=3, tm_mday=20, tm_hour=23, tm_min=11, tm_sec=33, tm_wday=2, tm_yday=79, tm_isdst=0)

## 2. 格式化时间类型

例如，'Wed Mar 20 23:12:26 2019'

## 3. 时间戳类型

时间戳是指格林威治时间1970年01月01日00时00分00秒(北京时间1970年01月01日08时00分00秒)起至现在的总毫秒数。
 例如，time.time()得到的float类型的秒数。

## 4 时间格式化指令

![](https://image--1.oss-cn-shenzhen.aliyuncs.com/20200323.png)

# 一、time模块

**time模块的主要功能分三个方面：**

- 获取系统时间
- 时间格式化
- 计时

## 1.1 获取系统时间

- `time.time()`  返回当前时间的时间戳

- `time.gmtime()`  获取UTC对应的struct_time对象，元祖的表现形式

- `time.localtime()`   获取当前本地时间的struct_time对象，元祖的表现形式

- `time.ctime()`    获取当前时间对应的易读字符串形式（符合美国人的阅读习惯）

**\# 上述后三个方法括号中可以加时间戳timestmap，即获取时间戳对应的时间。**

## 1.2 时间格式化

- `time.mktime(struct_time)`   把元祖形式的时间转化为时间戳

  struct_time—结构化的时间或者完整的9位元组元素。

- `time.strftime(format, struct_time)`  以指定的通用格式输出时间

- `time.strptime(string_time, format)`  与time.strftime()相反，生成元祖式时间

  format对应(string_time)的时间格式

## 1.3 计时

- `time.sleep(5)`  休息5秒再执行

## 1.4 总结

### 1.4.1 时间相互转化

![](https://image--1.oss-cn-shenzhen.aliyuncs.com/Snipaste_2020-03-23_23-42-34.png)



# 二、datetime模块

datetime模块是time模块的进一步封装，对用户更加友好，在时间各属性的获取上回更加方便一些，当然，在效率上会略微低一些。datetime模块的功能主要都几种在datetime、date、time、timedelta、tzinfo五个类中。这五个类功能如下表所示：

| 类名      | 功能                       |
| --------- | -------------------------- |
| date      | 提供处理日期（年月日）接口 |
| time      | 提供处理时间（时分秒）接口 |
| datetime  | 同时处理日期和时间         |
| timedelta | 提供时间运算接口           |
| tzinfo    | 提供处理时区信息接口       |

## 2.1 date类

使用date类的时候，一般会先传入参数：`date(year,month,day)`

### 2.1.1 元祖变日期：

```python
from datetime import date
date(2020,2,3)
//print输出如下：
2020-02-03  type<datetime.date>
…………………………………………………………………………………………………………………………………………………………………………
date(2020,2,3).isoformat()
//print输出如下：
2020-02-03   type<str>
```

以上用法不多。

### 2.1.2 时间戳变日期

`date.fromtimestamp(timestamp)`

```python
from datetime import date
import time
date.fromtimestamp(time.time())
//print输出如下：
2020-03-23    type<class 'datetime.date'>
```

### 2.1.3 元祖变周数

- `date(year,month,day).isocalendar()`

  以元组(iso year, iso_week, iso_weekday)的形式返回日期

  ```python
  date(2019,2,5).isocalendar()
  //print输出如下：
  (2019,6,2)   type<tuple>
  //意思为2019年第6周周二
  ```

- `date(year,month,day).isoweekday()`

  返回传入日期是星期几

  ```python
  date(2019,2,5).isoweekday()
  //print输出如下：
  2   type<int>
  //意思为传入的日期是周二
  ```

### 2.1.4 元祖变格式化日期

`date(year,month,day).strftime(format)`

按照给定的format进行格式化输出

```python
date(2019,2,5).strftime("%Y-%m-%d")
//print输出如下：
2019-02-05   type<str>
```

### 2.1.5 元祖变结构化日期

`date(year,month,day).timetuple()`

返回日期对应的time.struct_time对象，6维元祖

```python
date(2019,2,5).timetuple()
//print输出如下：
time.struct_time(tm_year=2019, tm_mon=2, tm_mday=5, tm_hour=0, tm_min=0, tm_sec=0, tm_wday=1, tm_yday=36, tm_isdst=-1)
```

### 2.1.6 元祖变英文日期

`date(year,month,day).ctime()`

```
date(2019,2,3).ctime()
//print输出如下：
un Feb  3 00:00:00 2019
```

### 2.1.7 替换日期

`date(year,month,day).replace(year1,month1,day1)`

替换给定日期，但不改变原日期

### 2.1.8 获取系统日期

`date.today()`

```python
date.today()
//print输出如下：
2020-03-23   type<class 'datetime.date'>
```

### 2.1.9 其他

`date(year,month,day).year` 

`date(year,month,day).month `

`date(year,month,day).day `

## 2.2 time类

使用time类的时候，一般会先传入参数：`time(hour,minute,second[,microsecond,tzoninfo])`

### 2.2.1 元祖变格式化时间

`time(hour,minute,second).strftime(format)`

按照给定的format进行格式化输出

```python
from datetime import time
time(12,30,20).strftime('%H:%M:%S')
//print输出如下：
12:30:20    type<str>
```

### 2.2.2 时区名字

`time(hour,minute,second,microsecond,tzoninfo).tzname()`

### 2.2.3 时区偏移

`time(hour,minute,second,microsecond,tzoninfo).utcoffset()`

## 2.3 datetime类

### 2.3.1 获取系统日期和时间

- `datetime.now()`

  返回当前系统时间

- `datetime.now().date()`

  返回当前系统时间的日期部分

- `datetime.now().time()`

  返回当前系统时间的时间部分

```python
datetime.now()
//print输出如下：
2020-03-23 21:47:59.372159    type<class 'datetime.datetime'>
```

### 2.3.2 时间戳变日期和时间

`datetime.fromtimestamp(timestamp)`

返回时间戳timestamp对应的时间，type<class 'datetime.datetime'>

### 2.3.3 元祖变英文时间

`datetime(year, month, day[, hour,……]).ctime()`

```python
datetime(2019,2,13,12).ctime()
//print输出如下：
Sun Feb  13 12:00:00 2019   type<str>
```

### 2.3.4 字符串变格式化时间

`datetime.strptime('xx',format)`

由时间字符串xx格式转化为日期格式

```python
datetime.strptime('2019-4-3','%Y-%m-%d')
//print输出如下：
2019-04-03 00:00:00  <class 'datetime.datetime'>
```

### 2.3.5 格式化输出

`datetime(year, month, day[, hour,……]).strftime(format)`

按照给定的format进行格式化输出

```python
datetime.now().strftime('%b-%d-%Y %H:%M:%S')
//print输出如下：
Mar-23-2020 22:09:44   type<class 'str'>
```

## 2.4 timedelta类

### 1.4.1 日期和时间计算

```python
from datetime import timedelta
from datetime import datetime
tomorrow=datetime.now()+timedelta(days=1)
//print输出如下：
2020-03-24 22:16:18.440283  <class 'datetime.datetime'>
```

### 2.4.2 计算相隔天数/秒数

```python
a=datetime(2016, 10, 20)-datetime(2015, 11, 2)
a.days
a.total_seconds()
//print输出如下：
353
30499200.0
type(a)<class 'datetime.timedelta'>
```



