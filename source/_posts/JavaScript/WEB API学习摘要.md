---
title: Web Api学习笔记
date: 2019-03-23 00:00:21
tags: JavaScript
categories: JavaScript
---

Ctrl+Alt+L    webstorm 格式化代码



#### JS的组成

1.ECMAScript
2.DOM
3.BOM

#### JS是一门运行在客户端的脚本语言(弱类型动态语言)

alert()       弹出警示框
console.log() 控制台显示内容
document.write() 在页面显示内容
prompt()         用户输入框
comfirm()        选择按钮

直接量：直接可以拿来使用的数据值  数字，字符串
用来帮助我们想计算机存储数据使用
变量必须下划线、字母、$开头
可以包含有数字\字母\下划线\$
区分大小写
不能使用关键字和保留字

#### 转义符：

```js
\+ 一些字符
\n   \t  \b
```

前++：先在原来的数字上先+1，然后得到结果参与运算
后++：先原来的数字先参与运算，然后在原来的基础上+1

### 关系运算符

... === !== ... （boolean）
==：只比较值是否相等， 不管数据类型
===：只要相等，数据类型也要一样

### 简单数据类型

1.string 字符串  2.number 数字  3.boolean 布尔类型  4.undefined  5.null

### 复杂数据类型

1.数组 2.对象 3.函数  4.Date  5.Array  等等

### 数据类型检测  typeof

注意：只要是用户输入的内容都是string类型

隐式转换 可以使用+ - *  /   !!  +""
加 +a
减 a - 0
乘 a * 1
除 a / 1
注意：字符串只有在一种情况下会转换成false 空字符串“”

### 显示转换（强制类型转换）

1.Number();
2.parseInt();  //整数
3.parseFloat();  //小数
4.Boolean();    //布尔
5.toString() String()  //字符串


注意：NaN：not a number 不是一个数字，NaN也是number类型中的一个值。它是用来表示数字的特殊的一种状态

一般都是表示不能真正意义上转换成数字的类型的值。

`Number()`只能将数字型的string类型的值转换成number，对于非数字型的`string`值，不能真正意义上的类型。

使用`parseInt()`方法来将`string`类型的数据转换成`number`类型的时候，如果这个string类型的数据时一个数字型的`string`，这种情况下是可以完成转换。
如果是数字+字母型的`string` 类型，如果数字在前面，那么使用`parseInt()`来转换的时候，只会将前面的数字部分转换，后面的字母部分直接丢弃。

`null`和`undefined`这两个类型没有`tostring()`方法



```js

// js求数组最大值方法汇总
// 定义一个数组

var arr = [-1, 1, 101, -52, 10, 1001, 1001]
// 1.es6拓展运算符...

Math.max(...arr)
// 2.es5 apply(与方法1原理相同)

Math.max.apply(null,arr)
// 3.for循环

let max = arr[0];
for (let i = 0; i < arr.length - 1; i++) {
    max = max < arr[i+1] ? arr[i+1] : max
}
// 4.数组sort()

arr.sort((num1, num2) => {
    return num1 - num2 < 0
})
arr[0]
// 5.数组reduce

arr.reduce((num1, num2) => {
    return num1 > num2 ? num1 : num2}
)


    //求一组数中的所有数的和
    //求一组数中的所有数平均值
     var sum = 0;
     var max = 0;
     var arr = [1,2,4,6,1,71,23,40,20,12,10,23,25,16,90,100];
     var min = arr[0];
     for(var i=0;i<arr.length;i++){
        sum += arr[i];
```



```js
}*/
/*  老师的做法  */
  // 假设数组的第一项就是数组中最大的一项
  var max = arr[0];
  for(var i = 0; i<arr.length;i++) {
     if(max < arr[i]){
         max = arr[i];
     }
  }
  console.log(max);
```

```js
//  求一组数中的最大值
//    求一组数中的最小值
     document.write(sum);
     document.write("<div></div>"+sum/arr.length);
     for(var i = 0;i<arr.length;i++){
        max = max < arr[i+1] ? arr[i+1] : max;
     }
     document.write("<div></div>"+max);
     for(var i = 0;i<arr.length;i++){
        min = min > arr[i+1] ? arr[i+1] : min;
     }
```



