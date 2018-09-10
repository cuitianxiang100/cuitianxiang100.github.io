---
layout:     post
title:      "Python笔记day55（jQuery）|操作标签、样式操作、文本操作、属性操作"
subtitle:   "操作标签、样式操作、文本操作、属性操作"
date:       2018-09-10 20:17:00
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

> 操作标签、样式操作、文本操作、属性操作 | 若本文不能正常显示 [点击这里](https://blog.csdn.net/qq_34755081/article/details/82595527)

#### 操作标签
#### 1，样式操作
**样式类**

```
addClass();// 添加指定的CSS类名。
removeClass();// 移除指定的CSS类名。
hasClass();// 判断样式存不存在
toggleClass();// 切换CSS类名，如果有就移除，如果没有就添加。
```

示例：开关灯和模态框

CSS

```
css("color","red")//DOM操作：tag.style.color="red"
```

示例：

```
$("p").css("color", "red"); //将所有p标签的字体设置为红色
```

位置：

```
offset()// 获取匹配元素在当前窗口的相对偏移或设置元素位置
position()// 获取匹配元素相对父元素的偏移
scrollTop()// 获取匹配元素相对滚动条顶部的偏移
scrollLeft()// 获取匹配元素相对滚动条左侧的偏移
.offset()方法允许我们检索一个元素相对于文档（document）的当前位置。
```
<!-- more -->
和 .position()的差别在于： .position()是相对于相对于父级元素的位移。

示例：

```
<!DOCTYPE html>
<html lang="zh-CN">
<head>
  <meta charset="UTF-8">
  <meta http-equiv="x-ua-compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>位置相关示例之返回顶部</title>
  <style>
    .c1 {
      width: 100px;
      height: 200px;
      background-color: red;
    }

    .c2 {
      height: 50px;
      width: 50px;

      position: fixed;
      bottom: 15px;
      right: 15px;
      background-color: #2b669a;
    }
    .hide {
      display: none;
    }
    .c3 {
      height: 100px;
    }
  </style>
</head>
<body>
<button id="b1" class="btn btn-default">点我</button>
<div class="c1"></div>
<div class="c3">1</div>
<div class="c3">2</div>
<div class="c3">3</div>
<div class="c3">4</div>
<div class="c3">5</div>
<div class="c3">6</div>
<div class="c3">7</div>
<div class="c3">8</div>
<div class="c3">9</div>
<div class="c3">10</div>
<div class="c3">11</div>
<div class="c3">12</div>
<div class="c3">13</div>
<div class="c3">14</div>
<div class="c3">15</div>
<div class="c3">16</div>
<div class="c3">17</div>
<div class="c3">18</div>
<div class="c3">19</div>
<div class="c3">20</div>
<div class="c3">21</div>
<div class="c3">22</div>
<div class="c3">23</div>
<div class="c3">24</div>
<div class="c3">25</div>
<div class="c3">26</div>
<div class="c3">27</div>
<div class="c3">28</div>
<div class="c3">29</div>
<div class="c3">30</div>
<div class="c3">31</div>
<div class="c3">32</div>
<div class="c3">33</div>
<div class="c3">34</div>
<div class="c3">35</div>
<div class="c3">36</div>
<div class="c3">37</div>
<div class="c3">38</div>
<div class="c3">39</div>
<div class="c3">40</div>
<div class="c3">41</div>
<div class="c3">42</div>
<div class="c3">43</div>
<div class="c3">44</div>
<div class="c3">45</div>
<div class="c3">46</div>
<div class="c3">47</div>
<div class="c3">48</div>
<div class="c3">49</div>
<div class="c3">50</div>
<div class="c3">51</div>
<div class="c3">52</div>
<div class="c3">53</div>
<div class="c3">54</div>
<div class="c3">55</div>
<div class="c3">56</div>
<div class="c3">57</div>
<div class="c3">58</div>
<div class="c3">59</div>
<div class="c3">60</div>
<div class="c3">61</div>
<div class="c3">62</div>
<div class="c3">63</div>
<div class="c3">64</div>
<div class="c3">65</div>
<div class="c3">66</div>
<div class="c3">67</div>
<div class="c3">68</div>
<div class="c3">69</div>
<div class="c3">70</div>
<div class="c3">71</div>
<div class="c3">72</div>
<div class="c3">73</div>
<div class="c3">74</div>
<div class="c3">75</div>
<div class="c3">76</div>
<div class="c3">77</div>
<div class="c3">78</div>
<div class="c3">79</div>
<div class="c3">80</div>
<div class="c3">81</div>
<div class="c3">82</div>
<div class="c3">83</div>
<div class="c3">84</div>
<div class="c3">85</div>
<div class="c3">86</div>
<div class="c3">87</div>
<div class="c3">88</div>
<div class="c3">89</div>
<div class="c3">90</div>
<div class="c3">91</div>
<div class="c3">92</div>
<div class="c3">93</div>
<div class="c3">94</div>
<div class="c3">95</div>
<div class="c3">96</div>
<div class="c3">97</div>
<div class="c3">98</div>
<div class="c3">99</div>
<div class="c3">100</div>

<button id="b2" class="btn btn-default c2 hide">返回顶部</button>
<script src="jquery-3.2.1.min.js"></script>
<script>
  $("#b1").on("click", function () {
    $(".c1").offset({left: 200, top:200});
  });


  $(window).scroll(function () {
    if ($(window).scrollTop() > 100) {
      $("#b2").removeClass("hide");
    }else {
      $("#b2").addClass("hide");
    }
  });
  $("#b2").on("click", function () {
    $(window).scrollTop(0);
  })
</script>
</body>
</html>
```

尺寸：

```
height()
width()
innerHeight()
innerWidth()
outerHeight()
outerWidth()
```


----------


#### 2，文本操作
HTML代码：

```
html()// 取得第一个匹配元素的html内容
html(val)// 设置所有匹配元素的html内容
```

文本值：

```
text()// 取得所有匹配元素的内容
text(val)// 设置所有匹配元素的内容
```

值：

```
val()// 取得第一个匹配元素的当前值
val(val)// 设置所有匹配元素的值
val([val1, val2])// 设置checkbox、select的值
```

示例：

获取被选中的checkbox或radio的值：

```
<label for="c1">女</label>
<input name="gender" id="c1" type="radio" value="0">
<label for="c2">男</label>
<input name="gender" id="c2" type="radio" value="1">
```

可以使用：

```
$("input[name='gender']:checked").val()
```


```
<!DOCTYPE html>
<html lang="zh-CN">
<head>
  <meta charset="UTF-8">
  <meta http-equiv="x-ua-compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>文本操作之登录验证</title>
  <style>
    .error {
      color: red;
    }
  </style>
</head>
<body>

<form action="">
  <div>
    <label for="input-name">用户名</label>
    <input type="text" id="input-name" name="name">
    <span class="error"></span>
  </div>
  <div>
    <label for="input-password">密码</label>
    <input type="password" id="input-password" name="password">
    <span class="error"></span>
  </div>
  <div>
    <input type="button" id="btn" value="提交">
  </div>
</form>
<script src="https://cdn.bootcss.com/jquery/3.2.1/jquery.min.js"></script>
<script>
  $("#btn").click(function () {
    var username = $("#input-name").val();
    var password = $("#input-password").val();

    if (username.length === 0) {
      $("#input-name").siblings(".error").text("用户名不能为空");
    }
    if (password.length === 0) {
      $("#input-password").siblings(".error").text("密码不能为空");
    }
  })
</script>
</body>
</html>
```


----------


#### 3，属性操作
用于ID等或自定义属性：

```
attr(attrName)// 返回第一个匹配元素的属性值
attr(attrName, attrValue)// 为所有匹配元素设置一个属性值
attr({k1: v1, k2:v2})// 为所有匹配元素设置多个属性值
removeAttr()// 从每一个匹配的元素中删除一个属性
```

用于checkbox和radio

```
prop() // 获取属性
removeProp() // 移除属性
```

注意：

在1.x及2.x版本的jQuery中使用attr对checkbox进行赋值操作时会出bug，在3.x版本的jQuery中则没有这个问题。为了兼容性，我们在处理checkbox和radio的时候尽量使用特定的prop()，不要使用attr("checked", "checked")。

```
<input type="checkbox" value="1">
<input type="radio" value="2">
<script>
  $(":checkbox[value='1']").prop("checked", true);
  $(":radio[value='2']").prop("checked", true);
</script>
```


----------
**自定义点击显示登录框代码**

```
<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <title>自定义模态框示例</title>
    <style>
        .cover {
            position: absolute;
            top: 0;
            right: 0;
            bottom: 0;
            left: 0;
            background-color: rgba(0,0,0,0.4);
            z-index: 998;
        }

        .modal {
            height: 400px;
            width: 600px;
            background-color: white;
            position: absolute;
            top: 50%;
            left: 50%;
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
<button id="b1">点击登录</button>
<div class="cover hide"></div>
<div class="modal hide">
    <form>
        <p>
            <label>用户名:
                <input type="text">
            </label>
        </p>
        <p>
            <label>密码:
                <input type="password">
            </label>
        </p>
        <p>
            <input type="submit" value="登录">
            <input id="cancel" type="button" value="取消">
        </p>
    </form>
</div>
<script src="jquery-3.2.1.min.js"></script>
<script>
    // 找到点击弹出模态框的按钮
    $("#b1").click(function () {
        // 把.cover和.modal显示出来(去除掉.hide)
        $(".cover").removeClass("hide");  // 显示背景
        $(".modal").removeClass("hide"); // 显示模态框
    });

    // 找到取消按钮,绑定事件
    $("#cancel").click(function () {
        // 给背景和模态框都加上hide类
        $(".cover").addClass("hide");
        $(".modal").addClass("hide");
    })
</script>
</body>
</html>
```