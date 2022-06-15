---
title: shell编程
categories: Linux
HTML格式实现标签样本: <center><font size=5 face="黑体">文本区</font></center>
mathjax: true
date: 2022-03-07 20:24:24
top:
tags:
Note: 不要直接使用MarkDown表格 请插图
---



# 一、shell编程概述

在 Linux 下有一门脚本语言叫做：**Shell 脚本**，这个脚本语言可以帮助我们简化很多工作，例如编写自定义命令等，所以还是很有必要学习它的基本用法的。

本质上讲，shell是linux命令集的概称，是属于命令行的人机界面。shell里语句可以直接是linux命令的执行。

一个简单的 `hello.sh` 脚本像下面这样：

```shell
#!/bin/bash                     # 标识该 Shell 脚本由哪个 Shell 解释

echo "Hello World!"             # 打印输出字符 Hello World!
```

**运行shell脚本**：在CMD命令行下，`sh hello.sh` 即可，也可 `./hello.sh` 运行

**后台运行**：`sh hello.sh &`  ，若退出终端的运行就会挂掉。

**后台挂起运行**：`nohup sh hello.sh &`，hohup的作用是：用于在系统后台不挂断地运行命令，退出终端不会影响程序的运行。

Shell 的编写流程：

1. 编写 Shell 脚本
2. 赋予可执行权限(一般可以忽略)
3. 执行，调试

# 二、开始shell

shell语言是linux命令集的概称，shell可以通过其条件语句和循环语句等，把一系列Linux命令结合在一起，形成一个相当于面向过程的程序。

同时，在语法上，shell语言格式与其他编程需要有相当大的不同。这个做一个简单的总结：

- 用 # 进行单行注释，也可多行注释，多行注释请百度，一般用的少
- 用空格分割命令，空格不能随便用
- 用换行分割语句，分号也可用来分割语句。语句可以是一个命令，也可以是一个关键词。
- 对缩进没有要求，但是尽可能好看点，便于阅读

## 2.1 shell关键字

### 2.1.1 基本关键字

基本常用的关键字如下：

1. echo：打印文字到屏幕
2. exec：执行另一个 Shell 脚本
3.  read：读标准输入
4.  expr：对整数型变量进行算术运算
5. test：用于测试变量是否相等、 是否为空、文件类型等
6. exit：退出

看个例子，新建文件test.sh，写入：

```shell
#!/bin/bash 

echo "Hello Shell"       # 打印输出字符串 Hello Shell

# 读入变量（从屏幕输入）
read VAR
echo "VAR is $VAR"     

# 计算变量
expr $VAR - 5       #算术运算，并返回值

# 测试字符串
test "Hello"="HelloWorld"

# 测试整数
test $VAR -eq 10

# 测试目录
test -d ./Android

# 执行其他 Shell 脚本
exec ./othershell.sh

# 退出
exit
```

> $符号是对变量取值



### 2.1.1 各种重要符号

- `$变量名`：对变量取值

- `${变量名}`：对变量取值，花括号的作用仅仅是精确界定变量名称的范围。

- `$(命令或函数名)`：执行命令或函数，并返回执行结果。与反引号  \`命令或函数名\`  作用一致

- 引号：见2.2.1章节

- `$[1+2]` 与`$((2+1)) `：进行整数运算，注意这里可以没有空格

- [ ]：判断符号，用于条件测试，见2.5章节

- (( ))：整型的计算

- [[ ]]：双方括号为字符串比较提供高级功能，模式匹配。常见用法见2.4.2

  [[是 bash 程序语言的关键字。

-------------

**()与{}的区别**

相同点：

- ()和{}都是把一串的命令放在括号里面，如果命令在一行，则命令之间用 ; 隔开

不同点：

- ()只是把一串命令重新开一个子shell进行执行，不影响当前shell环境；{}对一串命令在当前shell执行,影响当前shell环境

- ()最后一个命令不用分号，{}最后一个命令要用分号
- ()里的第一个命令和左边括号不必有空格，{}的第一个命令和左括号之间必要要有一个空格
- ()和{}中括号里面的某个命令的重定向只影响改名了，但括号外的重定向则影响到括号里的所有命令

---

**==与=的区别**

- == 可用于判断变量是否相等，= 除了可用于判断变量是否相等外，还可以表示赋值。
- = 与 == 在 [ ] 中表示判断(字符串比较)时是等价的
- 在 (( )) 中 = 表示赋值， == 表示判断(整数比较)，它们不等价

---

**$()的补充用法**

- ${var:-word}：若 var为空或未设置，返回默认值但是并不把默认值赋值给该变量，若 var 不为空，则不替换，var 的值不变
-  ${var:=word}：若var为空或未设置，把默认值赋值给该变量。若 var设置了，则不替换，var的值不变
- ${var:+word}：若 var 不为空时，返回默认值，并且也不重新赋值



## 2.2 字符串

shell就两种数据类型，字符型和数组。数字（整型）也可以看作是一种字符串。

**在Shell中所有的变量默认都是字符串型。字符串可以用单引号，也可以用双引号，也可以不用引号。**

> 字符串赋值时，变量名和等号之间**不能有空格**
>
> 取值时，只要在变量名前面加美元符号**$**即可，也可使用${变量名}

### 2.2.1 引号

- 单引号

  ```
  str='this is a string'
  ```

  单引号字符串的限制：

  - 单引号里的任何字符都会原样输出，单引号字符串中的变量是无效的；
  - 单引号字串中不能出现单独一个的单引号（对单引号使用转义符后也不行），但可成对出现，作为字符串拼接使用。

- 双引号

  ```
  name="Jack"
  str="my name is $name"
  ```

  双引号的优点：

  - 双引号里可以有变量
  - 双引号里可以出现转义字符

- 不加引号

  ```
  path=/home/qiang/
  
  file=$path/xor.txt
  ```

  不加引号特点：

  - 只能是连续字符串，不能有空格
  - 可以有变量
  - 可以出现转义字符

这里补充一个符号：**尖角号`**（或叫反引号）

