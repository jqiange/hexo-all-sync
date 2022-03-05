---
title: Selenium自动化测试模拟
categories: 爬虫
HTML格式实现标签样本: <center><font size=5 face="黑体">文本区</font></center>
date: 2020-02-29 21:29:54
top:
tags: Selenium
typora-root-url: ..
---



# 1、Selenium介绍

Selenium是一个用于Web应用程序自动化测试工具。其本来的开发目的帮助前端程序员，以来完成web网站项目的自动化测试。其特点是：能够完全模拟人的行为，打开浏览器，进行搜索，点击，翻页的操作。正是这些特性，在网络爬虫反反爬技术应用广泛，以躲开网站对爬虫的限制。

## 1.1 怎么实现浏览器控制

Selenium具体怎么就能操纵浏览器呢？这要归功于 **浏览器驱动** ，Selenium可以通过API接口实现和浏览器驱动的交互，进而实现和浏览器的交互。所以要配置浏览器驱动。

1. 首先安装python第三方库：`pip install selenium`

2. 下载浏览器驱动，以用于selenium对浏览器的控制。

   对于不同浏览器，对应不同的驱动，最常使用的浏览器是谷歌Chrome和火狐Firefox。

   **火狐驱动下载地址：** http://npm.taobao.org/mirrors/geckodriver/

   **谷歌驱动下载地址：**https://chromedriver.storage.googleapis.com/index.html

   注意：下载的驱动应与浏览器的版本一致。

