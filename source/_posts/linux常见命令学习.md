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

# 1. Linux常见命令

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

软链接：就是类似与快捷方式，指向源文件

硬链接：相当于给文件又给了个名字

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

`find 路径 形式 内容`

形式：

-name 按名字

-size 按大小

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

### 1.9.3 替换

```
sed -i s#需要替换的字段#新的字段#g 指定替换的文件
如：sed -i s#./prep#/home/prep#g ./*/*.log
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
vi 1.py   #创建1.py并进入编辑模式
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

# 2. 补充命令

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

`g=r-x ` 用户组

`o=---  ` 其他人

也可以以数字代表r,w,x。

r：4

w：2

x：1

以数字加和代表rwx三者的组合。

如u=4，代表的是w；如u=7，代表的是rwx。依次类推。

`chmod 137 1.py `等同于`chmod u=--x,g=-wx,o=rwx 1.py`

## 2.7 下载源

默认ubuntu系统使用apt命令下载文件，那么到底是在哪下呢？

  查看下载源： `cat /etc/apt/sources.list`  

最好使用国内镜像源。

可以自己用vi编辑器设置更改，完了之后记得更新：`sudo apt-get update`

备份ubuntu默认下载源地址：

```
sudo cp /etc/apt/sources.list /etc/apt/sources.list.backup
```

下载安装命令：

```
sudo apt-get install 软件包名
```

卸载删除命令：

```
sudo apt-get remove 软件包名
```

> apt与apt-get：简单来说就是：apt = apt-get、apt-cache 和 apt-config 中最常用命令选项的集合。
>
> 通过 apt 命令，用户可以在同一地方集中得到所有必要的工具，apt 的主要目的是提供一种以「让终端用户满意」的方式来处理 Linux 软件包的有效方式。
>
> apt 具有更精减但足够的命令选项，而且参数选项的组织方式更为有效。

#### 详解wget,apt-get,yum,rpm区别

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

（4）ap-get是ubuntu下的一个软件安装方式，它是基于debain。

# 3 Linux下运行python

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

# 5. Shell 语句

所谓的shell命令语句，就是linux下命令行里输入命令的集合。

比如：新建一个shell命令文件：touch run.sh (shell文件以.sh为后缀)

写入以下内容：

```
cd /home/test_work
touch test1.py
```

执行run.sh：`sh run.sh`

就是相当于命令行里执行了上两句命令。

但是，shell语句肯定没有这么简单，包含更多的功能。

## 5.1 字符串的定义，引号，等于号

```
#！ /bin/bash
filepath=/home/test_work
filepath=“/home/test_work”   #这两种表达的意思是一样的
```

在shell里面，字符串不需要引号标识，引号可要可不要。

但是，双引号（“”），单引号（‘’），尖角号（``）三种符号的内容不一样：

双引号：阻止shell对大多数特殊字符（例如#）进行解释，但是$，`，“仍然保持其特殊含义。

单引号：阻止shell对所有字符进行解释。

尖角号：这个符号通常位于键盘Esc键的下方。当用尖角号括起来一个shell命令时，代表执行这个命令，并将执行的结果返回给赋给的变量。

等于号（=）：也是赋值作用，注意它前后不能有空格（空格在shell里面是有隔断作用的，表示前命令的结束）。

## 5.2 变量及$符号

在BASH中，$符号用于对变量进行解析，相当于取值的作用。例如

```
#！ /bin/bash
filepath=/home/test_work
echo $filepath
```

通过sh运行这个脚本，就会打印输出/home/test_work。

echo 是打印输出的意思，$filepath 是对filepath这个变量取值。

### 位置变量

所谓的位置变量，就是在运行程序时，对所传入的参数进行取值。例子如下：

新建一个.sh文件，test.sh，写入：

```
filepath=/home/test_work
echo $0
echo $1
echo $2
echo $3
```

运行此程序：

<img src="https://image--1.oss-cn-shenzhen.aliyuncs.com/image-20210406110909691.png" style="zoom:80%;" align='left'/>

由此可见，

$0代表文件名

$1代表传入的第1个参数

$2代表传入的第2个参数

另外，shell还有另外三个位置变量：

- $* 所有传入的参数列表
- $@ 所有传入的参数列表，同上
- $# 传入参数的个数

示例：

```
filepath=/home/test_work
echo $*
echo $@
echo $#
```

运行结果：

<img src="https://image--1.oss-cn-shenzhen.aliyuncs.com/image-20210406112941373.png" style="zoom:100%;" align='left'/>



# 5.3 一些shell脚本中常用的命令

### 5.3.1 cat

```
# run.sh
cat > pframe.py << EOF
import numpy as np
a=np.array([1,2,3,4])
EOF
```

这里的意思是：

cat 新建一个文件pframe.py，并写入两个EOF标识符之间的字符。（EOF作为标识符不是固定的，也可以用其他字母代替）

