---
title: JavaScript入门
categories: 前端
HTML格式实现标签样本: <center><font size=5 face="黑体">文本区</font></center>
mathjax: true
date: 2020-11-09 20:46:08
top:
tags:
typora-root-url: ..
---



仍杂持续学习中，内容可能有点杂乱，后期会整理……

# 1、基本语法

## 1.1 js的引入

- 页面内嵌：`<script> code </script>`也就是直接在页面写
- 外部引入：`<script src='需引入的js代码文件位置及名称'> code </script>`，也就是引入外部的js文件

```html
……
<body>
    <script type='text/javascript'>   //告诉浏览器script标签里的文本属于javascript语言
        var a=100,
            _b=10,
            $c=1; // 变量的定义(变量的只有这三种命名规则)及赋值
        document.write(a, _b, $c) // 打印（输出）到页面
    </script>
</body>
……
```

以上代码中变量的命名需遵循一定的规则，以字母，下划线或$符开头。

## 1.2 数据类型

### 1.2.1 堆和栈

介绍数据类型前，需要介绍两个名词：**堆heap和栈stack**。

任何程序在运行时都要在内存中开辟空间，堆和栈就是跟内存相关的名词。

**堆**是动态分配内存，内存大小不一，也不会自动释放。可以简单理解为一块区域。

**栈**是自动分配相对固定大小的内存空间，并由系统自动释放。可以理解为客栈（固定的房间）。

<img src="https://image--1.oss-cn-shenzhen.aliyuncs.com/image-20201216191817451.png" style="zoom:67%;" />

### 1.2.2 原始值与引用值

- 原始值（放在栈stack里面）：
  
- `number, Boolean, String, undefined, null`一共5种。
  
- 引用值（放在堆heap里面）：
  - Array数组
    
    ```javascript
    var a=[1,2,3,4]
    ```
    
  - Object 对象
  
    ```javascript
    var obj={
        name:'zs',
        age:18,
        sex:'male'
    }
    ```
  
  - function函数
  
    ```javascript
    function test() {            //函数名必须遵循小驼峰原则
        //函数内容;
        return;     //函数必须有返回，return可以省略（返回空）
    }
    ```
  
  - date 日期
  
    ```javascript
    var date = Date.now()
    ```
  
  - RegExp正则



### **两类错误**

- 低级错误：比如冒号写成了中文冒号，会引发该代码块不会执行，因为js编译时会先大致扫描整个代码块，检查语法错误。注意，这不会影响另一个js代码块。
- 逻辑错误：比如变量未经定义便使用，代码会运行，到错误处再报错。

## 1.3 运算符

js中的运算符与其他编程语言大致一样，这里只做简单介绍。

### 1.3.1 自增(或自减）

- a++：

  - ```javascript
    var a=10;
    document.write(a++);   //先执行本条语句打印a，再++
    //输出打印为10
    ```

- ++a：

  - ```javascript
    var a=10;
    document.write(++a);      //先++计算a再执行本条语句
    //输出打印为11
    ```

### 1.3.2 逻辑运算符

- `A && B`：“并且”的意思。若A、B均为真，就取值B；若A、B中至少一个为假，则取值0

  - ```javascript
    2>1 && documnet.write('通过')  //打印输出‘通过’，这里运算符按顺序执行
    ```

- `A||B`：“或”的意思碰到真就返回。若A为真，返回A，若A为假，B为真，就返回B，若A、B均为假，也返回B。

- `！A`与`！！A`：非运算符，取反，结果为布尔值，双感叹号意思是取反再取反。

  ```javascript
  >>>!1
  false
  >>>!0
  true
  >>>![]
  false
  >>>!!1
  true
  ```

  

## 1.4 条件语句

- `if`语句

  ```javascript
  if (条件判断){
    当条件满足时，执行里面的语句
  }else if (条件判断){
    当条件满足时，执行里面的语句
  }else{当上述条件都不满足时，执行里面的语句}
  ```

- `for`循环

  ```javascript
  for (var i=0;i<10;i++){
      ……；
  }
  ```


- `while/do while`：用法与C++一样

- `switch case`：条件判断语句

  ```javascript
  switch(参数){
         case 条件1：      //判断参数是否等于满足条件1
            执行语句;
              break;
         case 条件2：      //判断参数是否等于满足条件2
              执行语句;
              break;
  }
  -------------------------------------------------------------------
  var date=window.prompt('input')
  switch(date){
      case 'monday':console.log('working');
          break;
      case 'tuesday':console.log('working');
          break;
      case 'wendesday':console.log('working'); 
          break;
  }
  ```

  注意：switch找到满足条件的语句后，后面的语句虽然不判断，但是也会执行出来，加个break，就可以终止switch case语句。

