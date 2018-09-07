---
layout:     post
title:      "Python笔记day54（jQuery）|jQuery基础语法、选择器、筛选器"
subtitle:   "jQuery基础语法、选择器、筛选器"
date:       2018-09-07 18:17:00
author:     "崔天祥"
header-img: "img/post-bg-os-metro.jpg"
catalog:    true
tags:
    - Python
    - 编程
    - 前端
    - JavaScript
    - jQuery
---

> jQuery基础语法、选择器、筛选器 | 若本文不能正常显示 [点击这里](https://blog.csdn.net/qq_34755081/article/details/82500659)

####  jQuery介绍
jQuery是一个轻量级的、兼容多浏览器的JavaScript库。
jQuery使用户能够更方便地处理HTML Document、Events、实现动画效果、方便地进行Ajax交互，能够极大地简化JavaScript编程。它的宗旨就是：“Write less, do more.“
#### jQuery的优势
一款轻量级的JS框架。jQuery核心js文件才几十kb，不会影响页面加载速度。
丰富的DOM选择器,jQuery的选择器用起来很方便，比如要找到某个DOM对象的相邻元素，JS可能要写好几行代码，而jQuery一行代码就搞定了，再比如要将一个表格的隔行变色，jQuery也是一行代码搞定。
链式表达式。jQuery的链式操作可以把多个操作写在一行代码里，更加简洁。
事件、样式、动画支持。jQuery还简化了js操作css的代码，并且代码的可读性也比js要强。
Ajax操作支持。jQuery简化了AJAX操作，后端只需返回一个JSON格式的字符串就能完成与前端的通信。
跨浏览器兼容。jQuery基本兼容了现在主流的浏览器，不用再为浏览器的兼容问题而伤透脑筋。
插件扩展开发。jQuery有着丰富的第三方的插件，例如：树形菜单、日期控件、图片切换插件、弹出窗口等等基本前端页面上的组件都有对应插件，并且用jQuery插件做出来的效果很炫，并且可以根据自己需要去改写和封装插件，简单实用。
<!-- more -->
#### jQuery内容：
选择器
筛选器
样式操作
文本操作
属性操作
文档处理
事件
动画效果
插件
each、data、Ajax

#### jQuery版本
1.x：兼容IE678,使用最为广泛的，官方只做BUG维护，功能不再新增。因此一般项目来说，使用1.x版本就可以了，最终版本：1.12.4 (2016年5月20日)
2.x：不兼容IE678，很少有人使用，官方只做BUG维护，功能不再新增。如果不考虑兼容低版本的浏览器可以使用2.x，最终版本：2.2.4 (2016年5月20日)
3.x：不兼容IE678，只支持最新的浏览器。需要注意的是很多老的jQuery插件不支持3.x版。目前该版本是官方主要更新维护的版本。
维护IE678是一件让人头疼的事情，一般我们都会额外加载一个CSS和JS单独处理。值得庆幸的是使用这些浏览器的人也逐步减少，PC端用户已经逐步被移动端用户所取代，如果没有特殊要求的话，一般都会选择放弃对678的支持。


----------


#### 1，jQuery对象

jQuery对象就是通过jQuery包装DOM对象后产生的对象。jQuery对象是 jQuery独有的。如果一个对象是 jQuery对象，那么它就可以使用jQuery里的方法：例如$(“#i1”).html()。

$("#i1").html()的意思是：获取id值为 i1的元素的html代码。其中 html()是jQuery里的方法。

相当于： `document.getElementById("i1").innerHTML;`

虽然 jQuery对象是包装 DOM对象后产生的，但是 jQuery对象无法使用 DOM对象的任何方法，同理 DOM对象也没不能使用 jQuery里的方法。

一个约定，我们在声明一个jQuery对象变量的时候在变量名前面加上$：

var $variable = jQuery对像
var variable = DOM对象
$variable[0]//jQuery对象转成DOM对象

拿上面那个例子举例，jQuery对象和DOM对象的使用：

$("#i1").html();//jQuery对象可以使用jQuery的方法
$("#i1")[0].innerHTML;// DOM对象使用DOM的方法


----------


#### 2，jQuery基础语法

```
$(selector).action()
```

#### 查找标签
###### 选择器
**id选择器：**

```
$("#id")
```

**标签选择器：**

```
$("tagName")
```

**class选择器：**

```
$(".className")
```

**配合使用：**

```
$("div.c1")  // 找到有c1 class类的div标签
```

**所有元素选择器：**

```
$("*")
```

**组合选择器：**

```
$("#id, .className, tagName")
```

**层级选择器：**

x和y可以为任意选择器

```
$("x y");// x的所有后代y（子子孙孙）
$("x > y");// x的所有儿子y（儿子）
$("x + y")// 找到所有紧挨在x后面的y
$("x ~ y")// x之后所有的兄弟y
```

**基本筛选器：**


```
:first // 第一个
:last // 最后一个
:eq(index)// 索引等于index的那个元素
:even // 匹配所有索引值为偶数的元素，从 0 开始计数
:odd // 匹配所有索引值为奇数的元素，从 0 开始计数
:gt(index)// 匹配所有大于给定索引值的元素
:lt(index)// 匹配所有小于给定索引值的元素
:not(元素选择器)// 移除所有满足not条件的标签
:has(元素选择器)// 选取所有包含一个或多个标签在其内的标签(指的是从后代元素找)
```


**例子：**

```
$("div:has(h1)")// 找到所有后代中有h1标签的div标签
$("div:has(.c1)")// 找到所有后代中有c1样式类的div标签
$("li:not(.c1)")// 找到所有不包含c1样式类的li标签
$("li:not(:has(a))")// 找到所有后代中不含a标签的li标签
```

**练习：**

自定义模态框，使用jQuery实现弹出和隐藏功能。


```
<!DOCTYPE html>
<html lang="zh-CN">
<head>
  <meta charset="UTF-8">
  <meta http-equiv="x-ua-compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>自定义模态框</title>
  <style>
    .cover {
      position: fixed;
      left: 0;
      right: 0;
      top: 0;
      bottom: 0;
      background-color: darkgrey;
      z-index: 999;
    }
    .modal {
      width: 600px;
      height: 400px;
      background-color: white;
      position: fixed;
      left: 50%;
      top: 50%;
      margin-left: -300px;
      margin-top: -200px;
      z-index: 1000;
    }
    .hide {
      display: none;
    }
  </style>
</head>
<body>
<input type="button" value="弹" id="i0">

<div class="cover hide"></div>
<div class="modal hide">
  <label for="i1">姓名</label>
  <input id="i1" type="text">
   <label for="i2">爱好</label>
  <input id="i2" type="text">
  <input type="button" id="i3" value="关闭">
</div>
<script src="https://cdn.bootcss.com/jquery/3.2.1/jquery.min.js"></script>
<script>
  var tButton = $("#i0")[0];
  tButton.onclick=function () {
    var coverEle = $(".cover")[0];
    var modalEle = $(".modal")[0];

    $(coverEle).removeClass("hide");
    $(modalEle).removeClass("hide");
  };

  var cButton = $("#i3")[0];
  cButton.onclick=function () {
    var coverEle = $(".cover")[0];
    var modalEle = $(".modal")[0];

    $(coverEle).addClass("hide");
    $(modalEle).addClass("hide");
  }
</script>
</body>
</html>
```


**属性选择器：**

```
[attribute]
[attribute=value]// 属性等于
[attribute!=value]// 属性不等于
```

**例子：**

```
// 示例
<input type="text">
<input type="password">
<input type="checkbox">
$("input[type='checkbox']");// 取到checkbox类型的input标签
$("input[type!='text']");// 取到类型不是text的input标签
```

**表单常用筛选：**


```
:text
:password
:file
:radio
:checkbox

:submit
:reset
:button
```

**例子：**

```
$(":checkbox")  // 找到所有的checkbox
```

**表单对象属性:**

```
:enabled
:disabled
:checked
:selected
```

**例子：**

找到可用的input标签

```
<form>
  <input name="email" disabled="disabled" />
  <input name="id" />
</form>

$("input:enabled")  // 找到可用的input标签
```

 **找到被选中的option：**


```
<select id="s1">
  <option value="beijing">北京市</option>
  <option value="shanghai">上海市</option>
  <option selected value="guangzhou">广州市</option>
  <option value="shenzhen">深圳市</option>
</select>

$(":selected")  // 找到所有被选中的option
```


----------


#### 2，筛选器
下一个元素：

```
$("#id").next()
$("#id").nextAll()
$("#id").nextUntil("#i2")
```

上一个元素：

```
$("#id").prev()
$("#id").prevAll()
$("#id").prevUntil("#i2")
```

父亲元素：

```
$("#id").parent()
$("#id").parents()  // 查找当前元素的所有的父辈元素
$("#id").parentsUntil() // 查找当前元素的所有的父辈元素，
```

直到遇到匹配的那个元素为止。
儿子和兄弟元素：

```
$("#id").children();// 儿子们
$("#id").siblings();// 兄弟们
```

查找

搜索所有与指定表达式匹配的元素。这个函数是找出正在处理的元素的后代元素的好方法。

```
$("div").find("p")
```

等价于$("div p")

筛选

筛选出与指定表达式匹配的元素集合。这个方法用于缩小匹配的范围。用逗号分隔多个表达式。

```
$("div").filter(".c1")  // 从结果集中过滤出有c1样式类的
```

等价于 $("div.c1")

补充：

```
.first() // 获取匹配的第一个元素
.last() // 获取匹配的最后一个元素
.not() // 从匹配元素的集合中删除与指定表达式匹配的元素
.has() // 保留包含特定后代的元素，去掉那些不含有指定后代的元素。
.eq() // 索引值等于指定值的元素
```

**示例：左侧菜单**



```
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta http-equiv="x-ua-compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>左侧菜单示例</title>
  <style>
    .left {
      position: fixed;
      left: 0;
      top: 0;
      width: 20%;
      height: 100%;
      background-color: rgb(47, 53, 61);
    }

    .right {
      width: 80%;
      height: 100%;
    }

    .menu {
      color: white;
    }

    .title {
      text-align: center;
      padding: 10px 15px;
      border-bottom: 1px solid #23282e;
    }

    .items {
      background-color: #181c20;

    }
    .item {
      padding: 5px 10px;
      border-bottom: 1px solid #23282e;
    }

    .hide {
      display: none;
    }
  </style>
</head>
<body>

<div class="left">
  <div class="menu">
    <div class="title">菜单一</div>
    <div class="items">
      <div class="item">111</div>
      <div class="item">222</div>
      <div class="item">333</div>
    </div>
    <div class="title">菜单二</div>
    <div class="items hide">
      <div class="item">111</div>
      <div class="item">222</div>
      <div class="item">333</div>
    </div>
    <div class="title">菜单三</div>
    <div class="items hide">
      <div class="item">111</div>
      <div class="item">222</div>
      <div class="item">333</div>
    </div>
  </div>
</div>
<div class="right"></div>
<script src="https://cdn.bootcss.com/jquery/3.2.1/jquery.min.js"></script>

<script>
  $(".title").click(function (){  // jQuery绑定事件
    // 隐藏所有class里有.items的标签
    $(".items").addClass("hide");  //批量操作
    $(this).next().removeClass("hide");
  });
</script>
```