```js
 document.write("<div></div>"+min);

// 将一个字符数组用|连接成一个新的字符串
var  arr = [“张宇”,“关飞”，“赵备”，“刘云”]
// 张宇|关飞|赵备|刘云
// 要求将数组中的0项去掉，将不为0的值存入一个新的数组，生成新的数组
        [10,12,14,18,0,20,0,15,0,”hello”,0]
 var arr = ["张三","关飞","李四","刘云"];
 var hebStr = "";
 for(var i = 0; i<arr.length;i++) {
   
 if(i==arr.length){
     hebStr +=arr[i];
 }else {
     hebStr +=arr[i] +"|";
 }
 }
 document.write(hebStr);
 // 第二种情况
  var str="";
  for(var i=0;i<arr.length-1;i++) {
     str += arr[i] + "|";
  }
  console.log(str+arr[arr.length-1]);

  var newNum = [];
  var num = 0;
  var arr_1 = [10,12,14,18,0,20,0,15,0,"hello",0];
  for( var i = 0 ; i<arr_1.length;i++){
     if(arr_1[i]!=0) {
         newNum[num] = arr_1[i];
         num++;
     }
  }
  document.write(newNum);
 /*第二种情况*/
  var arr = [1,2,4,6,1,71,23,40,20,12,10,23,25,16,90,100];
  var newArr = [];
  for(var i = 0; i<arr.length;i++){
     if(arr[i]!=0){
         newArr[newArr.length] = arr[i];
     }
  }
  document.write(newArr);
  ------------------------------------------------
         // 数组索引的规律：
         // 0  6   7-1  -0
         // 1  5   7-1  -1
         // 2  4   7-1  -2
// 翻转数组  长度/2  因为这执行一半就可以达到效果
// 思路：翻转数组就是数组数据之间交换位置
 var temp;
 for(var i =0;i<arr.length/2;i++){
    temp = arr[i];
    arr[i] = arr[arr.length-1-i];
    arr[arr.length-1-i] = temp;
 }
 /*数组Array(value)   --（number）值有一个是长度
 数组Array(value)   --（number）值有两个或两个以上，表示为参数
 数组Array("string") -- (string)值为参数*/
```





```js
 Array是object类型(对象)

 函数：函数就是代码复用的一种机制或是封装某种功能的代码段
 第一种定义函数的的方式：使用函数声明的方式
 函数的语法结构：
 函数的三要素： 函数名称(函数功能)   参数   返回值
 function  函数名(){
    函数体;(要执行的代码)
}
函数名称一般是动词，表示某种行为或是动作，推荐使用驼峰命名法。
函数的调用
函数声明完了，自己是不会执行的，只有调用才会执行。
调用函数： 函数名称();
参数分两种： 
形参： 在定义函数的时候，写的参数就是形参，是形式意义上的参数，起占位置的作用。
实形： 在调用函数的时候，传进来的数据才是实参，这是具有实际意义的数据值。
实参与形参的关系： 在调用函数的时候，是将实参的数据复制一份传递给形参。
定义函数的第一种方式： 函数声明的方式定义函数
function  函数名称([参数列表]){
  函数体(要执行的代码);
```
  }
  函数在声明子之后，不调用的时候，自己是不会执行的，只有调用的时候，才会执行，函数调用:   函数名称();
  `return` 的用法 ：

  1. 如果函数内部需要有数据返回的话，则使用return关键字，此数据就是这个函数的返回值，返回值返回之后，退出函数，`return`后面的代码不会执行。
  2. 如果函数内部没有数据要返回，但是使用了return关键字，程序执行完Return关键字之后，也是退出函数，return后面的代码不会执行。
    函数的返回值：
    1.如果函数中需要返回真正的数据值的话，需要使用return关键字返加，此值就是函数的返回值，undefined就不存在了。
    2.如果函数中没有真正的数据值要返回的话，则此函数会有一个默认的返回值，是undefined.
    函数的四种形式：   不包括undefined这个默认的返回值
  1.  无参无返回值   比如： `tellStory()`;
  2.  有参无返回值   比如：  alert(“弹出一个对话框”);
  3.   无参有返回值        比如：  `Math.random()`;
  4.   有参有返回值   比如 `getMoney()`   `prompt()`   `Number()`  `parseInt()`   `parseFloat()`   `String()`;
    函数的内部调用另外其它的函数：

  Js中的函数没有重载
  后台语言当中，只要是函数名称相同，但是函数的参数个数不同，或是函数的参数类型不同或是返回值不同，这样就构成了重载。重载的好处，就是在调用函数的时候，只调用一个函数名就可以。
  但是js中没有函数的重载。

