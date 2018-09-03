---
layout:     post
title:      "Python笔记day48（前端|CSS）|css语法、选择器、优先级"
subtitle:   "css语法、选择器、优先级"
date:       2018-09-03 19:47:00
author:     "崔天祥"
header-img: "img/post-bg-os-metro.jpg"
catalog:    true
tags:
    - Python
    - 编程
    - 前端
---

> css语法、选择器、优先级 | 若本文不能正常显示 [点击这里](https://blog.csdn.net/qq_34755081/article/details/82353100)
#### CSS介绍
CSS（Cascading Style Sheet，层叠样式表)定义如何显示HTML元素。

当浏览器读到一个样式表，它就会按照这个样式表来对文档进行格式化（渲染）。

#### 1，CSS语法
###### CSS实例
每个CSS样式由两个组成部分：选择器和声明。声明又包括属性和属性值。每个声明之后用分号结束。
###### CSS注释

```
/*这是注释*/
```


###### CSS的几种引入方式
**行内样式**
行内式是在标记的style属性中设定CSS样式。不推荐大规模使用。

```
<p style="color: red">Hello world.</p>
```

**内部样式**
嵌入式是将CSS样式集中写在网页的`<head></head>标签对的<style></style>`标签对中。格式如下：

<!-- more -->
```
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <style>
        p{
            background-color: #2b99ff;
        }
    </style>
</head>
```


**外部样式**
外部样式就是将css写在一个单独的文件中，然后在页面进行引入即可。推荐使用此方式。

```
<link href="mystyle.css" rel="stylesheet" type="text/css"/>
```


----------


#### 2，CSS选择器
###### 1）基本选择器
###### 元素选择器

```
p {color: "red";}
```

###### ID选择器

```
#i1 {
  background-color: red;
}
```

###### 类选择器
.

```
c1 {
  font-size: 14px;
}
p.c1 {
  color: red;
}
```

注意：

样式类名不要用数字开头（有的浏览器不认）。

标签中的class属性如果有多个，要用空格分隔。

###### 通用选择器

```
* {
  color: white;
}
```

###### 2）组合选择器
###### 后代选择器

```
/*li内部的a标签设置字体颜色*/
li a {
  color: green;
}
```

###### 儿子选择器

```
/*选择所有父级是 <div> 元素的 <p> 元素*/


div>p {
  font-family: "Arial Black", arial-black, cursive;
}
```

###### 毗邻选择器

```
/*选择所有紧接着<div>元素之后的<p>元素*/
div+p {
  margin: 5px;
}
```

###### 弟弟选择器

```
/*i1后面所有的兄弟p标签*/
#i1~p {
  border: 2px solid royalblue;
}
```

###### 属性选择器

```
/*用于选取带有指定属性的元素。*/
p[title] {
  color: red;
}
/*用于选取带有指定属性和值的元素。*/
p[title="213"] {
  color: green;
}
```


###### 不怎么常用的属性选择器
 

```
/*找到所有title属性以hello开头的元素*/
[title^="hello"] {
  color: red;
}

/*找到所有title属性以hello结尾的元素*/
[title$="hello"] {
  color: yellow;
}

/*找到所有title属性中包含（字符串包含）hello的元素*/
[title*="hello"] {
  color: red;
}

/*找到所有title属性(有多个值或值以空格分割)中有一个值为hello的元素：*/
[title~="hello"] {
  color: green;
}

不怎么常用的属性选择器
```

###### 3）分组和嵌套
###### 分组
当多个元素的样式相同的时候，我们没有必要重复地为每个元素都设置样式，我们可以通过在多个选择器之间使用逗号分隔的分组选择器来统一设置元素样式。 

例如：

```
div, p {
  color: red;
}
```

上面的代码为div标签和p标签统一设置字体为红色。

通常，我们会分两行来写，更清晰:

```
div,
p {
  color: red;
}
```

###### 嵌套
多种选择器可以混合起来使用，比如：.c1类内部所有p标签设置字体颜色为红色。

```
.c1 p {
  color: red;
}
```
###### 4，选择器的优先级
###### CSS继承
继承是CSS的一个主要特征，它是依赖于祖先-后代的关系的。继承是一种机制，它允许样式不仅可以应用于某个特定的元素，还可以应用于它的后代。例如一个body定义了的字体颜色值也会应用到段落的文本中。

```
body {
  color: red;
}
```

此时页面上所有标签都会继承body的字体颜色。然而CSS继承性的权重是非常低的，是比普通元素的权重还要低的0。

我们只要给对应的标签设置字体颜色就可覆盖掉它继承的样式。

```
p {
  color: green;
}
```

此外，继承是CSS重要的一部分，我们甚至不用去考虑它为什么能够这样，但CSS继承也是有限制的。有一些属性不能被继承，如：border, margin, padding, background等。

选择器的优先级
我们上面学了很多的选择器，也就是说在一个HTML页面中有很多种方式找到一个元素并且为其设置样式，那浏览器根据什么来决定应该应用哪个样式呢？

其实是按照不同选择器的权重来决定的，具体的选择器权重计算方式如下图：

![这里写图片描述](https://img-blog.csdn.net/2018090318571840?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM0NzU1MDgx/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

除此之外还可以通过添加 !import方式来强制让样式生效，但并不推荐使用。因为如果过多的使用!import会使样式文件混乱不易维护。

万不得已可以使用!import