## 1.5 类型查询与转换

- 类型查询：`typeof(obj)`，js中有六种数据类型：`number/srting/boolean/undefined/object/function`

    注意：typeof(null)为object。

- 类型转换

  - 显式转换

    `Number(a) `将a转换成数字

    `parseInt(string,radix)`：parse是转化，Int是整型，radix是进制，取值2-36。

    parseFloat(string)

    a.toString(radix)：转换成字符串，可以指定进制。

    String(a)：字符串转换

  - 隐式转换：在需要时自动转换

    自动转换：

    - ```javascript
  //这里随便举个例子
      
      var a='1'*123   //数字乘字符串会自动转换成数值类型，加减乘除除了加，都有这个性质
      -------------------------------------------------
    var a='1'
      var b=+a  //+号可将字符类型转换成数字类型
      ```
    
  
- 等于判断
  
    `===`：三个等于号，绝对等于
  
    `！==`：绝对不等于

# 2、函数

## 2.1 函数的定义

函数作用：实现高内聚，低耦合，减少重复。

```javascript
function 函数名() {            //函数名必须遵循小驼峰原则
    //函数内容;
    return;     //函数必须有返回，return可以省略（返回空）
}
-------------------------------------------------------------
var test = function 函数名() {         //函数表达式,这种写法没必要
    函数内容;
}
-------------------------------------------------------------
var test = function(a,b) {            //匿名函数表达式,还可以指定形参
    函数内容;
}
test(1,2);   //函数的调用,实参可以多于形参，不会报错
//另外，这里藏着一个aguments(数组类型),装着传入的实参[1,2],可以调用
//如document.write(aguments)
```

另外，这里藏着一个`aguments`(数组类型)，装着传入的实参[1,2]，可以调用，如document.write(aguments)。

举例：

```javascript
function sum(a,b) {
            a=2;
            arguments[0]=3
            document.write(a)

        }
sum(100,200)
//输出打印为3
```

形参长度：`函数名.length`

## 2.2 作用域

- 与python类似，分全局变量和局部变量

- js运行三部曲：语法分析（通篇扫面，检查语法错误）——>预编译——>解释执行（边解释边执行，解释一行执行一行）、

### 2.2.1 语法分析

变量和函数声明的顺序问题：

**函数声明，不管写在那里，都会被整体提到逻辑的最前面。**

**变量的声明也会提升（赋值不会）**

  ```javascript
  test();
  function test(){
      console.log('a');
  }
  //以上也是可以执行，先调用函数，再定义它
  ----------------------------------------------------------
  console.log(a);
  var a=123;
  //只会输出undefined,变量a声明了，但不会被赋值输出
  ```

imply global 暗示全局变量：即任何变量，若变量未经声明就赋值，则此变量就为全局对象（就是window所有）

一切声明的全局变量，全是window的属性，可以通过`window.变量名`来访问

### 2.2.2 预编译

预编译发生在函数执行前的一刻。

当函数执行时，会创建一个称为**执行期上下文**的内部对象。

一个执行期上下文定义了一个函数执行时的环境，函数每次执行时对应的执行上下文都是独一无二的，所以多次调用一个函数会导致创建多个执行上下文，当函数执行完毕，它所产生的执行上下文被销毁。

查找变量：从作用域链的顶端依次向下查找。

**（1）函数预编译过程**

- 创建AO对象（Activation Object），识别函数的作用域，函数产生的执行空间
- 找形参和变量**声明**，将变量和形参作为AO属性名，值为undefined
- 将实参值和形参统一（把实参值传到形参里）
- 在函数体里面找函数声明，值赋予函数体。（先看自己的AO，再看全局的GO）

```javascript
//这里举个例子
function fn(a) {
            console.log(a);
            var a=123;
            function a() {}
            console.log(a);
            var b=function () {}
            console.log(b);
            function d() {}
            }
fn(1);
//输出结果为：
ƒunction a()
123
ƒunction b()
```

以上执行的优先顺序理解：

<img src="https://image--1.oss-cn-shenzhen.aliyuncs.com/image-20201111220731524.png" style="zoom:120%;" />

**（2）全局的预编译**