```js
// js中没有函数的重载 ，如果函数名相同的话，后面的函数会将前面的函数给覆盖掉
function  getSum(num1,num2){
    return num1+num2;
}
function getSum(num1,num2,num3){
    return num1+num2+num3;
}
var sum = getSum(10,20);
  console.log(sum); //30  undefined  NaN
  通过上面的案例可以得出，js中的函数的形参的个数和实参的个数，可以不同

  arguments
   // arguments是js中的任何函数都有的，相当于是实参的一个备份,所有传过来的实参数据都相当于存在arguments伪数组当中
  // arguments是一个伪数组   伪数组就是不是通过 Array这个构造函数创建的数组，但是使用方法和真数组一模一样

  function getSum(){ //一个参数都不写
    var sum = 0;
    for(var i = 0; i < arguments.length; i++) {
       sum += arguments[i];
   }
   return sum;
}
var sum =  getSum(10,20,30,40,50,60,70,80,90,100);
console.log(sum);
```



```js
作用域
// 在没有学函数之前，js只有一个作用域，就是全局作用域(一对script标签之间的范围)
```
```js
  // 学了函数之后，就多了一个作用域，局部作用域(函数的内部)，js中没有块级作用域
//  {
//     var  a = 20;
//  }
//  console.log(a);
  // 两个作用域之间的关系：
  // 1. 局部作用域可以访问全局作用域内的变量或是函数
  // 2. 全局作用域无法访问局部作用域内的变量或是函数
  var c = 26;
  function getSum(num1,num2){
  var a = 30;
console.log(c);
    m = 80;
  }
  getSum(10,20);
  console.log(m);
//  console.log(a);
  // 全局变量和局部变量
  // 全局变量：就是在全局作用域内定义的变量  在函数的内部，不使用var声明的变量

  // 局部变量： 就是在函数的内部使用Var定义的变量就是局部变量，它的作用范围只属于函数内部
  if(true){
    var n = 20;
  }
  console.log(n);
```

###   封装函数的思想

>   1.确定函数的功能（确定函数名称）
>   2.确定哪些是可变的量，可变的量我们封装函数的时候设置成参数
>   3.确定函数执行完是否需要得到执行后的结果，如果需要，那么使用return返回，如果不需要，那么就不用返回.

#### 1.函数的声明的方式

  function 函数名(){

  }

#### 2.函数表达式的方式

var 变量名 = function(){

}

#### 匿名函数：没有函数名称的函数

```js
function() {}
var a = function() {}
```

#### 自执行函数写法：(第一种写法)

```js
(function() {alert("哈哈")})();
```



#### 第二种写法：

```js
(function(){}())
```

js的预解析：js文件在被加载后，浏览器中的js引擎会先将js文件浏览一遍，然后将会变量声明和函数的声明提到js文件的最前面。

### Math()：对数字进行操作的函数

1.`pow(x,y)` :次方
2.`round(x)` :把数四舍五入为最接近的整数。
3.`ceil(x)`  :天花板函数(对数进行上舍入)。
4.`floor(x)` :地板函数(对数进行下舍入)。
5.`abs(x)`   :绝对值。
6.`max(x,y)` :最大值。(可以设置多个值)
7.`min(x,y)` :最小值。(可以设置多个值)
8.`random()` :随机数默认0-1
9.`sqrt(x)`  :返回数的平方根。
立方根：1/3
平方根：1/2

### Date()：对日期时间进行操作的函数

1.`date.getTime()`          获取当前整个时间的一个毫秒
2.`date.getMilliseconds()`  获取当前时间的毫秒
3.`date.getSeconds()`       获取秒数
4.`date.getMinutes()`       获取分钟
5.`date.getHours()`         获取小时
6.`date.getDay()`           获取星期0-6
7.`date.getDate()`          获取日期中的天
8.`date.getMonth()`         获取月份0-11
9.`date.getFullYear()`      获取年份



### 数组对象Array

创建对象
变量名 = new Array();
变量名 = new Array(数组元素的个数);
变量名 = new Array(存入数组中的元素...);

属性：
`length`：返回数组的长度
`prototype`: 显示数组中的全部数据