这个符号通常位于键盘Esc键的下方。当用尖角号括起来一个shell命令时，代表执行这个命令，并将执行的结果返回给赋给的变量。

### 2.2.2 字符串方法

- 获取子串长度：`${#str}`，获取str变量的字符串长度

- 提取子串（切片）：`${str:1:4}`，从第2个字符开始，截取4个字符

  `${str:2}`，从第3个字符开始，截取后面所有字符

  `${str:2:-1}`，从左往右第3位开始截取，从右往左截取到第一位

- 查找子串：

  ```
  expr index "$str" xy    #查找字符x或y的位置(哪个字母先出现就计算哪个)
  ```

- 拼接字符串："hello, "$name" !"，直接写在一块就行。

- 匹配删除：

  `${str#*chr} `：表示删除从左到右第一个遇到的字符chr及其左侧的字符
  `${str##*chr}`：表示删除从左到右最后一个遇到的字符chr及其左侧的字符（贪婪模式）
  `${str%chr*` ：表示删除从右向左第一个遇到的字符chr及其右侧的字符
  `${str%%chr*}` ：表示删除从右到左最后一个遇到的字符chr及其右侧的字符（贪婪模式）



## 2.3 shell变量类型

Shell 变量分为 3 种：

1. **用户自定义变量**
   - 等号前后不要有空格：`NUM=10`
   - 一般变量名用大写：`M=1`
2. **预定义变量**
3. **环境变量**

### 2.3.1 自定义变量

这种变量**只支持字符串类型**，不支持其他字符，浮点等类型，常见有这 3 个前缀：

-  `unset`：删除变量
-  `readonly`：标记只读变量
-  `export`：指定全局变量

一个例子：

```shell
#!/bin/bash 

# 定义普通变量
CITY=SHENZHEN              #将字符串 SHENZHEN 赋给变量 CITY 

# 定义全局变量
export NAME=cdeveloper     

# 定义只读变量
readonly AGE=21

# 打印变量的值
echo $CITY
echo $NAME
echo $AGE

# 删除 CITY 变量
unset CITY
```

### 2.3.2 预定义变量

**预定义变量常用来获取命令行的输入**，有下面这些：

1. `$0` ：脚本文件名
2. `$1-9 `：第 1-9 个命令行参数名
3. `$# `：命令行参数个数
4. `$@` ：所有命令行参数
5. `$*` ：所有命令行参数
6. `$?` ：前一个命令的退出状态，**可用于获取函数返回值**
7. `$$` ：执行的进程 ID

在运行shell脚本的时候，可以外部传参，供脚本内部使用：

```
sh test.sh param1 parma2 parma3     #运行test.sh脚本并传入三个参数 param1 parma2 parma3
```

这时候这7个预定义变量的作用就容易出来了。

一个例子执行`sh.test.sh x y z`：

```
#!/bin/bash

echo $0         #输出 test.sh
echo $1         #输出 x
echo $2         #输出 y
echo $3         #输出 z
echo $#         #输出 3
echo $@         #输出 x y z
echo $*         #输出 x y z
echo $?         #输出 0
echo $$         #输出 184715
```

### 2.3.3 环境变量

环境变量默认就存在，常用的有下面这几个：

1. `HOME`：代表用户主目录
2. `PATH`：代表系统环境变量 PATH
3.  `TERM`：代表当前终端
4. `UID`：代表当前用户 ID
5.  `PWD`：代表当前工作目录，绝对路径

看一个例子：

