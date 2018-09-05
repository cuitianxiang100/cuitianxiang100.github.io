---
layout:     post
title:      "Python笔记day51（JavaScript）|JavaScript语言基础、数据类型、运算符、流程控制"
subtitle:   "JavaScript语言基础、数据类型、运算符、流程控制"
date:       2018-09-05 18:47:00
author:     "崔天祥"
header-img: "img/post-bg-os-metro.jpg"
catalog:    true
tags:
    - Python
    - 编程
    - 前端
    - JavaScript
---

> JavaScript语言基础、数据类型、运算符、流程控制 | 若本文不能正常显示 [点击这里](https://blog.csdn.net/qq_34755081/article/details/82427559)

#### 1，JavaScript概述
ECMAScript和JavaScript的关系
1996年11月，JavaScript的创造者--Netscape公司，决定将JavaScript提交给国际标准化组织ECMA，希望这门语言能够成为国际标准。次年，ECMA发布262号标准文件（ECMA-262）的第一版，规定了浏览器脚本语言的标准，并将这种语言称为ECMAScript，这个版本就是1.0版。

该标准一开始就是针对JavaScript语言制定的，但是没有称其为JavaScript，有两个方面的原因。一是商标，JavaScript本身已被Netscape注册为商标。二是想体现这门语言的制定者是ECMA，而不是Netscape，这样有利于保证这门语言的开发性和中立性。

因此ECMAScript和JavaScript的关系是，前者是后者的规格，后者是前者的一种实现。


尽管 ECMAScript 是一个重要的标准，但它并不是 JavaScript 唯一的部分，当然，也不是唯一被标准化的部分。实际上，一个完整的 JavaScript 实现是由以下 3 个不同部分组成的：

核心（ECMAScript） 
文档对象模型（DOM） Document object model (整合js，css，html)
浏览器对象模型（BOM） Broswer object model（整合js和浏览器）
简单地说，ECMAScript 描述了JavaScript语言本身的相关内容。

JavaScript 是脚本语言
JavaScript 是一种轻量级的编程语言。

JavaScript 是可插入 HTML 页面的编程代码。

JavaScript 插入 HTML 页面后，可由所有的现代浏览器执行。

JavaScript 很容易学习。

<!-- more -->
----------


#### 2，JavaScript引入方式
###### Script标签内写代码

```
<script>
  // 在这里写你的JS代码
</script>
```

###### 引入额外的JS文件

```
<script src="myscript.js"></script>
```


----------


#### 3，JavaScript语言规范
###### 注释（注释是代码之母）

```
// 这是单行注释

/*
这是
多行注释
*/
```

###### 结束符
JavaScript中的语句要以分号（;）为结束符。

**JavaScript语言基础**
###### 变量声明
JavaScript的变量名可以使用_，数字，字母，$组成，不能以数字开头。
声明变量使用 var 变量名; 的格式来进行声明

```
var name = "Alex";
var age = 18;
```

注意：

变量名是区分大小写的。

推荐使用驼峰式命名规则。

保留字不能用做变量名。

补充：

ES6新增了let命令，用于声明变量。其用法类似于var，但是所声明的变量只在let命令所在的代码块内有效。例如：for循环的计数器就很适合使用let命令。

```
for (let i=0;i<arr.length;i++){...}
```

ES6新增const用来声明常量。一旦声明，其值就不能改变。

```
const PI = 3.1415;
PI // 3.1415

PI = 3
// TypeError: "PI" is read-only
```

再次补充保留字：


```
abstract
boolean
byte
char
class
const
debugger
double
enum
export
extends
final
float
goto
implements
import
int
interface
long
native
package
private
protected
public
short
static
super
synchronized
throws
transient
volatile
```


----------


#### 4，JavaScript数据类型
JavaScript拥有动态类型

```
var x;  // 此时x是undefined
var x = 1;  // 此时x是数字
var x = "Alex"  // 此时x是字符串 
```

###### 数值(Number)
JavaScript不区分整型和浮点型，就只有一种数字类型。

```
var a = 12.34;
var b = 20;
var c = 123e5;  // 12300000
var d = 123e-5;  // 0.00123
```

还有一种NaN，表示不是一个数字（Not a Number）。

常用方法：

```
parseInt("123")  // 返回123
parseInt("ABC")  // 返回NaN,NaN属性是代表非数字值的特殊值。该属性用于指示某个值不是数字。
parseFloat("123.456")  // 返回123.456
```

###### 字符串(String)

```
var a = "Hello"
var b = "world;
var c = a + b; 
console.log(c);  // 得到Helloworld
```

常用方法：

```
方法	说明
.length	返回长度
.trim()	移除空白
.trimLeft()	移除左边的空白
.trimRight()	移除右边的空白
.charAt(n)	返回第n个字符
.concat(value, ...)	拼接
.indexOf(substring, start)	子序列位置
.substring(from, to)	根据索引获取子序列
.slice(start, end)	切片
.toLowerCase()	小写
.toUpperCase()	大写
.split(delimiter, limit)	分割
```

 

拼接字符串一般使用“+”



```
string.slice(start, stop)和string.substring(start, stop)：

两者的相同点：
如果start等于end，返回空字符串
如果stop参数省略，则取到字符串末
如果某个参数超过string的长度，这个参数会被替换为string的长度

substirng()的特点：
如果 start > stop ，start和stop将被交换
如果参数是负数或者不是数字，将会被0替换

silce()的特点：
如果 start > stop 不会交换两者
如果start小于0，则切割从字符串末尾往前数的第abs(start)个的字符开始(包括该位置的字符)
如果stop小于0，则切割在从字符串末尾往前数的第abs(stop)个字符结束(不包含该位置字符)
```


补充：

ES6中引入了模板字符串。模板字符串（template string）是增强版的字符串，用反引号（`）标识。它可以当做普通字符串使用，也可以用来定义多行字符串，或者在字符串中嵌入变量。

```
// 普通字符串
`这是普通字符串！`
// 多行文本
`这是多行的
文本`
// 字符串中嵌入变量
var name = "q1mi", time = "today";
`Hello ${name}, how are you ${time}?`
```


注意：

如果模板字符串中需要使用反引号，则在其前面要用反斜杠转义。

JSHint启用ES6语法支持:/* jshint esversion: 6 */

###### 布尔值(Boolean)
区别于Python，true和false都是小写。

```
var a = true;
var b = false;
```

**""(空字符串)、0、null、undefined、NaN都是false。**

###### null和undefined
null表示值是空，一般在需要指定或清空一个变量时才会使用，如 name=null;
undefined表示当声明一个变量但未初始化时，该变量的默认值是undefined。还有就是函数无明确的返回值时，返回的也是undefined。
null表示变量的值是空，undefined则表示只声明了变量，但还没有赋值。


###### 对象(Object)
JavaScript 中的所有事物都是对象：字符串、数值、数组、函数...此外，JavaScript 允许自定义对象。

JavaScript 提供多个内建对象，比如 String、Date、Array 等等。

对象只是带有属性和方法的特殊数据类型。

###### 数组

数组对象的作用是：使用单独的变量名来存储一系列的值。类似于Python中的列表。

```
var a = [123, "ABC"];
console.log(a[1]);  // 输出"ABC"
```

 常用方法：

```
方法	说明
.length	数组的大小
.push(ele)	尾部追加元素
.pop()	获取尾部的元素
.unshift(ele)	头部插入元素
.shift()	头部移除元素
.slice(start, end)	切片
.reverse()	反转
.join(seq)	将数组元素连接成字符串
.concat(val, ...)	连接数组
.sort()	排序
.forEach()	将数组的每个元素传递给回调函数
.splice()	删除元素，并向数组添加新元素。
.map()	返回一个数组元素调用函数处理后的值的新数组
```

关于sort()需要注意：

如果调用该方法时没有使用参数，将按字母顺序对数组中的元素进行排序，说得更精确点，是按照字符编码的顺序进行排序。要实现这一点，首先应把数组的元素都转换成字符串（如有必要），以便进行比较。

如果想按照其他标准进行排序，就需要提供比较函数，该函数要比较两个值，然后返回一个用于说明这两个值的相对顺序的数字。比较函数应该具有两个参数 a 和 b，其返回值如下：

若 a 小于 b，在排序后的数组中 a 应该出现在 b 之前，则返回一个小于 0 的值。
若 a 等于 b，则返回 0。
若 a 大于 b，则返回一个大于 0 的值。

示例：

```
function sortNumber(a,b){
    return a - b
}
var arr1 = [11, 100, 22, 55, 33, 44]
arr1.sort(sortNumber)
```

关于遍历数组中的元素，可以使用下面的方式：

```
var a = [10, 20, 30, 40];
for (var i=0;i<a.length;i++) {
  console.log(a[i]);
}
```

###### forEach()
语法：

```
forEach(function(currentValue, index, arr), thisValue)
```

参数：

|参数|	描述|
|---|---|
|function(currentValue, index, arr)|	必需。 数组中每个元素需要调用的函数。|
|thisValue|	可选。传递给函数的值一般用 "this" 值。如果这个参数为空， "undefined" 会传递给 "this" 值|
|currentValue|	必需。当前元素|
|index|	可选。当前元素的索引值。|
|arr|	可选。当前元素所属的数组对象。|
###### splice()
**语法：**

```
splice(index,howmany,item1,.....,itemX)
```

**参数：** 

|参数	|描述|
|---|---|
|index|	必需。规定从何处添加/删除元素。该参数是开始插入和（或）删除的数组元素的下标，必须是数字。|
|howmany|	必需。规定应该删除多少元素。必须是数字，但可以是 "0"。如果未规定此参数，则删除从 index 开始到原数组结尾的所有元素。|
|item1, ..., itemX	|可选。要添加到数组的新元素|
###### map()
**语法：**

```
map(function(currentValue,index,arr), thisValue)
```

**参数：**


|参数|	描述|
|---|---|
|function(currentValue, index, arr)|	必需。 数组中每个元素需要调用的函数。|
|thisValue|	可选。传递给函数的值一般用 "this" 值。如果这个参数为空， "undefined" 会传递给 "this" 值|
|currentValue|	必需。当前元素|
|index|	可选。当前元素的索引值。|
|arr|	可选。当前元素所属的数组对象。|
补充：

ES6新引入了一种新的原始数据类型（Symbol），表示独一无二的值。它是JavaScript语言的第7种数据类型。

###### 类型查询

```
typeof "abc"  // "string"
typeof null  // "object"
typeof true  // "boolean"
typeof 123 // "number"
```

typeof是一个一元运算符（就像++，--，！，- 等一元运算符），不是一个函数，也不是一个语句。

对变量或值调用 typeof 运算符将返回下列值之一：

```
undefined - 如果变量是 Undefined 类型的
boolean - 如果变量是 Boolean 类型的
number - 如果变量是 Number 类型的
string - 如果变量是 String 类型的
object - 如果变量是一种引用类型或 Null 类型的
```

----------


#### 5，运算符
###### 算数运算符

```
+ - * / % ++ --
```

###### 比较运算符

```
> >= < <= != == === !==
```

注意：

```
1 == “1”  // true
1 === "1"  // false
```

###### 逻辑运算符

```
&& || !
```

###### 赋值运算符

```
= += -= *= /=
```


----------


#### 6，流程控制

```
if-else
var a = 10;
if (a > 5){
  console.log("yes");
}else {
  console.log("no");
}
```

###### if-else if-else 

```
var a = 10;
if (a > 5){
  console.log("a > 5");
}else if (a < 5) {
  console.log("a < 5");
}else {
  console.log("a = 5");
}
```

###### switch

```
var day = new Date().getDay();
switch (day) {
  case 0:
  console.log("Sunday");
  break;
  case 1:
  console.log("Monday");
  break;
default:
  console.log("...")
}
```


switch中的case子句通常都会加break语句，否则程序会继续执行后续case中的语句。

###### for

```
for (var i=0;i<10;i++) {
  console.log(i);
}
while
var i = 0;
while (i < 10) {
  console.log(i);
  i++;
}
```

###### 三元运算

```
var a = 1;
var b = 2;
var c = a > b ? a : b
```

