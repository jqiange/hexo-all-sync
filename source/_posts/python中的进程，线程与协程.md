---
title: python中的进程，线程与协程
categories: python
HTML格式实现标签样本: <center><font size=5 face="黑体">文本区</font></center>
mathjax: true
date: 2020-11-04 21:29:16
top:
tags:
typora-root-url: ..
---



# GIL全局解释器锁

在提到进程、线程和协程前，不得不提下**GIL**([Global Interpreter Lock](https://zhuanlan.zhihu.com/p/97218985))，全局解释器锁。

**GIL**是一个互斥锁（mutex），是CPython（Python解释器）限制了同一时间内，一个进程里只能有一个线程运行。它阻止了 多个线程同时执行Python字节码，毫无疑问，这降低了执行效率。

Python最初的设计理念在于，**为了解决多线程之间数据完整性和状态同步的问题，设计为在任意时刻只有一个线程在解释器中运行。**而当执行多线程程序时，由GIL来控制同一时刻只有一个线程能够运行。即Python中的多线程是表面多线程，也可以理解为‘假’多线程，不是真正的多线程。

为什么要这样做呢？举个例子，比如用python计算：n=n+1。这个操作被分成了四步：

- 加载全局变量n
- 加载常数1
- 进行二进制加法运算
- 将运算结果存入变量n

以上的过程是非原子操作的，根据前面的线程释放GIL锁原则，线程a执行这四步的过程中，有可能会让出GIL。如果这样，n=n+1的运算过程就被打乱了。

这就是为什么我们说GIL是粗粒度的，它只保证了一定程度的安全。如果要做到线程的绝对安全，是不是所有的非IO操作，我们都需要自己再加一把锁呢？答案是否定的。在python中，有些操作是是原子级的，它本身就是一个字节码，GIL无法在执行过程中释放。对于这种原子级的方法操作，我们无需担心它的安全。比如sort方法，[1,4,2].sort()，翻译成字节码就是CALL METHOD 0。只有一行，无法再分，所以它是线程安全的。

同一时刻只有一个线程能够运行，那么是怎么执行多线程程序的呢？其实原理很简单：解释器的**分时复用**。即多个线程的代码，**轮流**被解释器**执行**，只不过切换的很频繁很快，给人一种多线程“同时”在执行的错觉。聊的学术化一点，其实就是“**并发**”。

**“并发”和“并行”：**

- 并发：不同的代码块交替执行
- 并行：不同的代码块同时执行

**GIL锁最终是保证Python解释器中原子操作的线程安全**。

**GIL是怎么起作用的：**

- 由于GIL的机制，单核CPU在同一时刻只有一个线程在运行，当线程遇到IO（读写）操作或Timer  Tick到期，释放GIL锁。其他的两个线程去竞争这把锁，得到锁之后，才开始运行。
- 线程释放GIL锁有两种情况，一是遇到IO操作，二是Time Tick到期（执行完100个字节码指令或者15ms）。IO操作很好理解，比如发出一个http请求，等待响应。而Time Tick规定了线程的最长执行时间，超过时间后自动释放GIL锁。

在多核CPU下，由于GIL锁的全局特性，无法发挥多核的特性，GIL锁会使得多线程任务的效率大大降低。线程1（Thread1）在CPU1上运行，线程2（Thread2）在CPU2上运行。GIL是全局的，CPU2上的Thread2需要等待CPU1上的Thread1让出GIL锁，才有可能执行。如果在多次竞争中，Thread1都胜出，Thread2没有得到GIL锁，意味着CPU2一直是闲置的，无法发挥多核的优势。为了避免同一线程霸占CPU，在python3.x中，线程会自动的调整自己的优先级，使得多线程任务执行效率更高。

**GIL的优缺点：**

- GIL的优点是显而易见的，GIL可以保证我们在多线程编程时，无需考虑多线程之间数据完整性和状态同步的问题。

- GIL缺点是：我们的多线程程序执行起来是“并发”，而不是“并行”。因此执行效率会很低，会不如单线程的执行效率。

**原子操作：**

- 原子操作就是不会因为进程并发或者线程并发而导致被中断的操作。**原子操作**的特点就是**要么一次全部执行，要么全不执行**。不存在执行了一半而被中断的情况。

**Python解释器：**

- python解释器是有多个版本的：CPython, Jpython等。CPython就是用C语言实现Python解释器，JPython是用Java实现Python解释器。那么 GIL的问题实际上是存在于CPython中的。

最初是为了利用多核，Python开始支持多线程。而解决多线程之间数据完整性和状态同步的最简单方法自然就是加锁。后来发现这种‘加锁’是低效的。但**当大家试图去拆分和去除GIL的时候，发现大量库代码开发者已经重度依赖GIL而非常难以去除了**。

**在Python编程中，如果想利用计算机的多核提高程序执行效率，用多进程代替多线程。**

使用多进程的好处：完全并行，无GIL的限制，可充分利用多cpu多核的环境。

虽说一般使用多进程对电脑系统资源占用比较多，但是在类unix系统中，创建线程的开销并不比进程小，因此在并发操作时，多线程的效率还是受到了很大制约的。所以后来人们发现通过yield来中断代码片段的执行，同时交出了cpu的使用权，于是协程的概念产生了。

# 进程、线程与协程

**进程（process）是系统资源分配的最小单位，线程（thread）是程序执行的最小单位**。

**而协程（Coroutine）不是被操作系统内核所管理，而完全是由程序所控制（也就是在用户态执行）。**

![](/assets/image-20201105220526734.png)

一个程序（进程）在计算机上运行时，操作系统会以进程为单位，分配系统资源（CPU时间片、内存等资源），当这个进程存在多个线程时，由于GIL锁，系统资源的红箭头会随机指向其中一个进程，供其使用。遇到IO操作或者Time Tick到期（执行完100个字节码指令或者15ms），该线程被设置成睡眠状态，红箭头就又会随机重新指向其中一个线程执行（按优先级），这就是多线程。

协程的概念应该是从进程和线程演变而来的，协程其实并不真正存在，它只是人为设想的一种产物，由程序或用户可随意切换执行。协程在子程序内部是可中断的，然后转而执行别的子程序，在适当的时候再返回来接着执行。

**协程的特点在一个线程中执行，那和多线程比，协程有何优势？**

- **极高的执行效率**：因为**子程序切换不是线程切换，而是由程序自身控制**，因此，**没有线程切换的开销**，和多线程比，线程数量越多，协程的性能优势就越明显；
- **不需要多线程的锁机制**：因为只有一个线程，也不存在同时写变量冲突，**在协程中控制共享资源不加锁**，只需要判断状态就好了，所以执行效率比多线程高很多。

当你程序中方法需要等待时间的话，就可以用协程，效率高，消耗资源少。

python可以通过 yield/send 的方式实现**协程**。以此有程序员控制函数的中断与执行。

在Python3.4正式引入了协程的概念，代码示例如下：

```python
import asyncio

# Borrowed from http://curio.readthedocs.org/en/latest/tutorial.html.
@asyncio.coroutine
def countdown(number, n):
    while n > 0:
        print('T-minus', n, '({})'.format(number))
        yield from asyncio.sleep(1)
        n -= 1

loop = asyncio.get_event_loop()
tasks = [
    asyncio.ensure_future(countdown("A", 2)),
    asyncio.ensure_future(countdown("B", 3))]
loop.run_until_complete(asyncio.wait(tasks))
loop.close()
```

示例显示了在Python3.4引入两个重要概念**协程**和**事件循环**。
通过修饰符@asyncio.coroutine定义了一个协程，而通过event loop来执行tasks中所有的协程任务。

之后在Python3.5引入了新的async & await语法，从而有了原生协程的概念。

## 多进程示例

multiprocessing 是 Python 的标准模块，它既可以用来编写多进程，也可以用来编写多线程。multiprocessing  提供了一个 Process 类来代表一个进程对象，这个对象可以理解为是一个独立的进程，可以执行另外的事情。

```python
import multiprocessing       //导入多进程库
import time

def upload():
    print(" 开始上传文件...")
    time.sleep(5)
    print(" 完成上传文件...")

def download():
    print(" 开始下载文件...")
    time.sleep(2)
    print(" 完成下载文件...")

def main():
    start=time.time()
    #同时开启两个子进程
    multiprocessing.Process(target=upload).start()
    multiprocessing.Process(target=download).start()
    
    end=time.time()
    print('A总耗时：%s'%(end-start))

if __name__ == '__main__':
    begin = time.time()
    
    main()
    
    stop=time.time()
    
    print('B总耗时：%s'%(stop-begin))
    
#输出结果：
A总耗时：0.02892470359802246
B总耗时：0.02892470359802246
 开始上传文件...
 开始下载文件...
 完成下载文件...
 完成上传文件...

```

这里面相当于有三个进程，该程序的这个主进程加上两个子进程upload和download。从运行结果看，先输出了A和B的总耗时，这个是属于主进程的，因为耗时少，先运行完先输出，upload和download两个子进程因为time.sleep()的存在，先后输出。upload和download谁先执行不一定的。

## 多线程示例

当一个进程启动之后，会默认产生一个主线程，因为线程是程序执行流的最小单元，当设置多线程时，主线程会创建多个子线程，在 python  中，主线程执行完自己的任务以后，就退出了，此时子线程会继续执行自己的任务，直到自己的任务结束。

```python
import threading
import time

def target():
    print("%s is runing"%(threading.current_thread().name))
    time.sleep(2)
    print("%s is ended"%(threading.current_thread().name))

print("%s is runing"%(threading.current_thread().name))   #主程序开始

t = threading.Thread(target=target)   
t.start()

# t.join() # t.join是阻塞当前线程(此处的当前线程是主线程) 
#可以使主线程直到Thread-1结束之后才结束
print("%s is ended"%(threading.current_thread().name))

#输出结果为：
MainThread is runing
Thread-1 is runing
MainThread is ended
Thread-1 is ended
```

## 进程池与线程池

均采用`concurrent.futures`模块。池的好处是，对于多个的进程/线程，它能进行合理调控，比如可以设置同时只能进行5个任务，避免python占用过多的电脑资源，当运行中的5个任务完成了其中3个，池会自动进行补调，保证同时5个任务的进行。

**进程池**

- ```python
  import concurrent.futures
  process_pool=concurrent.futures.ProcessPoolExecutor(max_workers=) //创建进程池
  process_pool.submit(tasks, arg*)    //提交任务
  process_pool.shutdown()
  ```

- 示例：

  ```python
  import concurrent.futures
  
  def download_one_page(url, name):
      print('{}对应的{}下载完毕'.format(url,name))
  
  process_pool=concurrent.futures.ProcessPoolExecutor(max_workers=5) // 设置最多子任务为 5
  
  for url, name in result:  # 每一次循环都启动一个新的线程
      host_url = 'http://www.shuquge.com/txt/8659/'
      process_pool.submit(download_one_page, host_url+url, name)
  
  process_pool.shutdown()
  ```

  

**线程池**

- ```python
  thread_pool=concurrent.futures.ThreadPoolExecutor(max_workers=)
  thread_pool.submit(tasks, arg*)
  thread_pool.shutdown()
  ```

- 示例：

  ```python
  import concurrent.futures
  
  def download_one_page(url, name):
      print('{}对应的{}下载完毕'.format(url,name))
      
  thread_pool=concurrent.futures.ThreadPoolExecutor(max_workers=5) // 设置最多子任务为 5
  
  for url, name in result:  # 每一次循环都启动一个新的线程
      host_url = 'http://www.shuquge.com/txt/8659/'
      thread_pool.submit(download_one_page, host_url + url, name)
  
  thread_pool.shutdown()
  ```

  以上程序也可这样写：

  ```python
  #最简单方法是作为上下文管理器，使用 with 语句来管理池的创建和销毁。
  
  import concurrent.futures
  
  if __name__ == "__main__":
      with concurrent.futures.ThreadPoolExecutor(max_workers=3) as executor:
           for url, name in result:  # 每一次循环都启动一个新的线程
               host_url = 'http://www.shuquge.com/txt/8659/'
               executor.map(download_one_page, host_url + url, name)
  ```

### 多进程、多线程爬取小说

```python
import requests
import re
import time
import concurrent.futures

def get_index():
    response = requests.get('http://www.shuquge.com/txt/8659/index.html')
    response.encoding = response.apparent_encoding
    html = response.text
    result = re.findall('<dd><a href="(.*?)">(.*?)</a></dd>', html)
    return result

# 多线程
def thread_download_ebook(url, name):
    print(name, url)
    response = requests.get(url)
    response.encoding = response.apparent_encoding
    html = response.text
    result = re.findall('<div id="content" class="showtxt">(.*?)</div>', html, re.S)
    with open(name + '.txt', mode='w', encoding='utf-8') as f:
        f.write(result[0].replace('<br/>&nbsp;&nbsp;&nbsp;&nbsp;', "").replace('<br/>', ""))

# 多进程
def process_download_ebook(urls):
    # 每一个进程 启动五个线程 25
    thread_pool = concurrent.futures.ThreadPoolExecutor(max_workers=5)
    for url, name in urls:
        # 往线程池里面放任务
        thread_pool.submit(thread_download_ebook, 'http://www.shuquge.com/txt/8659/' + url, name)
    # 等待线程池关闭
    thread_pool.shutdown()

if __name__ == '__main__':
    content_list = get_index()[:20]
    length = int(len(content_list)/5)  # 把所有任务分成五份
    start_time = time.time()
    # 启动五个进程
    # 进程之间的相互通信 默认情况下 进程之间的变量不共享数据
    process_pool = concurrent.futures.ProcessPoolExecutor(max_workers=length)
    for i in range(length):
        if i == length:
            i += 1
        process_pool.submit(process_download_ebook, content_list[i * length:(i + 1) * length])
    process_pool.shutdown()
    print(time.time() - start_time)
```



## 协程示例

async/await关键字是出现在python3.5版本中的新功能，是一种关于**协程**的语法糖。从此python就正式有了原生协程的概念。

正常的函数在执行时是不会中断的，所以你要写一个能够中断的函数，就需要添加async关键词。

`async` 用来声明一个函数为异步函数，异步函数的特点是能在函数执行过程中挂起，去执行其他异步函数，等到挂起条件消失后，再回来执行。

`await` 用来声明程序挂起，比如异步程序执行到某一步时需要等待的时间很长，就将此挂起，去执行其他的异步程序，await 后面只能跟异步程序函数或有`__await__`属性的对象，也就是说await表达式中的对象必须是awaitable的。

awaitable对象必须满足如下条件中其中之一：

- **原生协程对象**
- types.coroutine()修饰的**基于生成器的协程对象**
- 实现了await method，并在其中返回了**iterator的对象（可迭代对象）**

**举例一：**

```python
def main():
    
    //1、定义异步函数
    async def funcA():             #声明funcA为一个异步函数（或者叫协程函数）
        await asyncio.sleep(4)
        print('A函数执行完毕')

    async def funcB():              #定义的协程函数就是原生协程对象
        await asyncio.sleep(2)
        print('B函数执行完毕')

    async def funcD():
        await asyncio.sleep(8)
        print('D函数执行完毕')

    //2、创建一个事件循环    
    loop = asyncio.get_event_loop() 

    //3、将异步函数加入事件队列
    tasks=[funcA(),funcB(),funcD()]

    //4、执行事件队列, 直到最晚的一个事件被处理完毕后结束
    loop.run_until_complete(asyncio.wait(tasks))

    //5、如果不再使用 loop, 建议养成良好关闭的习惯
    loop.close()



if __name__=='__main__':
    start=time.time()
    main()
    end=time.time()
    print('总耗时为：'+ str(end-start))
    
#输出结果为：
B函数执行完毕
A函数执行完毕
D函数执行完毕
总耗时为：8.006016969680786s

```

**举例二：**

```python
import asyncio
import requests
import time


async def download(url): 
    print("get %s" % url)    
    response = requests.get(url)
    print(response.status_code)


async def wait_download(url):
    await download(url)        # 这里download(url)就是一个原生的协程对象
    print("get {} data complete.".format(url))


async def main():
    start = time.time()
    await asyncio.wait([
        wait_download("http://www.163.com"),
        wait_download("http://www.mi.com"),
        wait_download("http://www.baidu.com")])
    end = time.time()
    print("Complete in {} seconds".format(end - start))


loop = asyncio.get_event_loop()
loop.run_until_complete(main())

#运行结果：
get http://www.163.com
200
get http://www.163.com data complete.
get http://www.baidu.com
200
get http://www.baidu.com data complete.
get http://www.mi.com
200
get http://www.mi.com data complete.
Complete in 0.49027466773986816 seconds
```

程序可以运行，不过仍然有一个问题就是：它并没有真正地异步执行。

这里程序始终是同步执行的，这就说明仅仅是把涉及I/O操作的代码封装到async当中是不能实现异步执行的。必须使用支持异步操作的非阻塞代码才能实现真正的异步。目前支持非阻塞异步I/O的库是aiohttp。

```python
import asyncio
import aiohttp
import time


async def download(url): # 通过async def定义的函数是原生的协程对象
    print("get: %s" % url)
    async with aiohttp.ClientSession() as session:
        async with session.get(url) as resp:
            print(resp.status)
            # response = await resp.read()

# 此处的封装不再需要
# async def wait_download(url):
#    await download(url) 
#    print("get {} data complete.".format(url))


async def main():
    start = time.time()
    await asyncio.wait([
        download("http://www.163.com"),
        download("http://www.mi.com"),
        download("http://www.baidu.com")])
    end = time.time()
    print("Complete in {} seconds".format(end - start))


loop = asyncio.get_event_loop()
loop.run_until_complete(main())

#测试结果：
get: http://www.mi.com
get: http://www.163.com
get: http://www.baidu.com
200
200
200
Complete in 0.27292490005493164 seconds
```

可以看出这次是真正的异步了。