```
#!/bin/bash

echo "print env"

echo $HOME    #输出 /home/origin
echo $PATH    #输出 /home/orange/anaconda2/bin:后面还有很多
echo $TERM    #输出 xterm
echo $PWD     #输出 /home/origin/test
echo $UID     #输出 1674
```



## 2.4 shell运算符

### 2.4.1 算术运算

**shell的算术运算只支持整型数字运算（包括正负整型），不支持浮点数。在算术运算时，非数字的单字符或字符串都被shell当作0。**

由于Shell中所有的变量默认都是字符串型，故在运算时，不同于常规运算方法。

常用的四种运算格式（都是计算m+1）:

- `m=$[ m + 1 ]`
- `m=expr $m + 1`
- `let m=m+1`
- `m=$(( m + 1))`



![](https://image--1.oss-cn-shenzhen.aliyuncs.com/image-20220307194350402.png)

另外：还支持自增++和自减--。

### 2.4.2 逻辑运算符

![](https://image--1.oss-cn-shenzhen.aliyuncs.com/image-20220307195033890.png)



### 2.4.3 布尔运算符

![](https://image--1.oss-cn-shenzhen.aliyuncs.com/image-20220307195130426.png)



## 2.5 条件测试

shell条件测试一共就三种：**文件测试**，**数值比较**，**字符串比较**

条件测试分为三种用法，但作用是一样的：

- test 条件表达式
- [ 条件表达式 ]               注意`[`后要加空格，`]`前也有空格
- [[ 条件表达式 ]]

推荐使用第二种。

值得注意的是：**对于条件测试，其中的条件表达式condition为真返回 0，假返回 1。这与其他语言不同！**

在shell中，命令语句执行完，**其退出状态码，0（成功）表示真，非0（失败）表示假**

**但是用 if [ ] 的判断条件 if [ 1 ]时，任何数字都返回真。**

### 2.5.1 文件测试

![](https://image--1.oss-cn-shenzhen.aliyuncs.com/image-20220307195210982.png)

用法示例：

```shell
[ ! -d /home/test ] && makdir /home/test     #目录不存在就创建

[ -d /home/test ] || makdir /home/test     #目录存在就不创建

test -d /home/test
echo $?
0                     #在shell中，为真的退出状态码为0，为假时状态码为1
```

### 2.5.2 数值比较

关系运算符只支持数字，不支持字符串，除非字符串的值是数字。

![](https://image--1.oss-cn-shenzhen.aliyuncs.com/image-20220307195248494.png)

用法示例：

```shell
disk_use=`df -Th`|grep '/$'|awk '{print $(NF-1)}'|awk -F "%" '{print $1}'  #磁盘利用率
user=Alice

if [ $disk_use -ge 90 ]; then
   echo "`data +%F-%H` disk: ${disk_use}%"|mail -s "disk war..." $user
fi
```

数值比较还支持C语言风格：如`((1<10))，((1<=10))`这种，这里不多做介绍。

### 2.5.3 字符串比较

**字符串变量要使用双引号引起来**

![](https://image--1.oss-cn-shenzhen.aliyuncs.com/image-20220307195333214.png)

用法示例：

```shell
[ aaa = abb ];echo $?            #输出 1   1为假
[ "aaa" = "aaa" ];echo $?        #输出 0   0为真

str=asd
str2=abc
[ "$str" = "$str2" ];echo $?     #输出 1    
```



## 2.6 shell语句

### 2.6.1 if语句

格式：

```
if 条件测试1
then
   执行语句1
elif 条件测试2
then
   执行语句2   
else
   执行语句2
fi  
```

用法示例：

```shell
#!/bin/bash 

VAR=10

if [ $VAR -eq 10 ]    #if test $VAR -eq 10
then
    echo "true"
else
    echo "false"
fi
```

### 2.6.2 case语句

格式：

```shell
case 变量 in
    值1)
      执行语句1
      ;;
    值2)
      执行语句2
      ;;
    *)
      执行语句3
      ;;
esac    
```

### 2.6.3 for语句

格式：

```
for ((i = 1; i <= 3; i++))
do
    echo $i
done
 
for j in 1 2 3
do
    echo $j
done

for j in {0..100}    #输出0-100
do
    echo $j
done
```

### 2.6.4 while语句

格式：

```
while 条件测试       #条件为真，执行
do
    执行语句  
done
```

### 2.6.5 until语句

格式：

```
until 条件测试       #条件为假，执行
do  
    echo $i
    i=$[ $i + 1 ]
done
```

### 2.6.6 break

Shell 中的 `break` 用法与高级语言相同，都是**跳出循环**，来看个例子：

```
for VAR in 1 2 3
do
    if [ $VAR -eq 2 ]
    then
        break
    fi

    echo $VAR
done
```

### 2.6.7 continue

`continue` 用来**跳过本次循环**，进入下一次循环

```
for VAR in 1 2 3
do
    if [ $VAR -eq 2 ]
    then
        continue    
    fi

    echo $VAR
done
```

## 2.7 shell数组

shell就两种数据类型，字符型和数组。字符串之前已经介绍了，这里介绍数组。

数组中可以存放多个值。Bash Shell 只支持一维数组（不支持多维数组），初始化时不需要定义数组大小。

### 2.7.1 数组定义

```
#第一种
arr=(v1 v2 v3 v4 v5)    #v可以是字符串或者数字，不必类型相同

#第二种
arr1=(
xxx
yyy
zzz
)

#单独赋值

arr[1]=10
```

> 一对括号表示的是数组，数组元素用空格符号分隔开

### 2.7.2 读取与赋值

- 读取数组：
  - 读取整个数组：`${arr[@]}`   或者   `${arr[*]} `
  - 按索引读取：`${arr[index]}`，索引从0开始
- 读取长度：`${#arr[@]}`   或者   `${#arr[*]} `
- 赋值：`arr[1]=100`
- 删除：`unset arr[1]`  或 `unset arr`
- 替换：`${a[@或*] /查找字符/替换字符}` ，该操作不会改变原来的数组内容

一个常用的例子：

```shell
caselist=($(ls /home/template_dir | grep -v 'xor'))

for case in ${caselist[*]}
do
  echo $case
done
```



## 2.8 shell函数

**shell函数中，默认均为全局变量**，如想用局部变量加`local`关键字

### 2.8.1 函数定义与调用

格式1：

```shell
function fun1(){         #如果写了function关键字，（）可以省略

   echo $1       #返回方式1
   echo $2
   return 0      #返回方式2

}

#1 直接函数名调用,并传参
fun1 10 20

#2 间接调用
F=`fun1 100 200`
echo $F
```

格式2：

```shell
fun2()
{
  echo "fun2 call"  
}

#3 直接函数名调用，不带参数方式
fun2
```

Shell函数返回值，常用的两种方式：return、echo。二者是有差别的

- return：只能用来返回整数值（0-255），一般是用来表示函数执行成功与否的，0表示成功，其他值表示失败。
- echo：标准输出返回，可以返回任何类型的数据。



### 2.8.2 获取函数返回值

- 函数用echo输出返回：直接函数名调用

- 函数用return输出返回：`$?` 接收，`$? `要紧跟在函数调用处的后面

先举个例子：

```shell
fun2()
{
  echo "fun2 call"
  return 100
}

fun2        # 输出echo的内容：  fun2 call

echo $?      #输出return的内容：100

echo $?      #输出函数退出码：0
```

还有2种：

- 函数名当命令执行：\`fun2\` (尖角号)    或者   $(fun2)
- 传参输出返回：$(fun2  "/temp")

再举个例子：

```shell
fun3()
{
  echo "fun3 call"
}

echo `fun3`      #输出打印 fun3 call

echo $(fun3)     #输出打印 fun3 call

re=$(fun3) 

echo ${re}       #输出打印 fun3 call
```



### 2.8.3 高级收参

$@ 可以接收所有参数，以数组的方式。

```shell
function getsum(){
    local sum=0
    for n in $@
    do
         ((sum+=n))
    done
    echo $sum
    return 0
}

total=$(getsum 10 20 55 15)
echo $total
```

## 2.9 shell重定向

### 2.9.1 输出重定向

命令的结果不再输出到显示器上，而是输出到指定的文件里。

- **命令输出重定向**

格式：

```
command > file       #覆盖写入到文件
command >> file      #追加写入到文件
```

用法示例：

```shell
echo "hello world!" > data.txt

ls -l >> data.txt
```

- **cat 输出重定向**：**用于多行文本写入**

用法示例：

```shell
cat > file << EOF
export ORACLE_SID=yqpt 
export PATH=\$PATH:\$ORACLE_HOME/bin  
EOF
```

作用：将两个EOF之间的内容输出写到data.txt文件里。

1. EOF只是一个标识符，也可使用其他字符代替
2. 多行文本如有$等特殊字符时，须利用转义字符 \



### 2.9.2 输入重定向

使用文件作为命令的输入。

```
command < file       #将 file 文件中的内容作为 command 的输入
command << END       #从标准输入（键盘）中读取数据，直到遇见分界符 END 才停止
```

高级用法（代码输入重定向）示例：

```shell
#!/bin/bash
sum=0
while read n; do
    ((sum += n))
done < nums.txt      #输入重定向
echo "sum=$sum"
```



## 2.10 shell调试

**检查是否有语法错误：**

```
sh -n script_name.sh
```

**执行并逐步调试shell脚本：**

```
sh -x script_name.sh
```

**带有 `+` 表示的是 `Shell` 调试器的输出**，**不带 `+` 表示我们程序的输出**。