3. 将下载好的浏览器驱动解压，将解压出的 `exe` 文件放到Python的安装目录下，也就是和`python.exe`同目录即可。

   ![](https://image--1.oss-cn-shenzhen.aliyuncs.com/1560611571791.png)

配置完这一切，就能使用python程序控制浏览器啦！

注意：浏览器驱动也可以放置于对应浏览器的安装目录下；还可以放置与python程序同级目录下。

# 2、使用

## 2.1 初步上手

```python
from selenium import webdriver

driver=webdriver.Chrome()

driver.get('https://baidu.com')
```

这样运行程序就能启动浏览器工作了（这里我使用的是谷歌浏览器）。

```
drive.quit()   //关闭驱动
```

## 2.2 搜索交互

### 2.2.1 CSS选择器

**`.find_element_by_css_selector()`**

```
//打开网页
browser.get('https://www.jd.com') 

//定位到网页搜索框id，并清空
key_button=browser.find_element_by_css_selector('#key')

//输入关键词
key_button.send_keys('固态硬盘')  

//点击搜索按钮
cilck_button=browser.find_element_by_css_selector('#search button.button').click()
//注意这里先找到button标签下属性为button的类名，在往上找到其独一无二的id'search'。
```

![](https://image--1.oss-cn-shenzhen.aliyuncs.com/Snipaste_2020-02-29_23-06-26.png)

### 2.2.2 Xpath选择器

`.find_element_by_xpath()`

```python
key_button=browser.find_element_by_xpath('//*[@id="key"]')

key_button.send_keys('固态硬盘')

cilck_button=browser.find_element_by_xpath('//*[@id="search"]//div/button').click()
```

### 2.2.3 其他方式

根据类名定位元素：`.find_element_by_id()`

根据类名定位元素：`.find_element_by_class_name()`

根据标签名定位元素：`.find_element_by_tag_name()`

根据元素名定位元素：`.find_element_by_name()`

通过文本**链接**来定位元素：`.find_element_by_link_text()`

通过文字**链接**中的一部分文字定位，属于**模糊定位**：`.find_element_by_partial_link_text()`

```
browser.find_element_by_link_text('把百度设为主页').get_attribute('id')
browser.find_element_by_partial_link_text('把百度').get_attribute('id')
```

## 2.3 渲染

在浏览器搜索关键词跳转到新页面之后，需要有一个渲染过程，等页面加载出来。

### 2.3.1 页面等待

现在的网页越来越多采用了 Ajax 技术，这样程序便不能确定何时某个元素完全加载出来了。如果实际页面等待时间过短导致某个dom元素还没出来，但是你的代码直接使用了这个WebElement，那么就会抛出ElementNotVisibleException的异常。

为了避免这种元素定位困难而且会提高产生 ElementNotVisibleException 的概率。所以 Selenium 提供了两种等待方式：

- 一种是隐式等待
- 一种是显式等待

1. **隐式等待**

   隐式等待比较简单，就是简单地设置一个等待时间，单位为秒。隐式等待是等页面加载完毕，而不是元素加载！！！（隐式等待就是针对页面的，显式等待是针对元素的。）

   ```python
   browser.implicitly_wait(10)    //最多等待10秒
   ```

   隐式等待只需设置一次，后面的都遵循这个规则，不像`time.sleep`。`time.sleep`属于强制等待。

2. **显式等待**

   显式等待指定某个条件，然后设置最长等待时间。如果在这个时间还没有找到元素，还没有满足某个条件，那么便会抛出异常了。显式等待是等元素加载！！！

   ```python
   ///
   WebDriverWait:
       WebDriverWait(driver， timeout) 实例化的driver  timeout等待的时间
   
   until:
       直到xx条件符合为止
       until(条件)
   
   条件:
       presence_of_element_located: 当前的元素被加载进来了
       presence_of_element_located是一个类 初始化要写定位的元素  一般是元组格式: (By.ID， 'vaule')
   
   ///
   import time
   # 导入模块
   from selenium import webdriver
   # 导入等待的显式等待的类
   from selenium.webdriver.support.ui import WebDriverWait  # WebDriverWait(driver)
   # 导入判断元素的条件
   from selenium.webdriver.support import expected_conditions as EC
   # 导入选择元素的方法
   from selenium.webdriver.common.by import By
   url = 'https://www.receivesmsonline.net/'
   browser = webdriver.Chrome()
   browser.get(url)
   try:
       startTime = time.time() # 计算时间戳
       print('开始进入等待时间---->>'， startTime)
   
       wait = WebDriverWait(driver， 5)
       # 一直去寻找(By.ID， 'asdhakhkfl')被加载进来 直到时间耗尽  如果提前找到 就提前返回
       element = wait.until(EC.presence_of_element_located((By.ID， 'asdhakhkfl')))
   
   except:
       endTime = time.time()
       print('等待了多少秒---->>'， endTime - startTime)
   ```
   
   下面是一些内置的等待条件，可以直接调用，而不用自己写这些等待条件：
   
   - `title_is`： 判断当前页面的title是否完全等于（==）预期字符串，返回布尔值
   - `title_contains` : 判断当前页面的title是否包含预期字符串，返回布尔值
- `presence_of_element_located` : 判断某个元素是否被加到了`dom`树里，并不代表该元素一定可见
   - `visibility_of_element_located` : 判断某个元素是否可见. 可见代表元素非隐藏，并且元素的宽和高都不等于0
- `visibility_of` : 跟上面的方法做一样的事情，只是上面的方法要传入`locator`，这个方法直接传定位到的element就好了
   - `presence_of_all_elements_located` : 判断是否至少有1个元素存在于`dom`树中。举个例子，如果页面上有n个元素的class都是`column-md-3`，那么只要有1个元素存在，这个方法就返回True
   - `text_to_be_present_in_element` : 判断某个元素中的text是否 包含 了预期的字符串
   - `text_to_be_present_in_element_value` : 判断某个元素中的value属性是否 包含 了预期的字符串
   - `frame_to_be_available_and_switch_to_it` : 判断该`frame`是否可以`switch`进去，如果可以的话，返回`True`并且`switch`进去，否则返回`False`
   - `invisibility_of_element_located` : 判断某个元素中是否不存在于`dom`树或不可见
   - `element_to_be_clickable` : 判断某个元素中是否可见并且是`enable`的，这样的话才叫`clickable`
   - `staleness_of` : 等某个元素从`dom`树中移除，注意，这个方法也是返回True或False
   - `element_to_be_selected` : 判断某个元素是否被选中了，一般用在下拉列表
   - `element_selection_state_to_be` : 判断某个元素的选中状态是否符合预期
   - `element_located_selection_state_to_be` : 跟上面的方法作用一样，只是上面的方法传入定位到的`element`，而这个方法传入`locator`
   - `alert_is_present` : 判断页面上是否存在`alert`

### 2.3.2 切换窗口

- **切换Frame**

  虽然selenium控制浏览器打开了网页，但是有的网页是由多个Frame结构组成的，我们需要切换到目标Frame里才能提取到想要的网页元素。

  ```
  browser.switch_to.frame(0)    // 切入目标框内
  ```

  frame标签有frameset、frame、iframe三种，frameset跟其他普通标签没有区别，不会影响到正常的定位，而frame与iframe对selenium定位而言是一样的，selenium有一组方法对frame进行操作。

  **从frame中切回主文档(switch_to.default_content())**

  **嵌套frame的操作(switch_to.parent_frame())**

- **切换浏览器窗口：**

```
browser.switch_to_window(browser.window_handles[-1])   // 切入浏览器窗口
```

### 2.3.3 下拉与翻页

有时候网页元素的逐步加载的，首先只加载网页上部分区域，等到拉动网页的下滑条时，才会逐步往下加载可视区域。**只有网页元素加载出来，才能进行选取**。

**执行JS代码，实现下拉滑动条：**

```
// 执行js代码 下拉滑动条
js = 'window.scrollBy(0,8000)'

// 执行js
browser.execute_script(js)
///
注意:
    对于含有iframe的框也需要先切换进入框内才可以下拉。
///
```

`window.scrollBy(x,y)`， 在当前位置的基础上，再次移动x,y像素

`window.scrollTo(x,y)`，将滚动条移动到 横坐标为x，纵坐标为y的位置

**翻页：**

```
time.sleep(5)   //等待5秒

//找到下一页位置进行点击（以京东为例）
browser.find_element_by_css_selector('#J_bottomPage a.pn-next').click()
```

## 2.4 页面操作

除了上面已经提到的【点击 & 输入文本动作】，selenium还提供了其他丰富的操作方式。

### 2.4.1 选择下拉框

已经知道了怎样向文本框中输入文字，但是有时候会碰到 <select> </select> 标签的下拉框。直接点击下拉框中的选项不一定可行。Selenium专门提供了Select类来处理下拉框。 其实 WebDriver 中提供了一个叫 Select 的方法，可以帮助完成这些事情：http://www.jq22.com/demo/shengshiliandong/

```python
///
下拉框:
    Select(element) element是下拉框的元素
    选择的方法:
        1. select_by_value(value)  value="天津市"
        2. select_by_index(1)  通过索引 1 2 3 4 5 6
        3. select_by_visible_text(text) 通过可见的文本
///
import time
from selenium import webdriver
from selenium.webdriver.support.select import Select

url = 'http://www.jq22.com/demo/shengshiliandong/'

# 初始化浏览器
driver = webdriver.Chrome()
# 打开
driver.get(url)

# 寻找可以选择的元素 并实例化Select

elememt = driver.find_element_by_xpath('//*[@id="s_province"]')

# 实例化Select
select = Select(elememt)
time.sleep(2)
# 选择具体的值
select.select_by_index(5)
time.sleep(2)
select.select_by_value('河南省')
time.sleep(2)
select.select_by_visible_text('四川省')
# %%
driver.quit()

```

以上是三种选择下拉框的方式，它可以根据索引来选择，可以根据值来选择，可以根据文字来选择。注意：

- index 索引从 0 开始
- value是option标签的一个属性值，并不是显示在下拉框中的值
- visible_text是在option标签文本的值，是显示在下拉框的值

### 2.4.2 鼠标动作链

有些时候，需要在页面上模拟一些鼠标操作，比如双击、右击、拖拽甚至按住不动等，可以通过导入 ActionChains 类来做到：http://www.runoob.com/try/try.php?filename=jqueryui-api-droppable  比如：	

```python
///
拖动鼠标:
    ActionChains:
        ActionChains() --> 直接传入driver --> ActionChains(driver) 实例化
        perform --> 执行动作
        drag_and_drop(source， target)  source拖动的元素  target元素被放置的位置

///
import time
from selenium import webdriver
from selenium.webdriver import ActionChains



# 网址
url = 'http://www.runoob.com/try/try.php?filename=jqueryui-api-droppable'

# 初始化
driver = webdriver.Chrome()
# 打开网站
driver.get(url)

# 切入框内
driver.switch_to.frame(0)

# 找到可以拖拽的元素
drag = driver.find_element_by_css_selector('#draggable')
# 需要放置的位置的元素
drop = driver.find_element_by_css_selector('#droppable')

# 实例化
action = ActionChains(driver)
# 定义动作 但是不执行 没有执行
action.drag_and_drop(drag， drop)
# 执行
action.perform()
time.sleep(5)
driver.quit()

///
注意:
    perform才是真正的执行  可以在perform之前定义多个动作 最后一起执行
    注意切入框内 switch_to.frame(0)
///
```

### 2.4.3 其他 (了解)

- 弹窗处理

当你触发了某个事件之后，页面出现了弹窗提示，处理这个提示或者获取提示信息方法如下：`driver.switch_to_alert()`

- 窗口切换

一个浏览器肯定会有很多窗口，所以我们肯定要有方法来实现窗口的切换。切换窗口的方法如下：`switch_to.window("this is window name")`

- 页面前进和后退

操作页面的前进和后退功能：前进：`driver.forward()` 后退：`driver.back()`

- Cookie

获取页面每个Cookie值：`driver.get_cookies()`