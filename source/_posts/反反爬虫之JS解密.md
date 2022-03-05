---
title: 反反爬虫之JS解密
categories: 爬虫
HTML格式实现标签样本: <center><font size=5 face="黑体">文本区</font></center>
date: 2020-03-24 10:04:43
top:
tags:
typora-root-url: ..
---



对传输的数据进行JS加密是网站常用的加密方式，以防止爬虫的爬取，以下对有道翻译进行破解示例。

# 一、手动破解

打开有道翻译：http://fanyi.youdao.com/

输入翻译内容：你好，世界

打开开发者调试工具：

【Network】—刷新—重新输入需要翻译的内容

在【XHR】过滤后，可看到【Js】传来的数据：

![](https://image--1.oss-cn-shenzhen.aliyuncs.com/Snipaste_2020-03-24_10-20-51.png)

在【Headers】里可以看到请求的【url，请求方式】：![](https://image--1.oss-cn-shenzhen.aliyuncs.com/Snipaste_2020-03-24_10-23-37.png)

再往下翻，可以看到发送的表单数据：

![](https://image--1.oss-cn-shenzhen.aliyuncs.com/Snipaste_2020-03-24_10-25-34.png)

这里面是重点！

重新输入翻译内容，再对比看看两份表单数据：

![](https://image--1.oss-cn-shenzhen.aliyuncs.com/Snipaste_2020-03-24_10-28-53.png)

以上红框里四个参数是关键，需要搞清楚它是怎么产生的。

【ctrl+f】打开搜索框，搜索【salt】参数

![](https://image--1.oss-cn-shenzhen.aliyuncs.com/Snipaste_2020-03-24_10-37-46.png)

搜到了【fanyi.min.js】，这就说明【salt】【sign】【ts】【bv】可能是在这里面产生的。

![](https://image--1.oss-cn-shenzhen.aliyuncs.com/Snipaste_2020-03-24_10-43-48.png)

显示内容：

![](https://image--1.oss-cn-shenzhen.aliyuncs.com/Snipaste_2020-03-24_10-45-38.png)

为了便于分析查找，将【fanyi.min.js】数据复制到pycharm里。

在pycharm里新建【JavaScript file】，命名为【fanyi.min.js】，把数据复制进去。

【ctrl+f】打开搜索框，搜索【translate_o】关键词

因为这是数据传输的url：【Request URL: ==http://fanyi.youdao.com/translate_o?smartresult=dict&smartresult=rule==】

找到：

![](https://image--1.oss-cn-shenzhen.aliyuncs.com/Snipaste_2020-03-24_10-55-15.png)

这里面的（data: e）中的e是指查询或者说翻译参数，用户需要翻译的值。这个数据在函数c(e,t)里处理，故先ctrl+鼠标左键，点击函数名c进入：

<img src="https://image--1.oss-cn-shenzhen.aliyuncs.com/Snipaste_2020-03-24_11-08-55.png" style="zoom:80%;" />

可以看到我们需要的参数【salt】【sign】【ts】【bv】这些参数都跟r有关，ctrl+鼠标左键，点击r进入：

<img src="https://image--1.oss-cn-shenzhen.aliyuncs.com/Snipaste_2020-03-24_11-11-39.png" style="zoom:80%;" />

可以看到，r是由v.generateSaltSign(n)产生的，ctrl+鼠标左键，点击进入：

![](https://image--1.oss-cn-shenzhen.aliyuncs.com/Snipaste_2020-03-24_11-14-25.png)

可以看到，generateSaltSign是由r产生的，ctrl+鼠标左键，点击r进入：

![](https://image--1.oss-cn-shenzhen.aliyuncs.com/Snipaste_2020-03-24_11-16-45.png)

由此终于可以看到【ts】【bv】【sign】【salt】是怎么来的了。

【ts】与时间有关，再结合ts在表单数据里的值【ts: 1585016297455】，很容易知道它就是个时间戳，只不过是个13位数的时间戳。python里面可以得到：

```python
ts=str(int(time.time()*1000))
```

【bv】是由(navigator.appVersion）进行md5加密而来，(navigator.appVersion）其实就是User-Agent。

【sign】是由("fanyideskweb" + e + i + "Nw(nmmbP%A-r6U3EUn]Aj")进行md5加密而来，这里的e是翻译的查询参数，比如这里翻译的内容是"你好，世界"，e就是"你好，世界"；i就是salt。

【salt】是i，与r(即时间戳有关)，再结合salt在表单数据里的值【salt: 15850162974555】，很明显就知道salt是在ts时间戳后面再加了一位随机数。

```python
salt=ts+str(random.randint(0,9))  //字符串拼接
```

剩下的就是在python里面实现md5加密。

```python
import hashlib
def make_md5(string):
    string=string.encode('utf-8')
    md5=hashlib.md5(string).hexdigest()
    return md5

bv=make_md5("Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/79.0.3945.130 Safari/537.36")
sign=make_md5("fanyideskweb" + "你好，世界" + str(salt) + "Nw(nmmbP%A-r6U3EUn]Aj")
```

整个程序：

```python
import requests
import time
import random
import hashlib

def make_md5(string):
    string=string.encode('utf-8')
    md5=hashlib.md5(string).hexdigest()
    return md5

def translate(e):
    #特征值破解
    ts=str(int(time.time()*1000))
    salt=ts+str(random.randint(0,9))
    bv=make_md5('5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/75.0.3770.142 Safari/537.36')
    sign=make_md5("fanyideskweb" + e + salt + "n%A-rKaT5fb[Gy?;N5@Tj")

    formdata={
        "i": str(e),
        "from": 'AUTO',
         "to": "AUTO",
        "smartresult": "dict",
        "client": "fanyideskweb",
        "salt": salt,
        "sign": sign,
        "ts": ts,
        "bv": bv,
        "doctype": "json",
        "version": "2.1",
        "keyfrom": "fanyi.web",
        "action": "FY_BY_REALTlME"
    }
    headers={
        'Accept': 'application/json, text/javascript, */*; q=0.01',
        'Accept-Encoding': 'gzip, deflate',
        'Accept-Language': 'zh-CN,zh;q=0.9',
        'Connection': 'keep-alive',
        'Content-Length': '251',
        'Content-Type': 'application/x-www-form-urlencoded; charset=UTF-8',
        'Cookie': 'OUTFOX_SEARCH_USER_ID=289052185@10.108.160.18; OUTFOX_SEARCH_USER_ID_NCOO=469089922.02312773; DICT_UGC=be3af0da19b5c5e6aa4e17bd8d90b28a|; JSESSIONID=abczzcxuVriBO6I1nFq0w; ___rl__test__cookies=1567939951177',
        'Host': 'fanyi.youdao.com',
        'Origin': 'http://fanyi.youdao.com',
        'Referer': 'http://fanyi.youdao.com/',
        'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/75.0.3770.142 Safari/537.36',
        'X-Requested-With':'XMLHttpRequest',
    }
    res=requests.post("http://fanyi.youdao.com/translate_o?smartresult=dict&smartresult=rule",data=formdata,headers=headers).json()
    text=res['translateResult'][0][0]['tgt']
    return text

if __name__=='__main__':
    e=input('请输入需要翻译的词或句子：')
    result=translate(e)
    print(result)
```

# 二、JavaScript破解

原理：直接在网页调试工具中将实现JS加密的JavaScript代码整个赋值下来，在python中执行，这样就无需人为去破解。

## 2.1 搭建环境

如果想用python代码直接调用js的代码，则需要搭建js环境，并安装相关的库直接调用JS代码。

[PyExecJS](https://pypi.org/project/PyExecJS/) ，就是其中一个比较好的库。可以使用 python 运行 JavaScript 代码。

需要注意的是：这个库已经不再维护了，如果因为版本更新所导致的一些错误是无法修复的。

`PyExecJS` 的优点是不需要关心 JavaScript 环境。 特别是它可以在 Windows 环境下工作，而不需要安装额外的库。

缺点之一是性能。 通过文本传递 JavaScript 运行时，速度很慢。 另一个缺点是它不完全支持运行时特定的特性。

首先下载安装Node.js：https://nodejs.org/zh-cn/

然后安装python库：

```
pip install PyExecJS
```

## 2.2 配置Pycharm

目的：在pycharm中运行js代码。

在pycharm中安装插件Node.js：

![](https://image--1.oss-cn-shenzhen.aliyuncs.com/Snipaste_2020-03-24_13-29-37.png)

尝试运行：

```
//新建文件youdao.js，并输入以下代码
console.log('hello')
//运行输出
hello
```

如果提示没有编译器，则按下面配置：

![](https://image--1.oss-cn-shenzhen.aliyuncs.com/Snipaste_2020-03-24_13-37-20.png)

## 2.3 复制JS代码

上面已经找到了关键数据的加密方式，直接复制过来：

在pycharm里新建【JavaScript file】，命名为【fanyi.js】，把数据复制进去。

![](https://image--1.oss-cn-shenzhen.aliyuncs.com/Snipaste_2020-03-24_13-46-28.png)

这里面用到了n.md5函数，ctrl+鼠标左键，点击函数名找到位置：

![](https://image--1.oss-cn-shenzhen.aliyuncs.com/Snipaste_2020-03-24_13-56-54.png)

找到md5之后，发现里面的还有h(e),f(e)等函数，不可直接复制md5函数里的内容，应该向上找，找到包含md5的完整函数。

按缩进判断函数体：

![](https://image--1.oss-cn-shenzhen.aliyuncs.com/Snipaste_2020-03-24_14-00-27.png)

然后将断点之间的代码都复制到【fanyi.js】：

并删掉引用：e("./jquery-1.7").extend({

```javascript
var n = function (e, t) {
        return e << t | e >>> 32 - t
    }, r = function (e, t) {
        var n, r, i, a, o;
        return i = 2147483648 & e, a = 2147483648 & t, n = 1073741824 & e, r = 1073741824 & t, o = (1073741823 & e) + (1073741823 & t), n & r ? 2147483648 ^ o ^ i ^ a : n | r ? 1073741824 & o ? 3221225472 ^ o ^ i ^ a : 1073741824 ^ o ^ i ^ a : o ^ i ^ a
    }, i = function (e, t, n) {
        return e & t | ~e & n
    }, a = function (e, t, n) {
        return e & n | t & ~n
    }, o = function (e, t, n) {
        return e ^ t ^ n
    }, s = function (e, t, n) {
        return t ^ (e | ~n)
    }, l = function (e, t, a, o, s, l, c) {
        return e = r(e, r(r(i(t, a, o), s), c)), r(n(e, l), t)
    }, c = function (e, t, i, o, s, l, c) {
        return e = r(e, r(r(a(t, i, o), s), c)), r(n(e, l), t)
    }, u = function (e, t, i, a, s, l, c) {
        return e = r(e, r(r(o(t, i, a), s), c)), r(n(e, l), t)
    }, d = function (e, t, i, a, o, l, c) {
        return e = r(e, r(r(s(t, i, a), o), c)), r(n(e, l), t)
    }, f = function (e) {
        for (var t, n = e.length, r = n + 8, i = 16 * ((r - r % 64) / 64 + 1), a = Array(i - 1), o = 0, s = 0; s < n;) o = s % 4 * 8, a[t = (s - s % 4) / 4] = a[t] | e.charCodeAt(s) << o, s++;
        return t = (s - s % 4) / 4, o = s % 4 * 8, a[t] = a[t] | 128 << o, a[i - 2] = n << 3, a[i - 1] = n >>> 29, a
    }, p = function (e) {
        var t, n = "", r = "";
        for (t = 0; t <= 3; t++) n += (r = "0" + (e >>> 8 * t & 255).toString(16)).substr(r.length - 2, 2);
        return n
    }, h = function (e) {
        e = e.replace(/\x0d\x0a/g, "\n");
        for (var t = "", n = 0; n < e.length; n++) {
            var r = e.charCodeAt(n);
            if (r < 128) t += String.fromCharCode(r); else if (r > 127 && r < 2048) t += String.fromCharCode(r >> 6 | 192), t += String.fromCharCode(63 & r | 128); else if (r >= 55296 && r <= 56319) {
                if (n + 1 < e.length) {
                    var i = e.charCodeAt(n + 1);
                    if (i >= 56320 && i <= 57343) {
                        var a = 1024 * (r - 55296) + (i - 56320) + 65536;
                        t += String.fromCharCode(240 | a >> 18 & 7), t += String.fromCharCode(128 | a >> 12 & 63), t += String.fromCharCode(128 | a >> 6 & 63), t += String.fromCharCode(128 | 63 & a), n++
                    }
                }
            } else t += String.fromCharCode(r >> 12 | 224), t += String.fromCharCode(r >> 6 & 63 | 128), t += String.fromCharCode(63 & r | 128)
        }
        return t
    };
      md5=function (e) {
            var t, n, i, a, o, s, m, g, v, y = Array();
            for (e = h(e), y = f(e), s = 1732584193, m = 4023233417, g = 2562383102, v = 271733878, t = 0; t < y.length; t += 16) n = s, i = m, a = g, o = v, s = l(s, m, g, v, y[t + 0], 7, 3614090360), v = l(v, s, m, g, y[t + 1], 12, 3905402710), g = l(g, v, s, m, y[t + 2], 17, 606105819), m = l(m, g, v, s, y[t + 3], 22, 3250441966), s = l(s, m, g, v, y[t + 4], 7, 4118548399), v = l(v, s, m, g, y[t + 5], 12, 1200080426), g = l(g, v, s, m, y[t + 6], 17, 2821735955), m = l(m, g, v, s, y[t + 7], 22, 4249261313), s = l(s, m, g, v, y[t + 8], 7, 1770035416), v = l(v, s, m, g, y[t + 9], 12, 2336552879), g = l(g, v, s, m, y[t + 10], 17, 4294925233), m = l(m, g, v, s, y[t + 11], 22, 2304563134), s = l(s, m, g, v, y[t + 12], 7, 1804603682), v = l(v, s, m, g, y[t + 13], 12, 4254626195), g = l(g, v, s, m, y[t + 14], 17, 2792965006), m = l(m, g, v, s, y[t + 15], 22, 1236535329), s = c(s, m, g, v, y[t + 1], 5, 4129170786), v = c(v, s, m, g, y[t + 6], 9, 3225465664), g = c(g, v, s, m, y[t + 11], 14, 643717713), m = c(m, g, v, s, y[t + 0], 20, 3921069994), s = c(s, m, g, v, y[t + 5], 5, 3593408605), v = c(v, s, m, g, y[t + 10], 9, 38016083), g = c(g, v, s, m, y[t + 15], 14, 3634488961), m = c(m, g, v, s, y[t + 4], 20, 3889429448), s = c(s, m, g, v, y[t + 9], 5, 568446438), v = c(v, s, m, g, y[t + 14], 9, 3275163606), g = c(g, v, s, m, y[t + 3], 14, 4107603335), m = c(m, g, v, s, y[t + 8], 20, 1163531501), s = c(s, m, g, v, y[t + 13], 5, 2850285829), v = c(v, s, m, g, y[t + 2], 9, 4243563512), g = c(g, v, s, m, y[t + 7], 14, 1735328473), m = c(m, g, v, s, y[t + 12], 20, 2368359562), s = u(s, m, g, v, y[t + 5], 4, 4294588738), v = u(v, s, m, g, y[t + 8], 11, 2272392833), g = u(g, v, s, m, y[t + 11], 16, 1839030562), m = u(m, g, v, s, y[t + 14], 23, 4259657740), s = u(s, m, g, v, y[t + 1], 4, 2763975236), v = u(v, s, m, g, y[t + 4], 11, 1272893353), g = u(g, v, s, m, y[t + 7], 16, 4139469664), m = u(m, g, v, s, y[t + 10], 23, 3200236656), s = u(s, m, g, v, y[t + 13], 4, 681279174), v = u(v, s, m, g, y[t + 0], 11, 3936430074), g = u(g, v, s, m, y[t + 3], 16, 3572445317), m = u(m, g, v, s, y[t + 6], 23, 76029189), s = u(s, m, g, v, y[t + 9], 4, 3654602809), v = u(v, s, m, g, y[t + 12], 11, 3873151461), g = u(g, v, s, m, y[t + 15], 16, 530742520), m = u(m, g, v, s, y[t + 2], 23, 3299628645), s = d(s, m, g, v, y[t + 0], 6, 4096336452), v = d(v, s, m, g, y[t + 7], 10, 1126891415), g = d(g, v, s, m, y[t + 14], 15, 2878612391), m = d(m, g, v, s, y[t + 5], 21, 4237533241), s = d(s, m, g, v, y[t + 12], 6, 1700485571), v = d(v, s, m, g, y[t + 3], 10, 2399980690), g = d(g, v, s, m, y[t + 10], 15, 4293915773), m = d(m, g, v, s, y[t + 1], 21, 2240044497), s = d(s, m, g, v, y[t + 8], 6, 1873313359), v = d(v, s, m, g, y[t + 15], 10, 4264355552), g = d(g, v, s, m, y[t + 6], 15, 2734768916), m = d(m, g, v, s, y[t + 13], 21, 1309151649), s = d(s, m, g, v, y[t + 4], 6, 4149444226), v = d(v, s, m, g, y[t + 11], 10, 3174756917), g = d(g, v, s, m, y[t + 2], 15, 718787259), m = d(m, g, v, s, y[t + 9], 21, 3951481745), s = r(s, n), m = r(m, i), g = r(g, a), v = r(v, o);
            return (p(s) + p(m) + p(g) + p(v)).toLowerCase()
        }

var r = function (e) {
        var t = md5(navigator.appVersion), r = "" + (new Date).getTime(), i = r + parseInt(10 * Math.random(), 10);
        return {ts: r, bv: t, salt: i, sign: md5("fanyideskweb" + e + i + "Nw(nmmbP%A-r6U3EUn]Aj")}
    };

console.log(r('你好世界'))
//输出如下：
var t = md5(navigator.appVersion), r = "" + (new Date).getTime(), i = r + parseInt(10 * Math.random(), 10);
                    ^

ReferenceError: navigator is not defined
```

说明这里还有	navigator	这个参数还未定义。

在console中输入navigator，可得到数据：

![](https://image--1.oss-cn-shenzhen.aliyuncs.com/Snipaste_2020-03-24_14-25-13.png)

将navigator的参数以字典的形式添加到到代码中（其实只是需要键名为appVersion的值，即浏览器的UA）再运行：

提示以下错误：

```
 e = e.replace(/\x0d\x0a/g, "\n");
              ^
RangeError: Maximum call stack size exceeded
at String.replace (<anonymous>)
```

这是因为：

```javascript
var r = function (e) {……};
```

变量名r与之前的重复了，改掉就好。

![](https://image--1.oss-cn-shenzhen.aliyuncs.com/Snipaste_2020-03-24_14-32-47.png)

```
{
  ts: '1585031532017',
  bv: '901200199a98c590144a961dac532964',
  salt: '15850315320171',
  sign: 'e04455ff433514e20647a2a96c759bd0'
}

```

终于得到了想要的数据！

## 2.4 在python调用Js代码

```python
import execjs

with open('fanyi.min.js','r',encoding='utf-8') as f:
    js_code=f.read()
    
//使用compile加载JavaScript代码
ctx=execjs.compile(js_code)
//使用call调用对象
result=ctx.call('youdao','你好世界')
print(result)
//输出结果如下
{'ts': '1585032669319', 'bv': '901200199a98c590144a961dac532964', 'salt': '15850326693199', 'sign': '7b5bd55eb8c7c5f3c9ea99f9632109e4'}
```

# 三、JS混淆

## 3.1 利与弊

**站在网站开发者的角度**

1、是为了保护我们的前端代码逻辑
2、精简代码、加快传输

**站在爬虫者的角度**

1、增加了获取数据的难度
2、增加了获取数据的难度

## 3.2 混淆与反混淆

### 3.2.1 JS压缩

![](https://image--1.oss-cn-shenzhen.aliyuncs.com/image-20191225190455160.png)

特点：把JS代码写在一起。让你难以阅读，难以破解

削减是一个从源代码中删除不必要的字符的技术使它看起来简单而整洁。这种技术也被称为代码压缩和最小化。

常用压缩工具：
https://javascript-minifier.com/

破解方法-代码格式化：
http://tool.oschina.net/codeformat/js/

### 3.2.2 eval加密

js中的 eval() 方法就是一个 js 语言的执行器，它能把其中的参数按照 JavaScript 语法进行解析并执行，简单来说就是把原本的 js 代码变成了 eval 的参数，变成参数后代码就成了字符串，其中的一些字符就会被按照特定格式“编码”。

**特征：**

最明显的特征是生成的代码以`eval(function(p,a,c,k,e,r))`开头。

<img src="https://image--1.oss-cn-shenzhen.aliyuncs.com/image-20191225190543248.png" style="zoom:100%;" />

**原理：**

这类混淆的关键思想在于将需要执行的代码进行一次编码，在执行的时候还原出浏览器可执行的合法 的脚本

破解方法-浏览器
打开 谷歌 或者 火狐 浏览器
按 F12 打开控制台
把代码复制进去
删除开头 eval 这4个字母
按回车键

常用混淆工具：
http://dean.edwards.name/packer/

### 3.2.3 公钥加密关键字段

**特征：**

![](https://image--1.oss-cn-shenzhen.aliyuncs.com/image-20191225190622791.png)

**原理：**
用户访问客户端，客户端向服务器请求获取一个RSA公钥以及键值key，存储在本地
用户在本地公钥失效前发起登录请求，则使用已有公钥对用户密码进行加密；若已过期则执行1后 再加密
客户端将密文与key一起传回后台
后台通过key找到缓存里面的私钥，对密文进行解密

常用混淆工具：
http://travistidwell.com/jsencrypt/

### 3.2.4 变量名混淆

**特征：**

![](https://image--1.oss-cn-shenzhen.aliyuncs.com/image-20191225190656223.png)

**原理**

字符串字面量混淆：首先提取全部的字符串，在全局作用域创建一个字符串数组，同时转义字符增大 阅读难度，然后将字符串出现的地方替换成为数组元素的引用
变量名混淆：不同于压缩器的缩短命名，此处使用了下划线加数字的格式，变量之间区分度很低，相 比单个字母更难以阅读
成员运算符混淆：将点运算符替换为字符串下标形式，然后对字符串进行混淆 删除多余的空白字符：减小文件体积，这是所有压缩器都会做的事。

常用混淆工具：
http://javascriptobfuscator.com/Javascript-Obfuscator.aspx
http://js.51tools.info/
破解方法-IDE、解密工具、浏览器：  http://jsnice.org/  http://js.51tools.info/

（1）把变量名、函数名、参数名等，替换成没有语义，看着又很像的名字。

```
_0x21dd83、_0x21dd84、_0x21dd85
```

（2）用十六进制文本去表示一个字符串

```
\x56\x49\x12\x23
```

（3）利用JS能识别的编码来做混淆。JS是Unicode编码，本身就能识别这种编码。类似的一些变量名，函数名都可以用这个表示，并且调用。

  类似： `\u6210\u529f` 表示中文字符(成功)。

  类似：`\u0053\u0074\u0072\u0069\u006e\u0067.\u0066\u0072\u006f\u006d\u0043\u0068\u0061\u0072\u0043\u006f\u0064\u0065`  

代表String.fromCharCode

  类似：

  `('')['\x63\x6f\x6e\x73\x74\x72\x75\x63\x74\x6f\x72']['\x66\x72\x6f\x6d\x43\x68\x61\x72\x43\x6f\x64\x65'];` 

效果等同于String.fromCharCode

（4）把一大堆方法名、字符串等存到数组中，这个数组可以是上千个成员。然后调用的时候，取数组成员去用

```javascript
var arr = ["Date","getTime"];
var time = new window[arr[0]]()[arr[1]]();
console.log(time);
```

（5）字符串加密后发送到前端，然后前端调用对应的函数去解密，得到明文

```javascript
var arr = ['xxxx']

// 定义的解密函数
function dec(str){
  return 'push'
}
test[dec(arr[0])](200);
```

### 3.2.5 控制流平坦化

将顺序执行的代码混淆成乱序执行,并加以混淆

以下两段代码的执行结果是相同的:

```javascript
// 正常形态
function test(a){
	var b = a;
    b += 1;
    b += 2;
    b += 3;
    b += 4;
    return a + b
}

// 乱序形态
//（这里比较简单,在很多加密网站上case 后面往往不是数字或字符串,而是类似 YFp[15][45][4]这样的对象，相当恶心）
function test1(a){
  var arr = [1,2,3,4,5,6]
  for(var i = 0, i < arr.lenght, i++){
    switch (arr[i]) {
      case 4:
        b += 3;
        break;
      case 2:
        b += 1;
      break;
      case 1:
        var b = a;
      break;
      case 3:
        b += 2;
      break;
      case 6:
        return a + b
      case 5:
        b += 4;
      break;
    }
  }
}
// 结果都是30 但是test1看着费劲
console.log(test1(10));
console.log(test(10));
```

### 3.2.6 使用特定符号编写js脚本

**特征：**

![](https://image--1.oss-cn-shenzhen.aliyuncs.com/image-20191225190724159.png)

**原理：**
jsfuck源于一门编程语言brainfuck，其主要的思想就是只使用8种特定的符号来编写代码。而jsfuck
也是沿用了这个思想，它仅仅使用6种符号来编写代码。它们分别是(、)、+、[、]、!。

常用混淆工具：
http://www.jsfuck.com/
破解方法： 超级难

### 3.2.7 特殊转化规则

利用一些只能在浏览器中运行的特殊语句进行反扒，这种只能将语法重新进行改写。

在浏览器中base64编码转换使用的是

`_0x1c0cdf = _0xcbc80b['atob'](_0x1c0cdf)`,

但是在nodejs调试的时候使用的是

```
Buffer.from(_0x1c0cdf,"base64").toString()
```



# 四、练手项目

（1）获取一品威客登录页面，找到密码的加密方式

https://www.epwk.com/login.html

（2）获取电信官网登录页面，找到密码的加密方式

https://login.189.cn/web/login

（3）梦幻西游装备属性数据的加密方式

[https://xyq.cbg.163.com/equip?s=40&eid=202002011900113-40-NXOJDHZ8TKFX9&o&equip_refer=27&view_loc=reco_sim|%7B%22tag%22%3A%20%22RL_sim2%22%7D](https://xyq.cbg.163.com/equip?s=40&eid=202002011900113-40-NXOJDHZ8TKFX9&o&equip_refer=27&view_loc=reco_sim|{"tag"%3A "RL_sim2"})