方法：
`concat()`    连接两个或更多的数组，并返回结果
`indexOf()`   用来获取数据项在数组中对应的位置索引，如果存在返回该数据对应索引值，但是如果该数据不存在，返回-1，但是存在的情况下，只返回第一个匹配上得数据项对应的索引(n,m) m默认为0，indexof("String",开始的索引值)---返回找到当前位置，没找到返回-1，（开始匹配索引的位置 还有一个是lastindexOf）
`pop()`       删除并返回数组的最后一个元素
`push() `     向数组的末尾添加一个或多个元素，并返回新的长度
`shift()`     删除并返回数组的第一个元素
`unshift()`   向数组的开头添加一个或多个元素，并返回新的长度

`splice()`    删除元素，并向数组添加新元素(start, end,T) end值取不到 插入到end位置
`reverse()   颠倒数组中元素的顺序（翻转）
`sort()`      对数组的元素进行排序  function sortNumber(a,b){return a-b;}
`slice()     从某个已有的数组返回选定的元素(截取)
`toSource()`  代表对象的源代码
`toString()`  把数组转换为字符串，并返回结果
`toLocaleString()`  把数组转换为本地数组，并返回结果
`join()`      把数组的所有元素放入一个字符串，元素通过指定的分隔符进行分隔
`valueOf()`   返回数组对象的原始值

`string`对象的包装类型：这个对象我们能够直接取使用，不要去`new`，因为js默认的情况下已经帮我们创建了一个对象，当对象使用完成之后立即销毁。
`length`               长度
`toLocalUpperCase()`   大写
`charAt()`             指定索引的位置的字符
`charCodeAt()`         指定索引字符对应ASCII表
`concat() `            连接字符串
`slice()`              截取字符串
`substring()`         截取字符串
`indexOf()`            查找字符串  默认-1   lastindexOf()
`trim()`               去掉字符串前后
`replace() `           替换字符串
`split()`              分隔副直通车
`/a/g`   `grobel`   全局

注意：`number`类型和`boolean`类型也是对象的包装类型

对象：一组无序的数据的集合
对象的作用：可以用存储数据，使用面向对象编程可以提高开发效率
对象组成：属性、方法
属性：将物体所具有的特性或者特点抽象出来
方法：将物体的行为动作抽象出来

`new`:通过`Object`创建一个对象，将创建的这个对象的方法
`this`的使用：
1.`this`一般都是指向创建的这个对象
2.`this`指向事件的执行者

```js
//推荐使用（字面量创建对象）
var obj = {
  //属性
  name："涨价",
  age:18,
  gender: "女"
  //方法
  show:function(){
    //方法体
  }
}
```

JSON：数据的传输格式    XML:数据格式(被放弃)
for(var key in JSON/对象){
  对象[key]
}



<center>----------------------------------WEB API----------------------------</center>

### Document对象

文档（document）对象代表浏览器窗口中的文档，该对象是window对象的子对象。由于Window对象是DOM对象模型中的默认对象。因此window对象中的方法和子对象不需要使用window来引用。通过document对象可以访问HTML文档中包含的任何HTML标记并可以动态地改变html标记中的内容。

### JavaScript分三个部分：

ECMAScript标准:JS的基本的语法
1.DOM：Document Object Model  -->文档对象模型---操作页面的元素
2.BOM：Browser Object Model   -->浏览器对象模型---操作的是浏览器

### DOM：文档对象模型

文档：把一个html文件看成是一个文档，由于万物皆对象，所以把这个文档看成是一个对象
xml文件也可以看成是一个文档

HTML：展示信息，展示数据的
XML：侧重于存储数据
html文件看成是一个文档，那么这个文档看成是一个对象，文档中的所有的标签都可以看成是一个对象

页面中的每个标签，都是一个元素(element)，每个元素都可以看成是一个对象
标签可以嵌套，标签中有标签，元素中有元素

html页面中都有一个根标签--html--也叫根元素

页面中的有一个根元素（标签--html）,里面有很多的元素(有很多的标签，有很多的对象)

文档：一个页面就是一个文档

元素（element）：页面中的所有的标签都是元素，元素可以看成是对象
节点（node）：页面中所有的内容都是节点：标签，属性，文本
root:根

页面就是文档，文档中有根元素：html
html--->head--->body--->其他的标签

由文档及文档中的所有的元素（标签）组成的一个树形结构图，叫树状图(DOM树)

DOM经常进行的操作:

> 1.获取元素
> 2.动态创建元素
> 3.对元素进行操作(设置其属性或调用其方法)
> 4.事件(什么时机做相应的操作)

