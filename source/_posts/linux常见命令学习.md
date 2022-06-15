---
title: linux常见命令学习
categories: Linux
HTML格式实现标签样本: <center><font size=5 face="黑体">文本区</font></center>
mathjax: false
date: 2020-06-17 22:33:28
top:
tags:
typora-root-url: ..
---



> 一个程序员连Linux命令都不会，还算什么程序员！
>
> 这里对常见的Linux命令做一个总结。

Linux，全称GNU/Linux，是一套免费使用和自由传播的类Unix操作系统，是一个基于POSIX和Unix的多用户、多任务、支持多线程和多CPU的操作系统。

一般linux系统基本上分两大类：

- RedHat系列：Redhat、Centos、Fedora等；
- Debian系列：Debian、Ubuntu等。

**类Unix系统目录结构**

![](https://image--1.oss-cn-shenzhen.aliyuncs.com/image-20201023215641344.png)

以上这个目录结构要牢记。

# 1、Linux常见命令

## 1.1 linux命令格式

```
command [-options] [parameter]
 命令       选项        参数
```

## 1.2 查看帮助信息

方式一：

```
command --help
```

方式二：

```
man [section] command
```

man是linux中的用户手册，包含的各个section意义如下：

- 1 – User Commands 一般用户命令
- 2 - System Calls 系统调用命令,如open,write之类的(通过这个，至少可以很方便的查到调用这个函数，需要加什么头文件)
- 3 - C library Functions C函数库命令,如printf,fread
- 4 - Devices and Special files 是特殊文件,也就是/dev下的各种设备文件 man hd
- 5 - File formats and conventions 是指文件的格式,比如man 5 passwd, 就会得到说明这个文件/etc/passwd中各个字段的含义
- 6 - games for linux是给游戏留的,由各个游戏自己定义
- 7 - Miscellanea 杂项， 例如宏命令包、惯例等。
- 8 - System administration tools and Deamons 是系统管理用的命令,这些命令只能由root使用,如ifconfig
- 9 -其他（Linux特定的）， 用来存放内核例行程序的文档。
- n 新文档， 可能要移到更适合的领域。
- o 老文档， 可能会在一段期限内保留。
- l 本地文档， 与本特定系统有关的。

## 1.3 自动补全

想要编辑某个文件时，若文件名太长，可采用先敲几个前面的字母，按tab键可自动补全。

2次tab键可补全显示目录下所有文件

按方向键上可出现上次输入的命令。

## 1.4 基础命令

- `ls` 显示当前目录下的文件

  ```
  ls          #显示当前目录下的文件
  ls /        #显示根目录下的文件
  ls /bin     #显示根目录下bin文件夹里的文件
  ls -a       #显示当前目录下的所有文件（包括隐藏文件，以.开头的文件名）
  ls -l		#以详细列表形式显示文件（文件大小显示以字节大小形式）
  ls -l -h    #以详细列表形式显示文件,同时显示文件大小（以k,M,G等单位）
  ls -alh     #（选项可以写在一起）具体每个字符表示什么，后面会2.6.3介绍
  
  ls --help  #查看所有用法
  ```

- `pwd` 显示当前所处位置（在哪个目录下）

- `cd 目录` 切换目录

  ```
  cd /bin  	#切换到根目录下的bin文件夹里
  cd ~     	#切换到家目录中的用户文件夹
  cd ./test   #切换到当前路径下的test文件夹里
  cd ..  		#切换到上级目录
  cd -   		#快速回到上次你所在的路径
  ```

- `touch 文件名` 创建文件

- `mkdir 文件夹名` 创建文件夹

  ```
  mkdir A 			#在当前目录下创建A文件夹
  mkdir A/B/C -p 		#在当前目录下创建A文件夹,再建B,再建C
  ```

- `rmdir 文件夹名` 删除文件夹，若文件夹不为空，则无法删除

- `rm 文件名` 删除文件/文件夹

  ```
  rm *.txt   #删除所有txt文件（空文件）
  rm A -r    #强制删除A文件 即使里面有内容
  ```

- `cat 文件名` 查看文件里的所有内容，可同时显示多个文件内容

  `cat > 文件名` 新建文件，并在换行后继续输入，将输入内容写进文件中，ctrl + d 结束输入

- `more 文件名` 滑动逐次查看内容（按Enter键滑动阅览，f键前翻页，b键后翻页，q键结束）

  `head 文件名` 查看前10行

  `tail 文件名` 查看后10行 

  `tail -f xx.log`动态查看（适用于log在随时更新的情况）

  `tail -50f xx.log`动态查看后50行

  整齐输出(表格化输出)：`column -t`

  如：`tail xx.csv |colunmn -t`

- `history` 查看已经使用过哪些命令

  ```
  ！编号    #执行history记录的编号对应的哪个命令
  ```

- `tree` 以目录树的形式显示当前路径下所有文件。这个命令需要安装。

  

## 1.5 通配符

linux里也支持正则表达式进行文件的搜索。

```
ls 1*.txt    #显示名字以1开头的所有txt文件
ls 1?3       #显示名字为1什么3的所有文件
ls 1[1-5]3 	 #显示名字为113或123或133或143或153的所有文件
```

## 1.6 重定向

```
ls > xx.txt  #把当前目录下ls显示的所有文件名或文件夹名写到xx.txt文件里
ls >> xx.txt  #把当前目录下ls显示的所有文件名或文件夹名追加写到xx.txt文件里
cat 1.txt 2.txt > xxx.txt  #把1.txt和2.txt内容合并到xxx.txt里
```

## 1.7 管道



![](https://image--1.oss-cn-shenzhen.aliyuncs.com/image-20200618135249035.png)

以上的意思是ls -alh显示所有文件，将文件放到管道里，再调用more命令滑动逐次查看内容。

## 1.8 链接

分为软链接和硬链接

软链接：就是类似与快捷方式，指向源文件，加 -s 

硬链接：相当于给文件又给了个名字，不加 -s

**用法1：链接源文件到目标路径**

格式：ln -s 【源目录/文件】 【目标路径】

比如，将其他位置的1.txt文件，链接到我的目标文件夹的：

```
ln -s /home/qiang/file/test.txt ./    # 将1.txt链接到当前目录下
```

修改源目录/文件

```
ln -snf /home/qiang/file/test.py ./  
```

重命名：

在当前目录下，`mv test.py 1.py` 即可。



**用法2：创建快捷方式**

```
ln -s 1.txt 1-softlink.txt    #给1.txt创建软链接1-softlink.txt

ln 1.txt 1-hardlink.txt       #给1.txt创建硬链接1-hardlink.txt
```

当删除文件时，删除的是硬链接，当文件的硬链接为0时，就会自动删除文件。





## 1.9 查找

### 1.9.1 文本查找

```
grep 'ntfs' xx.txt     #在xx.txt里查找‘ntfs’
grep '^ntfs' xx.txt     #在xx.txt里查找开头为‘ntfs’的字符串

grep -v 'ntfs' xx.txt  # -v作用是取反，不包含‘ntfs’

grep -n 'ntfs' xx.txt  # -n作用是显示行数

grep -r 'internal_crop_0810.oas' ./   # 以递归的方式查找当前路径及子目录文件里指定的字符串

grep -i 'end process' test.py         # -i 是忽略查找字符的大小写
```

### 1.9.2 文件查找

`find 路径 形式 内容`。find默认递归指定目录。

形式：

- -name 按名字
- -iname 按名字，不区分大小写
- -size 按大小

```
find ./temp -name test.sh
find ./temp -name '*.sh'
find ./temp -name '[A-Z]*'

find ./temp -size 2M        #查找大小为2M的文件
find ./temp -size -2M		#查找小于2M的文件
find ./temp -size +2M		#查找大于2M的文件

find ./temp -size +2M -size -4M		#查找大于2M，小于4M的文件

find ./ -not -name 'plot*'  #取反，不包含
```

遇到权限问题，一律在最前面加`sudo`。

```
sudo find / -name *redis* -exec rm -rf {} \;  #查找根目录下所有包含redis的文件并删除
```



## 1.10 移动与拷贝

```
mv 1.txt 1_1.txt  #重命名1.txt为1_1.txt

mv 1.txt /laowang  #移动文件1.txt到laowang文件夹里

cp 1.txt /laowang  #复制文件1.txt到laowang文件夹里
```

若被拒绝，则加`-r`即可解决。

## 1.11 压缩与解压

`tar [参数] 打包文件名 需要打包的文件`



![](https://image--1.oss-cn-shenzhen.aliyuncs.com/image-20200618153443295.png)

```
tar -cvf test.tar *.py   #将所有py文件打包成test.tar

tar -xvf test   #对test.tar进行解包
```

注意以上命令只是打包，并没有压缩。

两个核心压缩方式：

```
tar -zcvf test.tar.gz *.py   #将所有py文件压缩成test.tar.gz,压缩比很可观
tar -jcvf test.tar.bz2 *.py   #将所有py文件压缩成test.tar.gz,压缩比上面小

tar -zxvf test.tar.gz        #解包
tar -jxvf test.tar.bz2       #解包

tar -zxvf test.tar.gz -C laowang/ #解压到指定路径
```

其他：

```
zip zzz.zip *.py
unzip -d laowang/ zzz.zip 
```

## 1.12 which/who

```
which ls    #输出ls命令的路径
#输出如下：
/bin/ls

who      #显示当前的用户登陆信息
whoami   #显示当前的账户名（用户名）
```

## 1.13 编辑器vim

使用`vi`或者`vim`命令即可编辑

```
vi text.py   #创建1.py并打开vim编辑器，默认进入命令模式
```

命令与模式转换：

![](https://image--1.oss-cn-shenzhen.aliyuncs.com/image-20200619104223301.png)

由命令模式进入编辑模式时：a命令在光标后面插入编辑，A命令在行末编辑，O命令在光标上一行插入，o命令在光标下一行编辑……。

**退出保存**：末行模式下按`wq`键，一个`x`键也可以。也可以`shift+zz`

**命令模式下命令：**

- yy：复制光标所在的行
  - 4yy：复制光标所在的行开始往下的一共4行
- dd：剪切光标所在的行
  - 2dd：剪切光标所在的行开始往下的一共4行
  - D：剪切光标所在的位置一直到行末
  - d0：剪切光标所在的位置一直到行首
  - u：撤销
  - ctrl+r：反撤销
  - x：删除光标定位的那个字符
  - X：删除光标前的那个字符
- p：粘贴
- 控制光标
  - h左，j下，k上，l右
  - M定位屏幕中间，H定位屏幕最上方，L定位屏幕最下方
  - ctrl+f 向下翻一页，ctrl+b 向上翻一页；ctrl+d向下翻半页，ctrl+u 向上翻半页
  - 回到第20行：20+G键；回到整个代码最后一行：G键；回第一行：gg键
  - w向后跳一个单词长度，b向前跳一个单词长度
  - shift+a：快速到行尾，并进入插入模式
- v、V：选择多行
  - `>>`：向右缩进
  - `<<`：向左缩进
  - `.`：重复上次命令
- 替换
  - r：替换当前字符
  - R：替换当前行光标及其后的字符
  - 整体替换：末行模式下输入`%s/需替换的内容/替换后的内容/g`
  - 替换部分：末行模式下输入`1,10s/需替换的内容/替换后的内容/g`替换1-10行的。
- 搜索
  - /：进入搜索模式，输入搜索词，之后按enter
  - n：下一个
  - N：上一个
  - 取消高亮显示`:noh`
- 高频率命令（十分有用）
  - `[[ `：跳到光标所在的程序块（函数）的开头；
  - `]]`：跳到光标所在的下一个程序块（函数）的开头；
  - `gD`：跳转到光标所在的局部变量的定义处
  - `''`：跳转到光标上次停靠的地方, 是两个'，而不是一个"。与上面一个命令配合用
  - `>>`：增加光标所在行的缩进
  - `<<`：较少光标所在行的缩进
  - `ctrl+p`：编辑模式下的代码补全
  - `ctrl+z`：vim模式下回到终端（vim后台挂起），终端下输入`fg`回车，重新回到vim界面。
  - 分屏
    - `:sp`：上下分屏，ctrl+w 切换屏
    - `:vs`：左右分屏，ctrl+ww切换屏
    - `:new` 新建空白分屏，:w ./new.py
    - `vim -o file1 file2`：以水平分屏的形式打开多个文件，-O是垂直分屏

# 2、补充命令

## 2.1 时间日期相关

```
cal #显示当前日历
cal -y 2018  #显示2018年日历
```

```
date 			       #显示当前时间
data "+%Y-%m-%d"  	   #显示年，月，日

data > xx.txt        #把显示的日期写到xx.txt文件里
```

## 2.2 查看进程

`ps [选项]`



![](https://image--1.oss-cn-shenzhen.aliyuncs.com/image-20200618164306567.png)

```
ps -aux  #查看当前时刻的所有进程消息
ps -uf
top      #查看进程消息,持续动态存在
htop  
kill [PID]  #以指定PID方式终止对应的进程
kill [PID] -9  #强制杀死进程
```

## 2.3 关闭重启

```
reboot  #重启
shutdown -h now  #立刻关机
shutdown -r now  #立刻关机并通知其他用户

```

## 2.4 挂载

```
df		#显示文件系统的磁盘使用情况
du      #显示本目录所占磁盘空间
```

## 2.5 查看或配置网卡

```
ifconfig   #查看网卡ip信息
ping 网址/ip   #测试通信
```

## 2.6 用户权限管理

### 2.6.1 用户创建切换删除

```
sudo useradd  账户名 -m      #创建一个账户并创建一个home目录（-m的作用）
sudo passwd 账户名      	  #设置密码
su 账户名               	  #切换账户
su - 账户名                  #切换账户的同时切换到它的家目录
exit                        #登出账户
```

```
ssh python@ip    #远程登陆ip地址的python账户
```

```
userdel 账户名       #删除账户

userdel -r 账户名    #删除账户并删除该账户的主目录，谨慎操作
```

```
sudo -s     #切换到超级管理员
```

### 2.6.1 用户组

```
groupadd 组名   #创建用户组
groupdel 组名   #删除用户组

cat /etc/group  #查看有哪些组
groupmod +2次tab键   #查看有哪些组
```

1. **为普通账户添加sudo权限**

新创建的用户，默认不能sudo，需要以下设置

```
sudo usermod -a -G adm 用户名
sudo usermod -a -G sudo 用户名
```

2. **切换文件的用户组**

```
chgrp YY 1.py   #切换1.py到YY用户组
chown xxx 1.py  #切换1.py到xxx用户
```

### 2.6.3 权限

![](https://image--1.oss-cn-shenzhen.aliyuncs.com/image-20200618231136063.png)

```
chmod u=rwx 1.py   #修改文件owner的权限
chmod +x 1.py      #给ower加执行（x）的权限
```

- `g=r-x ` 用户组

- `o=---  ` 其他人

也可以以数字代表r,w,x。

r：4      w：2       x：1       --> 可以数字加和代表rwx三者的组合。

如u=4，代表的是r--；如u=7，代表的是rwx。依次类推。

`chmod 751 1.py `等同于`chmod u=rwx,g=r-x,o=--x 1.py`

- -R **参数以递归方式对子目录和文件进行修改**。

## 2.7 下载源

默认ubuntu系统使用apt命令下载文件，那么到底是在哪下呢？

  查看下载源： `cat /etc/apt/sources.list`  

最好使用国内镜像源。

可以自己用vi编辑器设置更改，完了之后记得更新：`sudo apt-get update`

备份ubuntu默认下载源地址：

```
sudo cp /etc/apt/sources.list /etc/apt/sources.list.backup
```

下载安装某工具：

```
sudo apt-get install 软件包名
```

卸载删除某工具：

```
sudo apt-get remove 软件包名
```

> apt与apt-get：简单来说就是：apt = apt-get、apt-cache 和 apt-config 中最常用命令选项的集合。
>
> 通过 apt 命令，用户可以在同一地方集中得到所有必要的工具，apt 的主要目的是提供一种以「让终端用户满意」的方式来处理 Linux 软件包的有效方式。
>
> apt 具有更精减但足够的命令选项，而且参数选项的组织方式更为有效。

------------------

**详解wget,apt-get,yum,rpm区别**

（1）wget是一个下载命令，名字是World Wide Web与get的结合

如果要下载一个软件，可以直接wget+下载地址，通过HTTP，HTTPS，FTP三个最常见的TCP/IP协议下载。

（2）rpm是redhat公司的一种软件包管理机制，直接通过rpm命令进行安装删除等操作，最大的优点是自己内部自动处理了各种软件包可能的依赖关系。

```
rpm -ivh xx.rpm   #安装xx.rpm 包
rpm -e package    #删除包
```

（3）yum全称为 Yellow dog Updater, Modified， 基于RPM包管理，能够从指定的服务器自动下载RPM包并且安装，可以自动处理依赖性关系，并且一次安装所有依赖的软件包。

是redhat、centos下的一个软件安装方式，基于Linux。

```
yum install package
yum remove package
yum update
```

（4）apt-get是ubuntu下的一个软件安装方式，它是基于debain。

# 3、Linux文本处理

## 3.1  Sed命令

sed是一种流编辑器，是一种面向行的文本处理工具，配合正则使用更强大。每次从文本读入一行，在“保持空间”和“模式空间”进行修改，然后再读入下一行。

主要用于字符替换、格式化。

命令格式：

```shell
sed [options] 'command/script' file(s)
```

------------

| options 选项 | 含义                                                         |
| ------------ | ------------------------------------------------------------ |
| -e           | 该选项会将其后跟的脚本命令添加到已有的命令中。默认-e         |
| -f           | 该选项会将其后文件中的脚本命令添加到已有的命令中。           |
| -n           | 默认情况下，sed 会在所有的脚本指定执行完毕后，会自动输出处理后的内容，而该选项会屏蔽启动输出，需使用 print 命令来完成输出。 |
| -i           | 此选项会直接修改源文件。                                     |

--------

| command/script 命令字符             | 含义                                                         |
| ----------------------------------- | ------------------------------------------------------------ |
| [address]s/pattern/replacement/rule | address 表示指定要操作的具体行，pattern 指的是需要替换的内容，replacement 指的是要替换的新内容。 |
| [address]d                          | 删除文本中的特定行                                           |
| [address]a(或 i)\新文本内容         | a 命令表示在指定行的后面附加一行；i 命令表示在指定行的前面插入一行 |
| [address]c\用于替换的新文本         | 指定行中的所有内容，替换成该选项后面的字符串                 |
| [address]y/inchars/outchars/        | 处理单个字符，对 inchars 和 outchars 值进行一对一的映射，要求二者长度一致 |
| [address]p                          | 搜索符号条件的行，并输出该行的内容                           |
| [address]w filename                 | 将文本files中指定行的内容写入文件filename中                  |
| [address]r filename                 | 将一个独立文件filename的数据插入到当前数据流files的指定位置  |
| [address]q                          | 使 sed 命令在第一次匹配任务结束后                            |

----

**address 介绍：**

格式：`[address]脚本命令`    或者   `address {多个脚本命令}`

| address   | 例子及含义                                                   |
| --------- | ------------------------------------------------------------ |
| 2         | sed '2s/dog/cat/' data.txt，替换第2行                        |
| 2,8       | sed '2,8s/dog/cat/' data.txt，替换第2-8行                    |
| ``2,$``   | `` sed '2,$s/dog/cat/'  data.txt`` 替换第2-最后行            |
| /pattern/ | 指定文本模式来过滤出命令要作用的行，sed '/line1/s/w1/w2/rule' filename，将line1所在行中的w1替换为w2 |

介绍两个通配符：`^`行开始符；`$`行结尾符。

以下做一些常见用法介绍。

### 3.1.1 替换s

格式：

```
sed -i s#old_text#new_text#rule file       #按照rule规则，替换old_text为new_text
```

`-i `直接修改原文件file      ` s`代表替换      ` #`是分隔符，也可用`/`符号         

`rule`表示替换的规则：

- 若不写，只匹配替换第一个old_text
- 若为`g`，表示全局替换
- 若为`i`，忽略大小写关键字替换
- 若为`m`，m是数字，如果old_text的出现次数小于m，那么替换不生效。
- 若为`p`，打印匹配到的字符所在的行，此标记通常与 -n 选项一起使用。

举个例子：`sed -i s#./prep#/home/prep#g ./*/*.log`     也可这样写：`sed -i 's#./prep#/home/prep#g' ./*/*.log`  

 还可以这样用：

```
sed -i '#line1#s#old_text#new_text#rule' filename      #按照rule规则,将line1所在行中的old_text替换为new_text
sed -i '2,6s#old_text#new_text#rule' filename          #按照rule规则,将2,6行中的old_text替换为new_text

#举一个使用p的例子
sed -n 's#old_text#new_text#p' filename    #将old_text替换为new_text，并输出替换过的该行
```

> 注意：替换类似文件路径的字符串会比较麻烦，需要将路径中的正斜线`/`进行转义：`\/`

**批量替换：**

```
sed '{
s/sales/sale/
s/man/woman/
}' data.txt
```

### 3.1.2 删除d

```
sed -i '2,3d' data.txt    #删除2-3行
```

### 3.1.3 插入a/i

```
sed -i '3a\add new line' data.txt            #第3行后面加一行文字：add new line

sed -i '3i\add new line' data.txt            #第3行前面加一行文字：add new line

# 加多行
sed -i '3a\
add one line.\
add two line.' data.txt           
```

### 3.1.4 替换整行c

```
sed -i '3c\
add new line' data.txt            #第三行替换成add new line

sed -i '/num3/c\
add new line' data.txt            #将num3所在的行全部替换为add new line
```

### 3.1.5 打印行p

```
sed -n '/num3/p' data.txt         #打印num3所在的行
```

### 3.1.6 写入w

```
sed -i '1,2w test.txt' data.txt    #将data.txt的第1-2行写入到test.txt中
```

### 3.1.7 文件插入r

```
sed -i '3r test.txt' data.txt    #将text.txt全部插入到data.txt的第3行的后一行
```

### 3.1.8 退出q

```
sed -i '3q' data.txt        #输出前3行并退出
```

### 3.1.9 option -f

```
sed -f sed.sh data.txt     #-f后接一个shell脚本文件，这个脚本定义了command/script
```

### 3.1.10 option -e

```
# 利用-e可以实现多点编辑

sed -e '3,$d' -e 's/bash/blueshell/' data.txt  #删除3-最后一行，并且替换bash为blueshell
```



## 3.2 awk命令

awk 是一种编程语言，用于在linux/unix下对文本和数据进行处理。数据可以来自标准输(stdin)、一个或多个文件，或其它命令的输出。它在命令行中使用，但更多是作为脚本来使用。awk有很多内建的功能，比如数组、函数等，这是它和C语言的相同之处，灵活性是awk最大的优势。

语法格式：

```
awk [options] 'scripts' var=value filename
```

**options：**

- -F  fs：指定分隔符（可以是字符串或正则表达式），默认分隔符为空格或制表符
- -f  file：从脚本文件中读取awk命令
- -v var=value： 赋值变量，将外部变量传递给awk

**'scripts'：**

```shell
#'scripts'基本结构

'BEGIN{ print "start" } pattern{ commands } END{ print "end" }'
```

一个awk脚本通常由**BEGIN语句** + **模式匹配** + **END语句**三部分组成，这三部分都是**可选项**。

执行步骤：

- 第一步执行BEGIN 语句
- 第二步从文件或标准输入读取一行，然后再执行pattern语句，逐行扫描文件到文件全部被读取
- 第三步执行END语句

**举几个简单例子：**

- 1   从标准输入（管道）读取

![](https://image--1.oss-cn-shenzhen.aliyuncs.com/image-20220306122301005.png)

输出为：

```
welcome
hello
2022-03-06
```

> 不加print参数时默认只打印当前的行

- 2   只用command语句

```shell
>>> echo|awk '{ a="mgg"; b="mingg"; c="mingongge"; print a" is "b" or "c; }'

mgg is mingg or mingongge
```

> awk的print语句中**双引号**其实就是个**拼接**作用

- 3   pattern{ commands }，从文件读取匹配

```
#data.txt
line1  xxxx1
line2  yyyy2
line3  zzzz3
--------------------------
>>> awk '/line2/{print $2}' data.txt
yyyy2
```

下面开始正式介绍awk相关例子。

### 3.2.1 分隔符-F

awk默认以空格为分隔符，还可以接受指定的分隔符。

- `-F ' '` ：以空格为分隔符，单双引号都可

- `-F#` ：以`#`为分隔符，也可`-F '#'`，格式没有那么死板。
- `-F,`：以`,`为分隔符
- `-F '[ ,]'`：以左中括号`[`   空格    逗号`，` 右中括号`]`这**多个单字符作为分隔符**
- `-F'abc|cde'`：用abc和cde**多个字符串来作为分隔符**
- `-v FS='#'`：设置内部变量，指定输入分隔符`#`，其中FS是输入分隔符的意思
- `-v OFS='#'`：设置内部变量，指定输出分隔符`#`，其中OFS是输出分隔符的意思

```
举个简单例子
awk -F '#' '{print $1}' data.txt
```

$1代表文本行中的第1个数据字段。后面会介绍3.3.3---Awk的变量。

### 3.2.2 文件命令读取-f

awk 允许将脚本命令存储到文件中，然后再在命令行中引用。

```
awk -f {awk脚本} {文件名}

awk -f cal.awk log.txt
```

一个awk脚本文件的例子：

```
$ cat cal.awk
#!/bin/awk -f
#运行前
BEGIN {
    math = 0
    english = 0
    computer = 0
 
    printf "NAME    NO.   MATH  ENGLISH  COMPUTER   TOTAL\n"
    printf "---------------------------------------------\n"
}
#运行中
{
    math+=$3
    english+=$4
    computer+=$5
    printf "%-6s %-6s %4d %8d %8d %8d\n", $1, $2, $3,$4,$5, $3+$4+$5
}
#运行后
END {
    printf "---------------------------------------------\n"
    printf "  TOTAL:%10d %8d %8d \n", math, english, computer
    printf "AVERAGE:%10.2f %8.2f %8.2f\n", math/NR, english/NR, computer/NR
}
```

> awk即支持print，也可printf。差别可自行百度。

### 3.3.3 变量赋值-v

- 接受内部变量

```
# log.txt
2 this test
3 Are awk
This's a
10 There apple
-------------------------------------------
>>> awk -v a=1 '{print $1,$1+a}' log.txt

2 3
3 4
This's 1
10 11
#上述中log.txt文本中的a都被赋值了
```

- 接受外部变量

```
>>> val="aaa"
>>> echo|awk -v v1=$val '{print v1}' 
aaa
```



**Awk的变量**

```shell
# 内置变量
$0   #当前记录
$1~$n #当前记录的第N个字段

FS   #输入字段分隔符（-F相同作用）默认空格
RS   #输入记录分割符，默认换行符
OFS  #输出字段分隔符，默认空格
ORS  #输出记录分割符，默认换行符 

NF   #该行字段个数，就是列数
NR   #记录数，就是行号，默认从1开始
```

> 这些都是比较常用的内置变量，其余的自行百度。

### 3.3.4 匹配规则pattern

- 正则表达式作为pattern

  ```
  /A/            #去匹配含有 A 的字符
  
  awk '/hello/ { print $0 }' myfile  #输出含有hello的整行
  
  ```

- 比较表达式作为pattern

  ```
  '$NF == "A"          #输出最后一个字段为 A 的
  
  awk '$NF == "A" { print $0 }' myfile        #输出最后一个字段为A的整行
  ```

- 常量表达式作为pattern，或多模式多动作

  ```
  awk ' 真 { 执行代码 } 假 { 不执行代码 }' 
  
  awk '1 { print $0 }' myfile                           #为真，执行输出整行
  awk '$NR==1 {print $NF} $NR==3 {print $NF}'  myfile     
  ```

- 空pattern是永远匹配为真的

  ```
  awk '{ print $0 }' myfile                   #输出所有的整行，
  ```

- 模式范围: begpat, endpat

  这个模式范围, 是由两个 pattern 组成, 每个 pattern可以是任意的非特殊类型(可以是正则，比较，常量等pattern).

  ```
  awk 'NR==4, /555-3430/ { print $0}' myfile
  ```

  执行方式：匹配输出满足以begpat开始到以endpat结束中间的所有行，第一轮结束后可以开始下一轮（只满足这个起始和终止条件）。

- 特殊匹配BEGIN、END

  awk里的BEGIN语句和END语句也是一种匹配。



**Awk正则**

```shell
^         行首定位符
$         行尾定位符
.         匹配任意单个字符
*         匹配0个或多个前导字符（包括回车）
+         匹配1个或多个前导字符
?         匹配0个或1个前导字符 
[]        匹配指定字符组内的任意一个字符/^[ab]
[^]       匹配不在指定字符组内的任意一个字符
()        子表达式
|         或者
\         转义符
~,!~      匹配或不匹配的条件语句
x{m}      x字符重复m次
x{m,}     x字符至少重复m次
X{m,n}    x字符至少重复m次但不起过n次（需指定参数-posix或--re-interval）
```



### 3.3.5 执行命令{commands}

awk 是一种编程语言，故支持运算，文件操作，输入输出，循环语句，数组，内置函数等操作。

一般高级点的用法我们用不到，这里不做介绍。如有需要，跳转：

[shell编程之awk命令详解 - QuincyHu - 博客园 (cnblogs.com)](https://www.cnblogs.com/quincyhu/p/5884390.html)

[AWK 概述_w3cschool](https://www.w3cschool.cn/awk/hs671k8f.html)



# 4. Linux下配置环境变量

环境变量：使得命令能够在全局使用。而不必切换到其安装目录。

## 4.1 环境变量的配置方法

linux在正常启动时，先会启动一系列配置文件，再显示命令行提示符：



![](https://image--1.oss-cn-shenzhen.aliyuncs.com/image-20210106233834424.png)

在用户目录下的 `~/.bashrc`中配置环境变量：

```
# vim ~/.bashrc
// 添加以下一行
export PATH=$PATH:/home/uusama/mysql/bin
或者
export PATH=/home/uusama/mysql/bin：$PATH   （二者表达意思一样）
```

以上的`export PATH=$PATH:/home/uusama/mysql/bin`可以这样理解：

`export` 是配置环境变量的命令

`PATH` 是指定命令的搜索路径。类似的Linux中还有9个环境变量，见4.2.3。

`$PATH` 是指PATH这个环境变量，$符号就是表示PATH是个变量，$PATH代表了PATH路径下所有的变量，可能在其他配置文件配置了PATH=/etc/pangen/bin等多个路径，$PATH可将这些一并加载过来。

**冒号`:` 是分割符的意思**，意思是PATH环境变量不仅包含了$PATH代表的其他路径下的命令，还加上/home/uusama/mysql/bin，与win平台的分号作用类似，**表示并列**。

如果只是`export PATH=/home/uusama/mysql/bin`那么`PATH`只会指向`/home/uusama/mysql/bin`这个路径，覆盖了其他的指向。

## 4.2 扩展

### 4.2.1 export命令

配置环境变量当然不只是上面一种方法，还可以直接采用命令的形式：

在命令窗口输入：`export PATH=/home/uusama/mysql/bin:$PATH`

注意事项：

- 生效时间：立即生效
- 生效期限：当前终端有效，窗口关闭后无效
- 生效范围：仅对当前用户有效

而相对于`vim`命令直接编辑`~/.bashrc`，就会永久生效：

- 生效时间：使用相同的用户打开新的终端时生效，或者手动`source ~/.bashrc`生效
- 生效期限：永久有效
- 生效范围：仅对当前用户有效

### 4.2.2 查看变量

- echo $PATH ：显示所有PATH环境变量的路径

  ```
  /home/pangen/bin:/home/pangen/pangen_build_env/bin:/usr/lib64/qt-3.3/bin:/usr/local/bin:/usr/bin:/usr/local/sbin:/usr/sbin:/opt/ibutils/bin:/home/qiangjiang/.local/bin:/home/qiangjiang/bin
  ```

- export 可以查看（显示）Shell 环境变量

  ```
  [roc@roclinux ~]$ export
  declare -x CVS_RSH="ssh"
  declare -x G_BROKEN_FILENAMES="1"
  declare -x HISTCONTROL="ignoredups"
  declare -x HISTSIZE="1000"
  declare -x HOME="/home/roc"
  declare -x HOSTNAME="roclinux"
  …………………………
  ```

- env命令显示所有的环境变量
- set命令显示所有本地定义的Shell变量

### 4.2.3 10个环境变量

常用的环境变量：

PATH 决定了shell将到哪些目录中寻找命令或程序

HOME 当前用户主目录

HISTSIZE　历史记录数

LOGNAME 当前用户的登录名

HOSTNAME　指主机的名称

SHELL 当前用户Shell类型

LANGUGE 　语言相关的环境变量，多语言可以修改此环境变量

MAIL　当前用户的邮件存放目录

PS1　基本提示符，对于root用户是#，对于普通用户是$

### 4.2.4 给命令取别名

在配置文件中，如`~/.bashrc`或` ~/.profile`中使用alias命令可以定义一些命令的别名。

如：

```
vim ~/.bashrc
//编辑添加
alias rm='rm -irf'
```

这样每次使用`rm`命令时，就代表使用了`rm -irf`



# 5、Linux下运行python

ubuntu下默认使用python2.x版本，这个版本注意不能卸载掉。

输入python命令，即可进入python编辑模式，同时也可看到其版本信息。

- 安装python3.x版本：

```
sudo apt install python3.7
```

- 改变python指向版本

  默认情况下，输入python还是会进入python2.x，输入python3才会进入python3.x。

  可执行以下命令改变，使得python命令指向python3.x。

  ```
  ls -l /usr/bin | grep python   #查看目前的python版本及其指向（链接）
  
  rm /usr/bin/python    #删除原有python链接
  
  sudo ln -s /usr/bin/python3.7 /usr/bin/python  #新建python链接
  ```

  至此已经完成了目标。

- 安装pip

  ```
  sudo apt install python3-pip
  ```

  完成后，输入pip即可知道是否安装成功。

  