- 生成一个GO对象Global Object（window就是GO）
- 找形参和变量声明，将变量和形参作为GO属性名，值为undefined
- 在函数体里面找函数声明，值赋予函数体。

  - 先生成GO还是AO？想执行全局，先生成GO，在执行函数前一刻生成AO。若有几层嵌套关系，近的优先，从近到远的，有AO就看AO，AO没有才看GO。

- [[scope]]：每个javascript函数都是一个对象， 对象中有些属性我们可以访问，但有些不可以，这些属性仅供javascript弓擎存取，[[scope]]就是其中一 个。[[scopel]指的就是我们所说的作用域,其中存储了运行期上下文的集合。
  作用域链： [[scopel]中所存储的执行期上下文对象的集合，这个集合呈链式链接，我们把这种链式链接叫做作用域链。

举个例子：

  ![](https://image--1.oss-cn-shenzhen.aliyuncs.com/image-20201216201800380.png)

  a函数被定义是，发生如下过程：

  ![](https://image--1.oss-cn-shenzhen.aliyuncs.com/image-20201112215403316.png)

  定义的函数a被执行时，发生如下过程：

  ![](https://image--1.oss-cn-shenzhen.aliyuncs.com/image-20201112215211763.png)

  b函数被创建时，发生如下过程：（继承了a函数的作用域）

  ![](https://image--1.oss-cn-shenzhen.aliyuncs.com/image-20201112220454635.png)

  b函数被执行时，发生如下过程：（创建的自己的AO）

  ![](https://image--1.oss-cn-shenzhen.aliyuncs.com/image-20201112220802498.png)

## 2.3 立即执行函数

执行完不再另外调用时，立即被销毁，目的时为了避免占用空间。

```javascript
var num = (function (a,b,c){
    return a+b+c;
}(1,3,4));

//还有第二种写法：不过w3c建议采用第一种
(function (){})();   //这两种函数的 名都没有写的必要
```

只有表达式才能被执行符号执行，执行符号就是括号  。

```javascript
function test(){console.log('a');}();  //这样会报错，函数的声明不能执行
--------------------------------------------------
function test(){console.log('a');};
test()；                 //test()就是执行表达式test的意思，故不会报错
---------------------------------------------------
//以下这两种写法也不会报错
var num=function (){console.log('a');}();
+ function test(){console.log('a');}();    //加号将其变成了表达式
```

# 3、对象

## 3.1 创建对象

创建对象有三种方法：

- 字面量
- 构造函数
  - 系统自带，new Object();Array();Number();Boolean();String();Date()
  - 自定义
- Object.create(原型)方法

```javascript
var MrJiang={              //字面量创建对象
            name:'MrJiang',
            age:18,
            sex:'male',
            health:100,
            smoke:function () {
                console.log("I'm smoking!");
                this.health--;       //这里的this代表的是MrJiang对象
            },
        };
----------------------------------------
//也可这样创建对象,与上无区别
var obj = new Object();    //有new就是构造对象
obj.name='MrJinag';
obj.sex='male';
```

## 3.2 增删改查

获取属性：

在浏览器里的控制台console面板运行：

![](https://image--1.oss-cn-shenzhen.aliyuncs.com/image-20201114171046778.png)



增加属性：（当然在代码也也可以增加属性）

![](https://image--1.oss-cn-shenzhen.aliyuncs.com/image-20201114171211389.png)



删除：`delete MrJiang.name`

## 3.3 函数生成对象（构造函数）

```javascript
function Car() {
            this.name='BWM';
            this.height='1400';
            this.health=100;
            this.run=function() {
                this.health--;
            };
        }
var car = new Car();      //通过函数生成一个对象
var car2 = new Car();
car2.name='Tesla';
-------------------------------------------------------
function Student(name,age,sex) {
            //这里隐藏个这个 var this={};
            this.name = name;
            this.age = age;
            this.sex = sex;
            this.grade=2017;
            //隐式的 return this;
        };
var zs = new Student('zs',18,'male');      
-------------------------------------------------------
function Student(name,age,sex) {
            //这里隐藏个这个 var this={};
            this.name = name;
            this.age = age;
            this.sex = sex;
            this.grade=2017;
            return 123;        //这个就是来搞破坏了,这个‘return 原始值’是无效的，‘retuen 对象’是可以的（数组，函数都行）
        };
var zs = new Student('zs',18,'male');   //一定要这个new
```

有了new，函数就产生了构造函数的功能。原始值是没有属性和方法的，只有个length。只有引用值有。

构造函数与普通函数的区别：①定义上没有很大区别，构造函数一般首字母大写，创造实例时用new。②this指向不同，构造函数的this 指向实例对象

**因为在ES6之前JS并没有引入类的概念 所以用构造函数代表类**。

**原始值的属性：**

```javascript
var num = 4;
num.len=3;
console.log(num.len)   //输出undefined，但不报错
```

以上执行结果的原因是，系统碰到num.len=3，知道这是不被允许的，于是就自动补充：`new Number(4).len = 3;  delete; `给你创建，但创建完就立马删除了，故不对num.len=3报错；
后面碰到console.log输出num.len，系统又会自动补充`new Number(4).len`，这个就没被定义，所以输出undefined。

## 3.4 包装类

```javascript
//将原始值变成对象，这就是包装类
var num =new Number(123);
var str=new String('123');
var bol=new Boolean('true');

```

原始值类型的，通过包装类可以变得有属性。

一个特例：

```javascript
var str='abcd';
str.length=2;    
console.log(str.length)  //结果为4
```

以上运行解释：碰到`str.length=2`，自动隐式的创建`new String('abcd').length=2;`，然后立马`delete;`，然后`str.length`取的是对象类型的字符串的属性。

记住这个：字符串只有一个不可更改的属性length（字符串还有其他方法的）。赋其他任何属性都不行。

## 3.5 继承模式

- 原型链继承
- 借用构造函数继承
  - 不能继承借用构造函数的原型
  - 每次构造函数都要多走一个函数
- 共享原型
  - 不能随便改动子类的原型
- 圣杯模式

### 3.5.1 原型

1、定义：原型是function对象的一 个属性（出生即隐式存在），它定义了构造函数制造出的对象的公共祖先。通过该构造函数产生的对象，可以继承该原型的属性和方法。原型也是对象。

2、利用原型特点和概念，可以提取共有属性。

3、对象如何查看原型一> 隐式属性`_ proto_`

4、对象如何查看对象的构造函数一> `constructor`

```javascript
Person.prototype.LastName='Deng';  //这里的prototype就是原型
Person.prototype.say=function () {
       console.log('hehe');
        };
function Person(){
           
       }
var person=new Person();   //虽然函数Person什么都没有，但继承了Person.prototype的属性
console.log(person.LastName)
//输出为‘Deng’
---------------------------------------------------
function Person(){
          this.LastName='Ji';     //在函数中添加这一句，其他不变
       }
console.log(person.LastName)
//输出为‘Ji’,自己有就不继承了
```

更改原型的指向：在使用new将函数Person()变对象时，默认在函数加了一行代码：`var this={__proto__:Person.prototype}`声明的原型的指向。

![](https://image--1.oss-cn-shenzhen.aliyuncs.com/image-20201115131012309.png)

既然这样，那么原型的指向便可以更改：

```javascript
Person.prototype.LastName = 'Deng';
function Person(){
	}
var ch = {
    LastName:'sunny',
        };
var pon = new Person();
pon.__proto__ = ch;            //更改pon的原型
console.log(pon.LastName)
//输出：sunny
```

### 3.5.2 原型链

```javascript
//以下就是原型链：
Grand.prototype.LastName='Deng';
function Grand(){};
var grand=new Grand();

Father.prototype=grand;
function Father(){
    this.name='Xuming';
}
var father=new Father();

Son.prototype=father;
function Son(){
    this.hobbit='smoke';
}
var son=new Son();
--------------------------------
//控制台里
>>>son.lastName
'Deng'
>>>son.name
'Xuming'
>>>son.hobbit
'smoke'
//son继承了祖先所有的属性
```

原型链会过多的继承不该继承的属性。

### 3.5.3 原型创建对象

```javascript
Person.prototype.LastName = 'Deng';
function Person(){}

var pon = Object.create(Person.prototype);
```

### 3.5.4 call/apply

（1）call

作用：改变this的指向。

```javascript
function Person(name,age){
    this.name=name;
    this.age=age;
}
var pers=new Person('deng',100);
var obj={};
Person.call();   //等同于Person() 执行的意思
----------------------------------------
//它更大的用处是：
Person.call(obj,'cheng',200);   
//它改变了Person函数中的this指向，相当于在函数中添加了this==obj
```

此时obj就变成了：

![](https://image--1.oss-cn-shenzhen.aliyuncs.com/image-20201115155551827.png)

通过call方法，就可以让obj借用函数的属性。

原型链会将长辈的所有特点都会继承过来，有时我们不需要其中的东西，利用上面学的call方法可以实现这一点。

下例就是一个实际应用：

```javascript
function Person(name,age,sex,health){
    this.name=name;
    this.age=age;
    this.sex=sex;
    this.health=health
}
function Student(name,age,sex,tel,grade){
    Person.call(this,name,age,sex);   //调用Person函数里的属性
    this.tel=tel;
    this.grade=grade;
}
var student=new Student('sunny','123','male',139,2017);
```

（2）apply

与call的区别是：call需要把实参按照形参的个数传进去

apply 需要传一个arguments。

```javascript
……
Person.apply(this,[name,age,sex]); //括起来
……
```

### 3.5.5 共享原型与圣杯模式

```javascript
Father.prototype.LastName='Deng';
function Father(){}
var father=new Father();

function Son(){}
Son.prototype=Father.prototype;   //只继承Father的原型
var son=new Son();
```

进一步地：

```javascript
Father.prototype.LastName='Deng';
function Father(){}
function Son(){}
function inherit(Target,Origin){
    Target.prototype=Origin.prototype;
}
inherit(Son,Father);
var son=new Son(); 
----------------------------------------------------
//以上有个缺点，如添加
Son.prototype.sex='male';
//则这也改变了Father的sex,显然不合理
//Son.prototype和Father.prototype指向了同一个空间
```

共享原型：改变子对象的原型会改变父对象的原型

圣杯模式：还是共享原型，不过需要一点小技巧。

```javascript
Father.prototype.LastName='Deng';
function Father(){}
function Son(){}
function inherit(Target,Origin){
    function F(){};
    F.prototype=Origin.prototype;
    Target.prototype=new F();
    Target.prototype.constuctor=Target;
}
inherit(Son,Father);
son.prototype.sex='male';
var son=new Son(); 
var father=new Father();
//这样就不影响了
```

## 3.6 命名空间

一个网页文件往往是团队合作的结果，经常嵌入多个js文件，有时为了实现某个类似的功能，不同的程序员可能写了相似的功能，用了一样的变量命名或者一样的标签名，这时把多个人的工作整合到一起，就会出现冲突的问题。

这是就出现了命名空间，将变量全放在命名空间里。

```javascript
//定义一个命名空间（以创建对象的形式）
var org={
    department1:{
        jicheng:{
            name:'abc',
            age:123
        },
        xuming:{
            name:'sxc',
            age:23
        }
    },
    department2:{
        zs:{},
        ls:{}  
    }
}
//调用变量
var jicheng=org.department1.jicheng;
jicheng.name
```

以上是老的解决办法。

现在采用新的方法：使用闭包实现

```javascript
var name='dcf'
var init=(function(){         //立即执行函数
    var name='abc';
    function callName(){
        console.log(name);
    }
    return function(){
        callName();
    }
}())

var initDeng=(function(){
    var name='Deng';
    function callName(){
        console.log(name);
    }
    return function(){
        callName();
    }
}())
init()
initDeng()
//以上就不会污染name这个变量了
```

## 3.7 访问属性名

```javascript
var obj={
    name:'qwe'
}
//访问属性
obj.name
//实际上，obj.name等价于obj['name'],真正执行的是obj['name']
obj['name']  //也可以访问属性
```

再看个例子：通过拼接的属性访问

```javascript
var deng={
    wife1:{name:'xiaoliu'},
    wife2:{name:'xiaozhang'},
    wife3:{name:'xiaomeng'},
    callWife:function(num){
        return this['wife'+num];
    }
}
deng.callWife(2) //打印出Object {name:'xiaozhang'}
```

## 3.8 对象枚举

```javascript
var arr=[1,2,3,4,5,6]
for (var i=0;i<arr.length;i++){
    console.log(arr[i])
}
//for in 循环
var obj={
    name:'zs',
    age:12,
    sex:'male',
    height:180
}
for (var prop in obj){
    console.log(obj[prop]);   //这里只能这样访问，console.log(obj.prop)会出现undefined
}
//输出
zs 
12 
male 
180
```

以上还是那个问题，obj.prop的意思是obj['prop']。

```javascript
var obj={
    name:'zs',
    age:12,
    sex:'male',
    height:180,
    __proto__:{
        lastName:'deng'
    }
}
for (var prop in obj){
    console.log(obj[prop]);
}
//输出
zs 
12 
male 
180
deng
//如果不想要deng,可以这样排除原型：
for (var prop in obj){
    if (obj.hasOwnProperty(prop)){
        console.log(obj[prop]);
    } 
}
```

`instanceof`用法及含义

A instanceof B ：A对象是不是B构造函数构造出来的（看A对象原型链上有没有B的原型）

```
[] instanceof Array
//输出True
```

`toString`

```javascript
//让数组调用对象的toString方法
Object.prototype.toString.call([])
//靠call改变this的指向
```

## 3.9 克隆

```javascript
var obj={
    name:'abc',
    age:123,
    card:['vasa','master'],
    wife:{
        name:'dvc',
        son:{
            name:'ass'
        }
    }
}
var obj1={
    name:obj.name,
    age:obj.age
}
```

正菜来了：

```javascript
function deepClone(origin,target){
    var target=target||{},
        toStr=object.prototype.toString,
        arrStr='[Object]';
    for (var prop in origin){
        if (origin.hasOwnProperty(prop)){
            if (origin[prop]!=='null'&& typeof(origin[prop])=='object'){
                if(toStr.call(origin[prop])==arrStr){
                    target[prop]=[];
                }else{
                    target[prop]={};
                }
                deepClone(origin[prop],target[prop]);
            }else{
                target[prop]=origin[prop];
            }
        }
    }
    return target
}
```

# 4、数组

## 4.1 数组

```javascript
var arr = [1,2,,4];
-------------------------------
var arr = new Array(1,2,,4);
```

数组的方法

- arr.push()：增加元素（单个，多个，数组）

  ```javascript
  //实现push的方法
  var arr=[];
  Array.prototype.push=function(){
      for(var i=0;i<arguments.length;i++){
          this[this.length]=arguments[i];
      }
      return this.length   
  }
  ```

- arr.pop()：弹出最后一个元素

- arr.shift()：删除元素（从前开始删）

- arr.unshift()：增加元素（从前开始插）

- arr.reverse()：逆转元素的顺序

- arr.sort()：元素排序（默认升序），arr.sort().reverse()降序

  ```javascript
  //这里排序有个问题
  var arr=[1,3,5,4,10]
  -----------------------
  //控制台里
  >>>arr.sort()
  Array(5) [ 1, 10, 3, 4, 5 ]  //这里排序是按ascii码排的
  -----------------------------------------
  //自定义排序
  //1.函数体里必须写两形参
  //2.看返回值  1)当返回值为负数时，那么前面的数放在前面
  //           2)为正数时，那么后面的数在前
  //           3)为0，不动
  arr.sort(function(a,b){
     return a-b;//升序
     //retrun b-a;降序
  });
  //控制台里
  >>>arr
  Array(5) [ 1, 3, 4, 5, 10 ]
  -----------------------------------------------
  //乱序
  arr.sort(function(a,b){
     return Math.random()-0.5;
  });
  ----------------------------------------------------
  var cheng={
      name:'cheng',
      age:18,
      sex:'male',
      face:'handsome',
  };
  var deng={
      name:'deng',
      age:40,
      sex:undefined,
      face:'amazing',
  };
  var zhang={
      name:'zhang',
      age:20,
      sex:'female',
  };
  var arr=[cheng,deng,zhang];
  arr.sort(function(a,b){
      return a.age-b.age;
  })
  //控制台里
  >>>arr
  0: Object { name: "cheng", age: 18, sex: "male", … }
  1: Object { name: "zhang", age: 20, sex: "female" }
  2: Object { name: "deng", age: 40, face: "amazing", … }
  length: 3
  <prototype>: Array []
  ```

- arr.splice(从第几位开始，截取长度，在切口处添加新元素)：切片

  ```javascript
  var arr=[1,2,3,5]
  -----------------------
  //控制台里
  >>>arr
  Array(4) [ 1, 2, 3, 5 ]
  >>>arr.splice(3,0,4)
  Array[]
  >>>arr
  Array(5) [ 1, 2, 3, 4, 5 ]
  ---------------------------------
  >>>arr
  Array(4) [ 1, 2, 3, 4, 5 ]
  >>>arr.splice(-1,2)
  Array[5]
  >>>arr
  Array(4)[1,2,3,4]
  ```

- arr.toString()：将数组变成字符串

- arr.slice(begin,end)：切片

- arr.join(str)：按照str的规则连接，返回字符串类型

  ```javascript
  var arr=[ 1, 2, 3, 4, 5 ]
  //控制台里
  >>>arr.join('-')
  1-2-3-4-5
  ```

- str.split()：按照括号里的规则拆分，返回为数组类型

  ```javascript
  var str='1-2-3-4-5'
  //控制台里
  >>>arr.split('-')
  ['1','2','3','4','5']  
  ```

## 4.2 类数组

类数组要求：属性要为索引（数字）属性，必须有length属性，最好加上push

  ```javascript
  var obj={
      '0':'a',
      '1':'b',
      '2':'c',
      'length':3,
      'push':Array.prototype.push
  }
  //控制台里
  >>>obj
  Object { 0: "a", 1: "b", 2: "c", length: 3, push: push() }
  >>>obj.push('d')
  4
  >>>obj
  Object { 0: "a", 1: "b", 2: "c", 3: "d", length: 4, push: push() }
  ```

  如果obj对象再加个splice属性，长得就完全像个数组了。

  ```javascript
  var obj={
      '0':'a',
      '1':'b',
      '2':'c',
      'length':3,
      'push':Array.prototype.push,
      'splice':Array.prototype.splice,
  }
  //控制台里
  >>>obj
  ['a','b','c']
  ```

#   5、补充知识点

## 5.1 错误类型

- EvalError：eval()的使用与定义不一致
- RangeError：数值越界
- ReferenceError：非法或不能识别的引用数值
- SyntaxError：发生语法解析错误
- TypeError：操作数类型错误
- URlError：URl处理函数使用不当

## 5.2 错误捕捉及处理

```javascript
try{
    console.log('a');
    console.log(b);
    console.log('c');
}catch(e){
    console.log(e.name+':'+e.massage);
}
//尝试执行try里面的代码，如果出现报错，则立即转到执行catch里的代码
```

在try里面发生错误，不会执行错误后的try里面的代码。例如上述代码中，console.log('c')便不会执行。

## 5.3 严格模式

JavaScript的语法标准随着时间的更替也在不断更新，从ES3.0到ES5.0，语法更新可能会导致某些方法不兼容。现在主流采用ES5.0标准(其实目前已经到10版本了)。

```javascript
//写在代码最前面，表示支持ES5.0的严格模式
'use strict';
```

严格模式既可以写在全局中，可以写在局部函数里（推荐局部）。

严格模式下，变量使用前必须先声明；局部this必须被赋值

# 6、DOM

## 6.1 DOM

DOM是Document Object Model的缩写，定义了表示和修改文档所需的方法。DOM对象即为宿主对象，有浏览器厂商定义，用来操作html和xml功能的一类对象的集合。也可以说DOM是对HTML及XML的标准编程接口。

DOM不能直接操作CSS。

xml和html：

DOM的使用示例：

```javascript
<script type='text/javascript'>
  //dom对象
  var div=documnet.getElementByTagName('div')[0]; //选取html中的div便签
  div.style.width='100px';
  div.style.height='100px';
  div.style.backgroundColor='red';
  
  var count=0;
  div.onclick=function(){
      count++;
      if (count%2==1){
          this.style.backgrandColor='green';//点击变绿
      }else{
          this.style.backgrandColor='red';//点击变红
      }
      
  }
</script>
```

## 6.2 事件



# 7、数据传输

## 7.1 json

一种数据传输格式，一种类似‘对象’的数据格式，便于前端和后端数据传输。

``` javascript
//json
'{'name':'Jiang','age':18}'
```

js中的对象变为json格式：

```javascript
var obj={
    name:'Jiang',
    age:18
}
var str=JSON.stringify(obj)
//控制台里
>>>str
"{'name':'Jiang','age':18}"
```

json格式变js中的对象：

```javascript
var obj=JSON.parse(str)
```

## 7.2 异步加载

js加载：js加载是一种同步加载，加载到javascript文件时，暂时阻断了html和css的的加载线，等js加载并执行完毕后，再继续执行html的css的加载。注意js加载本身是单线程的。是阻塞的。

但是有些js的存在就只是为了初始化一些数据，与页面没关系，不会修改操作页面。这些js文件包只是为了引入工具包（模块化的function）,不调用就不会执行，不会影响页面，于是就可以采用并行先加载过来，提高效率。

javascript异步加载的三种方案：

- defer异步加载，但是要等到dom文档全部解析完才会被执行。只有IE能用，也可以将代码写到内部。

  ```javascript
  <script type='text/javascript' src='tools.js' defer='defer'>
        
  </script>
  //执行到这时，不会阻塞后续的html和css的加载
  ```

- async异步加载，加载完就执行，async只能加载外部脚本，不能把js写在script标签里。

  - 执行时不会阻塞页面

  ```javascript
  <script type='text/javascript' src='tools.js' async='async'></script>
  ```

  

- 创建script，插入到DOM中，加载完毕后callback。这种就是按需加载。

  ```javascript
  <script type='text/javascript'>
      var script=document.createElement('script');
      script.type='text/javascript';
      script.src='tools.js';
      
      script.onload=function(){     //此处的作用是等tools.js加载完毕再执行test
          test();
      }
  //script.onload支持chrome,safari,firefox,opera,不支持IE
      
      document.head.appendChild(script);
  
  </script>
  -----------------------------------------
  //tools.js文件
  alter('我居然这么帅')
  function test(){
      console.log('a')
  }
  ```

  进行函数封装（最终的形式）：

  ```javascript
  <script type='text/javascript'>
      function loadScript(url,callback){
          var script=document.createElement('script');
      	script.type='text/javascript';
      	if (script.readState){//处理IE浏览器
              script.onreadystatechange=function(){
                  if (script.readyState=='comlete'||script.readyState=='loaded'){
                      tools[callback]();
                  }
              }
          }else{//处理目前几种主流浏览器
               script.onload=function(){     
          		tools[callback]();
      		}
          }
      	script.src=url;
          document.head,appendChild(script);
  	}
  	
  	load.Script('tools.js','test')
  
  </script>
  ---------------------------------------
  //tools.js文件
  var tools={
      test:function(){
      	console.log('a')
  	},
      demo:function(){}
  }
  
  ```

  

> asynchronous javascript and xml    ---->ajax
>



## 7.3 js加载时间线

浏览器的执行顺序：

1. 创建Document对象，开始解析web页面，解析HTML元素和他们的文本内容后添加Element对象和Text节点到文档中。这个阶段document.readyState='loading'。
2. 遇到Link外部CSS，创建线程加载，并继续解析文档。
3. 遇到script外部js，并且没有设置async，defer，浏览器加载，并阻塞，等待js加载完成并执行该脚本，然后继续解析文档。
4. 遇到script外部js，并且设置有async，defer，浏览器创建线程加载，并继续解析文档。对于async属性的脚本，脚本加载完成后立即执行。（异步禁止使用document.write()）
5. 遇到img等，先正常解析dom结构，然后浏览器异步加载src，并继续解析文档。
6. 当文档解析完成，document.readyState='interactive'。
7. 当文档解析完成后，所有设置有defer的脚本会按照顺序执行。（注意与async的不同，但同样禁止使用document.write()）
8. document对象触发DOMContenLoaded事件，这也标志这程序执行从同步脚本执行阶段，转化为事件驱动阶段。
9. 当所有async的脚本加载完毕并执行后，img等加载完毕后，document.readyState='complete'，window对象触发load事件。
10. 从此，以异步响应方式处理用户输入，网络事件等。

# 8、正则

## 8.1 实现方式

正则实现的几种形式：

- `/匹配规则/修饰符`
- `new RegExp(匹配规则，修饰符)`

举个例子：

```javascript
<script type='text/javascript'>
    var reg_1=/^a/g;                 //第一种形式
	var reg_2=new RegExp('^a','mg');  //第二种形式
    
	var str='abcd\na';
</script>
-----------------------------
//控制台里
>>>reg_1.test(str)
true

>>>str.match(reg_1) //匹配以a开头的字符
Array['a']
>>>str.match(reg_2)
Array['a','a']
```

## 8.2 修饰符与匹配规则

修饰符：

- i：执行对大小写不敏感的匹配
- g：执行全局匹配（查找所有符合条件的匹配）
- m：执行多行匹配，对于多行字符而言

匹配规则：

方括号：用于查找某个范围内的字符。

如`[abc]`，查找方括号之间的任意字符

如`[^abc]`，查找不在方括号里的字符

等等…………，还有元字符，与python里的正则差不多。

举个例子：

```javascript
<script type='text/javascript'>
   var reg=/[0-9A-z][cd][d]/g;   //第一位匹配0至9或A至z，第二位匹配c或d，第三位匹配d
   var str='adc1cd'
</script>
-----------------------------
//控制台里
>>>str.match(reg)
['1cd']

```







一些常见的函数方法

- `alert(a)`弹窗函数
- 字符串方法：
  - `str.charAt(1)`，取出字符串str的第2位字符
  - `str.charCodeAt(i)`，给出字符串str的第i+1位字符的unicode编码值。
- 交互输入：`num = window.prompt('input')`