伪数组：具备数组的特征，不具备数组的行为
里面装有满足条件

点击操作：-----事件：就是一件事，有触发和响应，事件源
按钮被点击，弹出对话框
按钮-----事件源
点击-----事件名字
被点了---触发了
弹框了---响应

html跟js分离
html标签中的id属性中存储的值是唯一的
id属性就像的人的身份证一样，不能重复，页面中的唯一的标识：document.getElementById("id的属性的值")  -->返回的是一个元素对象

```js
btnObj.onclick = f2; //点击的时候触发事件  不加括号

//根据id属性的值从整个文档中获取这个元素(标签)
var btn = document.getElementById("btn1");
//为该元素注册点击事件，添加事件处理函数(匿名函数)
btn.onclick= function(){
  alert("哦，这真是太好了");
};
```



```js
//简化版
document.getElementById("btn1").onclick = function() {
  alert("哦，这真是太好了");
};
注意：先有按钮，才能获取，获取之后才能注册事件

注意：disabled 禁用 true/false  （开关属性）
只要出现就有效果

设置style优先级最高

document.querySelector(选择器);      ----只会得到单个
document.querySelectorAll(选择器);   ----会得到所有(伪数组)
```



凡是成对的标签，中间的文本内容，设置的时候，都使用innerText这个属性的方式

根据id获取按钮，为按钮注册点击事件，添加事件处理函数
document.getElementById("btn").onclick = function(){
  //根据id获取p标签，设置内容
  document.getElementById("p1").innerText="这是一个P";
};

document.getElementsByTagName("标签的名字");返回的是一个伪数组
无论获取的是一个标签，还是多个标签，最终都是在数组中存储的，这行代码的返回值就是一个数组

在某个元素的事件中，自己的事件中的this就是当前的这个元素对象
排他功能：
----让所有的元素注册点击事件，然后让被点击的事件设置点击后事件

规律：
1.在表单标签中，如果属性和值只有一个，并且值是这个属性本身，那么
在写js代码，dom操作的时候，这个属性值，是布尔类型就可以了
2.凡是css中这个属性是多个单词的写法， 在js代码中DOM操作的时候，把
-去掉，后面的单词的首字母大写即可。
阻止超链接的默认的跳转（在事件里）：return false

如果想要获取body标签直接用document.body

注意:getElementsByClassName()属于H5的(会有兼容问题)


根据id属性的值获取元素，返回一个元素对象
document.getElementById("id属性的值");

color:“gray”  //灰色
注意：字符串的运行效率会比数字的运行效率低
推荐使用数字判断

设置标签中的文本内容，应该使用context属性，谷歌，火狐支持，IE8不支持
### 下面的几个，有的浏览器不支持


根据标签名字获取元素,返回一个伪数组，里面保存多个DOM对象

`document`.getElementByTagName("标签名字");

根据name属性的值获取元素，返回一个伪数组，里面保存多个DOM对象

`document`.getElementsByName("name属性的值");

根据类样式的名字来获取元素，返回一个伪数组，里面保存多个dom对象

`document`.getElementsByClassName("类样式的名字");

根据选择器的方式获取元素，返回一个元素对象

`document`.querySelector("选择器的名字");

根据选择器获取元素，返回一个伪数组，里面保存多个DOM对象

`document`.querySelectorAll("选择器的名字");

注意：如果这个属性在浏览器中不支持，那么这个属性的类型是undefined

如果使用innerText主要是设置文本的，设置标签内容，是没有标签的效果的
如果innerHTML是可以设置文本内容
innerHTML主要的作用是在标签中设置新的html标签内容，是有标签效果的
想要设置标签内容，使用innerHTML，想要设置文本内容，
innerText或者textContext，或者innerHTML,推荐用innerHTML

获取的时候:
innerText可以获取标签中间的文本内容，但是标签中如果还有标签，
那么最里面的标签的文本内容也能获取

结论：
如果想要（获取）标签及内容，使用innerHTML
如果想要设置标签，使用innerHTML
想要设置文本，用innerText，或者innerHTML，或者textContent

html标签中有没有什么自带的属性可以存储成绩的  --没有
本身html标签没有这个属性，自己（程序员）添加的----自定义属性---为了存储一些数据

在html标签中添加的自定义属性，如果想要获取这个属性的值，需要使用getAttribute(“自定义属性名字”)才能获取这个属性的值

获取所有的li标签，然后为每个标签中动态的添加自定义属性和值
点击的时候获取该标签的自定义属性的值

↓↓↓↓↓API操作自定义属性↓↓↓↓ 
总结：
设置自定义属性：setAttribute(“属性的名字”,"属性的值")
获取自定义属性的值：getAttribute(“属性的名字”)

## node 节点
 节点:页面中所有的内容都是节点(标签,属性,文本:文字,空格,换行)
 文档：document---页面中的顶级对象
 元素:页面中所有的标签,   标签---元素--对象(通过DOM的方式来获取这得到了这个对象,此时这个对象叫DOM对象)
 节点的属性:
 作用:为了将来获取很多节点,得到节点中的标签(识别节点中的标签元素

 节点的类型:
 1 标签节点,2 属性节点,3 文本节点
 nodeType:节点的类型,1---标签节点,2----属性节点,3---文本节点
 nodeName:标签节点--大写的标签名字,属性节点---小写的属文本节点---#text
 nodeValue:标签---null,属性---属性的值,文本---文本内容
 if(node.nodeType==1&&node.nodeName=="P")

 动态------>元素的创建

### 三种元素创建的方式

> 1.document.write("标签代码及内容"); 如果在页面加载完毕后创页面中的内容会被干掉
>
> 2.父级对象.innerHTML="标签代码及内容";
>
> 3.可创建元素节点：
> document.createElement("标签名字");   得到的是一个对象,
> 可向节点的子节点列表的末尾添加新的子节点：
>    父级元素.appendChild(子级元素对象);//移除元素到另外一个元素
> 在您指定的已有子节点之前插入新的子节点：
>   父级元素.insertBefore(新的子级对象,参照的子级对象);
> 移除子元素：
>   父级元素.removeChild(要干掉的子级元素对象);

### 事件的绑定:为同一个元素绑定多个相同的事件

 三种方式:
> 对象.on事件名字=事件处理函数
> 如果是多个相同事件注册用这最后一个执行,之前的被覆盖了
>  my$("btn").onclick=function(){};
>
> 对象.addEventListener("没有on的事件名字",事件处理函数,false);
> my$("btn").addEventListener("click",function(){},false);
>
> 对象.attachEvent("有on的事件名字",事件处理函数);
> my$("btn").attachEvent("onclick",function(){});

### 事件有三个阶段：

捕获    -   事件从外到内传播的过程
目标    -   事件传播到达事件源的阶段
冒泡    -   事件从内到外传播的过程

事件在传播的过程中，如果某个节点也注册了同类型的事件就会被触发

得到某个父元素底下所有的子元素
children --父元素.children

得到某个元素的父元素
parentNode  --子元素.parentNode

得到的是所有的子节点，节点和 元素，节点包含元素，元素只是节点的一种
childNodes  --父元素.childNodes

### 兄弟元素

nextSibling   下一个节点
next          下一个节点
previous      上一个
Sibling       兄弟

previousSibling         上一个节点
    --元素.previousSibling

previousElementSibling  上一个元素
join          优化拼接字符串

insertBefore
  元素节点.insertBefore(新的节点，旧的节点);  //在旧节点前插入新节点
replaceChild
  元素节点.replaceChild(新节点，旧的节点);    //在目标节点替换新节点
cloneNode
  节点对象.cloneNode(是否深度克隆);           //克隆节点(复制)
innerText                                     //元素内部的文本

  不可以生成元素

绑定事件的区别:
 addEventListener("没有on的事件类型",事件处理函数,控制事件阶段的);
 attachEvent()

  ```js
    /*  addEventListener和attachEvent用来测试兼容问题
    
    - addEventListener() 方法用于向指定元素添加事件句柄。
    - 使用 removeEventListener() 方法来移除 addEventListener() 方法添加的事件句柄。
    - 
    - 注意: 不要使用 "on" 前缀。 例如，使用 "click" ,而不是使用 "onclick"。 
    - */
  ```

​    

 相同点: 都可以为元素绑定事件
 不同点:

> 1.方法名不一样
>  2.参数个数不一样addEventListener三个参数,attachEvent两个参数
>  3.addEventListener 谷歌,火狐,IE11支持,IE8不支持
>    attachEvent 谷歌火狐不支持,IE11不支持,IE8支持
>  4.this不同,addEventListener 中的this是当前绑定事件的对象
>    attachEvent中的this是window
>  5.addEventListener中事件的类型(事件的名字)没有on
>   attachEvent中的事件的类型(事件的名字)有on

<center>-------------------------个人理解------------------------------</center>

> 1、attachEvent()里面的第一个参数是onclick而不是和addEventListener()中的click一样;
> 2、attachEvent()使用和使用DOM0级的区别主要在于事件处理程序的作用域。
> 在使用DOM0级方法情况下，事件处理程序会在其所属的作用域内运行;
> 在使用attachEvent()方法的情况下，事件处理程序会在全局作用域中运行，
> 因此这里面的this相当于window。
> 必须是ie浏览器才可以使用。
> 3、另外，还有一点不同的是，使用attachEvent()绑定多个事件的时候，和addEventListener()以相反的顺序触发
> 4、和removeEventListener的作用一样，deachEvent()也可以移除attachEvent()添加的事件



解绑事件：
* 注意:用什么方式绑定事件,就应该用对应的方式解绑事件
* 1.解绑事件
* 对象.on事件名字=事件处理函数--->绑定事件
* 对象.on事件名字=null;
* 2.解绑事件
* 对象.addEventListener("没有on的事件类型",命名函数,false);

false要一致

绑定事件
* 对象.removeEventListener("没有on的事件类型",函数名字,false);
* 3.解绑事件
* 对象.attachEvent("on事件类型",命名函数);---绑定事件
* 对象.detachEvent("on事件类型",函数名字);

事件冒泡：多个元素嵌套，有层次关系，这些元素都注册了相同的事件，如果里面的元素 的事件触发了，外面的元素的该事件自动的触发了。
如何阻止事件冒泡
window.event.cancelBubble = true;  IE特有的，谷歌支持，火狐不支持
e.stopPropagation();   谷歌支持和火狐支持
function(e)     --e表示当前事件的所有[事件源]

得到鼠标移动的坐标：   clientX/clientY   浏览器的可视区坐标
target||srcElement      分别是谷歌和ie8里面的事件目标（事件源）
pageX|pageY       页面坐标，以body的左上角为原点


默认行为；preventDefault    阻止默认行为（跳转）
1、javascript：void(0);   ---a href
2、在事件处理程序里面return false
3、event.preventDefault();


attachEvent——兼容：IE7、IE8；不兼容firefox、chrome、IE9、IE10、IE11、safari、opera

addEventListener——兼容：firefox、chrome、IE、safari、opera；不兼容IE7、IE8


<center>-------------------------------BOM-----------------------------</center>
浏览器中有个顶级对象：window
全局函数和变量
window一般可以省略

页面中顶级对象：document
页面中所有的内容都是属于浏览器的，页面中的内容也都是window的

window.onload = function() {
  //页面加载完执行的js代码;
}

onunload 页面关闭后才触发的事件 --谷歌没效果

onbeforeunload 页面之前触发的事件  --谷歌没效果
<center>-------------------------------定时器----------------------------</center>
参数1：函数
参数2：时间 - - 毫秒 - - 1000毫秒=1秒
执行过程：页面加载完毕后，过了1秒，执行一个函数的代码，又过了1秒再执行
var timeId = setInterval(function(){
  //执行操作
},1000)

//要清理的定时的id的值
whidow.clearInterval(timeId);

一次性的定时器 
参数1:函数
参数2:时间 /毫秒
返回值：该定时器的id
语法：window.setTimeout(函数,时间);

注意：如果样式的代码是在style的标签中设置，外面是获取不到
如果样式的代码是style的属性设置，外面是可以获取的

如何得到left的值：设置成offsetLeft获取宽高/左右的值，返回数字类型
offsetLfet:父级元素margin + 父级元素padding + 父级元素的border+
自己的margin

脱离文档流
主要是自己的left和自己的margin
<center>-----------------------------scroll------------------------------</center>
scroll：卷曲======>滚出去
scrollWidth：元素中内容的实际的宽（没有边框），如果没有内容就是元素的宽
scrollHeight：元素中内容的实际的高（没有边框），如果没有内容就是元素的高
scrollTop：获取或者设置元素的内容垂直滚动出去的距离
scrollLeft：获取或者设置元素的内容滚动出去的水平距离

回到顶部：(设置javacript.void(0); 阻止跳转)
把滚动的距离设置为0
document.documentElement.scrollTop = 0;
<center>--------------------------轮播图---------------------------------</center>
20-30，人眼的识别的时间间隔，最低大概在30毫秒左右
计算机屏幕刷新频率，一般就是60帧

把ul移动到对应的位置 = 索引  *  图片的宽度  * -1

透明度
css
  opacity：控制整个元素的透明度的
  rgba() : 这是让背景透明
获取任意样式的当前值
  window.getComputedStyle(element);
    谷歌支持，ie8不支持
  element.currentStyle
    ie8支持的，谷歌不支持的

以上的两个API返回的结果都是对象
    需要通过对象的方式访问里面的属性

```js
res.属性名
res["属性名"]
```
这两个方式都是只能获取不能设置的 - 只读

兼容处理：
if(window.getComputedStyle != undefined){
  //兼容代码
}else if(element.currentStyle != undefined){
  //兼容代码
}

computed  :   计算过后的-综合各种因素，最终谁作用在元素身上，就是计算过后的

style      :  样式 

<center>----------------------------放大镜----------------------------</center>
最大距离：small的宽高 - mask的宽高

大图的当前位置 = 遮罩的当前位置 * 大图的最大移动距离/遮罩的最大移动距离

<center>----------------------------client-------------------------------</center>
可视区域:client
clientWidth:可视区域的宽
clientHeight:可视区域的高（）
通过arguments.length，可以得出：事件处理函数中实际上是一个参数的，这个参数和事件有关系，是一个对象    事件参数对象

pageX/Y解决拖动滚动条跟随功能（会有一点卡顿）

如果想要获取鼠标按下的时候的坐标，可以这样设置：e.pageX/e.pageY
获取元素/document.onmousedown = function(e){

}
包含区域：padding和内容


Document属性
alinkColor        链接文字被单击时的颜色，对应于`<body>`标记中的alink属性
all[]             存储html标记一个数组（该属性本身也是一个对象）
anchors[]         存储锚点的一个数组（该属性本身也是一个对象）
bgColor           文档的背景颜色
cookie            表示cookie的值
fgColor           文档的文本的颜色（不包含超链接的文字）
forms[]           存储窗口对象的一个数组
fileCreatedDate   创建文档的日期
fileModifiedDate  文档最后修改的日期
fileSize          当前文件的大小
lastModified      文档最后修改的时间
images[]          存储图像对象的一个数组
linkColor         未被访问的链接文字的颜色
links[]           存储link对象的一个数组
vlinkColor        表示已访问的链接文字的颜色
title             当前文档标题对象
body              当前文档主体对象
readyState        获取某个对象的当前状态
URL               获取或设置URL

Document对象的常用方法：
close             关闭文档的输出流
open              打开一个文档输出流并接收有write()和writeln()方法的创建页面内容
write             向文档中写入HTML或js语句
writeln           向文档中写入html或js语句，并以换行符结束
createElement     创建一个HTML标记
getElementById    获取指定Rd的THML标记

Doc常用事件：
onabort         对象载入被中断时触发
onblur          元素或窗口本身失去焦点时触发
onchange        改变`<select>`元素中的选项或其他表单元素失去焦点，并且在其获取焦点后内容发生过改变时触发
onclick         单击鼠标左键时触发。
ondblclick      双击鼠标左键时触发
onerror         出现错误时触发
onfocus         任何元素或窗口本身获得焦点时触发
onkeydown       键盘上得按键（包含Shift或alt等键）被按下时触发
onkeypass       键盘上得按键被按下，并产生一个字符时发生，即当按下shift或alt等键时不触发
onkeyup         释放键盘上得按键时触发
onload          页面完全载入后，在window对象上触发；所有框架都载入后，在框架集上触发；<img>标记指定的图像完全载入后，在其上触发`<object>` 标记指定的对象完全载入后，在其上触发
onmousedown     单击任何一个鼠标按键时触发
onmousemove     鼠标在某个元素上移动时持续触发
onmouseout      将鼠标从指定的元素上时触发
onmouseover     鼠标移到某个元素上时触发
onmouseup       释放任意一个鼠标按键时触发
onreset         单击重置按钮时，在`<form>`上触发
onresize        窗口或框架的大小发生改变时触发
onscroll        在任何带滚动条的元素或窗口上滚动时触发
onselect        选中文本时触发
onsubmit        单击提交按钮时，在`<form>`上触发
onunload        页面完全卸载后，在window对象上触发，或者所有框架都卸载后，在框架集上触发
keyup           键盘弹起
keydown         键盘按下