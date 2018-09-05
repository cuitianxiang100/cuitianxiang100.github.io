---
layout:     post
title:      "Python笔记day50（前端|CSS）|opacity属性、transition属性、静态商城页面"
subtitle:   "opacity属性、transition属性、静态商城页面"
date:       2018-09-05 12:47:00
author:     "崔天祥"
header-img: "img/post-bg-os-metro.jpg"
catalog:    true
tags:
    - Python
    - 编程
    - 前端
---

> opacity属性、transition属性、静态商城页面 | 若本文不能正常显示 [点击这里](https://blog.csdn.net/qq_34755081/article/details/82427038)

#### 1，内容回顾

```
	1. 伪类和伪元素
		1. 伪类
			1. :link
			2. :visited
			3. :hover (重要)
			4. :active
			5. :focus(input标签获取光标焦点)
		2. 伪元素
			1. :first-letter
			2. :before(重要 在内部前面添加)
			3. :after(重要 在内部后面添加)
			
	2. CSS属性
		1. 字体
			1. font-family
			2. font-size
			3. font-weight
		2. 文本属性
			1. text-align 对齐(重要)
			2. text-decoration 装饰 (去除a标签的下划线(text-decoration: none))
			3. text-indent 首行缩进
			
		3. 背景属性
			1. background-color  背景颜色
			2. background-image  背景图片(九宫格涮葫芦娃)  url() no-repeat 50% 50%
			
		4. color
			1. red (直接写名字)
			2. #FF0000
			3. rgb(255, 0, 0)  --> rgba(255,0,0,0.5)
			
		5. 边框属性 border
			1. border-width (边框宽度)
			2. border-style (边框样式)
			3. border-color (边框颜色)
			
			简写:
				border: 1px solid red;
```
<!-- more -->
```				
		6. CSS盒子模型
		
			1. content (内容)
			2. padding (内填充) 调整内容和边框之间距离时使用这个属性
			3. border  (边框)
			4. margin  (外边距) 多用于调整调整标签之间的距离 (注意两个挨着的标签margin取最大值)
			
			注意: 要习惯看浏览器console窗口那个盒子模型
		
		7. display (标签的展现形式)
			1. inline
			2. block   菜单里面的a标签可以设置成block
			3. inline-block
			4. none  (不让标签显示,不占位)
		
		8. float(浮动)
			1. 多用于实现布局效果
				1. 顶部的导航条
				2. 页面左右分栏 (博客页面:左边20%,右边80%)
			2. float
				1. 任何标签都可以浮动,浮动之后都会变成块级 a标签float之后就可以设置高和宽
			3. float取值:
				1. left
				2. right
				3. none
		9. clear 清除浮动--> 清除浮动的副作用(内容飞出,父标签撑不起来)
			1. 结合伪元素来实现
				.clearfix:after {
					content: "",
					display: "block",
					clear: both;
				}
				
			2. clear取值:
				1. left
				2. right
				3. both
		10. overflow
			1. 标签的内容放不下(溢出)
			
			2. 取值:
				1. hidden  --> 隐藏
				2. scroll  --> 出现滚动条
				3. auto
				4. scroll-x
				5. scroll-y
			
			例子:
				圆形头像的例子
					1. overflow: hidden
					2. border-radius: 50%  (圆角)
		11. 定位 position
			1. static(默认)
			
			2. relative(相对定位 --> 相当于原来的位置)
			
			3. absolute(绝对定位 -->相当对于定位过的前辈标签)
			
			4. fixed (固定 --> 返回顶部按钮示例)
		
		
			补充:
				脱离文档流的3种方式
					float
					absolute
					fixed
			
		12. opacity (不透明度)
			1. 取值0~1
			2. 和rgba()的区别:
				1. opacity改变元素\子元素的透明度效果
				2. rgba()只改变背景颜色的透明度效果
		
		13. z-index
			1. 数值越大,越靠近你
			2. 只能作用于定位过的元素
			
			3. 自定义的模态框示例
```
			


----------

#### 2，opacity

```
 opacity (不透明度)
			1. 取值0~1
			2. 和rgba()的区别:
				1. opacity改变元素\子元素的透明度效果
				2. rgba()只改变背景颜色的透明度效果
```

```
opacity: value|inherit;
```

|值|	描述	|
|---|---|
|value|	规定不透明度。从 0.0 （完全透明）到 1.0（完全不透明）。	|
|inherit|	应该从父元素继承 opacity 属性的值。|


----------
#### 3，transition

定义和用法
transition 属性是一个简写属性，用于设置四个过渡属性：

transition-property
transition-duration
transition-timing-function
transition-delay
注释：请始终设置 transition-duration 属性，否则时长为 0，就不会产生过渡效果。

```
transition: property duration timing-function delay;
```

|值|	描述|
|---|---|
|transition-property|	规定设置过渡效果的 CSS 属性的名称。|
|transition-duration|	规定完成过渡效果需要多少秒或毫秒。|
|transition-timing-function|	规定速度效果的速度曲线。|
|transition-delay|	定义过渡效果何时开始。|
把鼠标指针放到 div 元素上，其宽度会从 100px 逐渐变为 300px：

```
<!DOCTYPE html>
<html>
<head>
<style> 
div
{
width:100px;
height:100px;
background:blue;
transition:width 2s;
-moz-transition:width 2s; /* Firefox 4 */
-webkit-transition:width 2s; /* Safari and Chrome */
-o-transition:width 2s; /* Opera */
}

div:hover
{
width:300px;
}
</style>
</head>
<body>

<div></div>

<p>请把鼠标指针移动到蓝色的 div 元素上，就可以看到过渡效果。</p>

<p><b>注释：</b>本例在 Internet Explorer 中无效。</p>

</body>
</html>

```


----------
#### 4，纯html+CSS静态商场页面
![这里写图片描述](https://img-blog.csdn.net/2018090516593569?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM0NzU1MDgx/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
![这里写图片描述](https://img-blog.csdn.net/20180905165948482?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM0NzU1MDgx/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
**shop.html**

```
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>mi.com</title>
    <link href="https://cdn.bootcss.com/normalize/8.0.0/normalize.min.css" rel="stylesheet">
    <link rel="stylesheet" href="./css/mi.css">
</head>

<body>
    <!-- 顶部导航栏 start -->
    <div class="nav">
        <div class="container">
            <ul class="nav-left">
                <li>
                    <a href="">玉米商城</a>
                </li>
                <li>
                    <a href="">链接A</a>
                </li>
                <li>
                    <a href="">链接B</a>
                </li>
                <li>
                    <a href="">链接C</a>
                </li>
                <li>
                    <a href="">链接D</a>
                </li>
            </ul>
            <ul class="nav-right fr">
                <li>
                    <a href="">登录</a>
                </li>
                <li>
                    <a href="">注册</a>
                </li>
                <li>
                    <a href="">消息</a>
                </li>
                <li>
                    <a href="">购物车</a>
                </li>
            </ul>
        </div>

    </div>
    <!-- 顶部导航栏 end -->

    <!-- 首屏 开始 -->
    <div class="main">
        <!-- 目录导航 开始 -->
        <div class="main-header">
            <div class="logo fl">
                <img src="./img/logo.png" alt="">
                <img src="./img/logo_r.png" alt="">
            </div>
            <ul class="top-menu fl">
                <li>
                    <a href="">玉米手机</a>
                </li>
                <li>
                    <a href="">苞米</a>
                </li>
                <li>
                    <a href="">电视</a>
                </li>
                <li>
                    <a href="">爆米花</a>
                </li>
                <li>
                    <a href="">路由器</a>
                </li>
                <li>
                    <a href="">智能硬件</a>
                </li>
                <li>
                    <a href="">服务</a>
                </li>
                <li>
                    <a href="">社区</a>
                </li>
            </ul>
            <div class="main-search fr">
                <input type="text">
                <button>搜索</button>
            </div>
        </div>
        <!-- 目录导航 结束 -->

        <!-- 菜单+轮播 开始 -->
        <div class="menu clearfix">
            <div class="sidebar fl">
                <ul class="item-list">
                    <li>
                        <a href="">手机 电话卡</a>
                    </li>
                    <li>
                        <a href="">电视 盒子</a>
                    </li>
                    <li>
                        <a href="">笔记本</a>
                    </li>
                    <li>
                        <a href="">智能 家电</a>
                    </li>
                    <li>
                        <a href="">健康 家居</a>
                    </li>
                    <li>
                        <a href="">出行 儿童</a>
                    </li>
                    <li>
                        <a href="">路由器 手机配件</a>
                    </li>
                    <li>
                        <a href="">移动电源 插线板</a>
                    </li>
                    <li>
                        <a href="">耳机 音箱</a>
                    </li>
                    <li>
                        <a href="">生活 米兔</a>
                    </li>
                </ul>
            </div>
            <div class="carousel fr">
                <img src="./img/carousel.png" alt="">
            </div>
        </div>
        <!-- 菜单+轮播 结束 -->

        <!-- 副广告区 开始 -->
        <div class="main-down clearfix">
            <div class="fl">
                <a class="pic">
                    <img src="./img/pic0.png" alt="">
                </a>
            </div>
            <div class="fr">
                <a class="pic-item first">
                    <img src="./img/pic1.png" alt="">
                </a>
                <a class="pic-item">
                    <img src="./img/pic2.png" alt="">
                </a>
                <a class="pic-item">
                    <img src="./img/pic3.png" alt="">
                </a>
            </div>

        </div>
        <!-- 副广告区 结束 -->

        <!-- 闪购 开始 -->
        <div class="flash-sale">
            <h1>限时闪购</h1>
            <div class="flash-list clearfix">
                <div class="flash-left fl">
                    <a class="flash-item first" href="">
                        <img src="./img/flash0.png" alt="">
                    </a>
                </div>
                <div class="flash-right fr">
                    <a class="flash-item" href="">
                        <img src="./img/flash1.png" alt="">
                    </a>
                    <a class="flash-item" href="">
                        <img src="./img/flash2.png" alt="">
                    </a>
                    <a class="flash-item" href="">
                        <img src="./img/flash1.png" alt="">
                    </a>
                    <a class="flash-item" href="">
                        <img src="./img/flash2.png" alt="">
                    </a>
                </div>

            </div>
        </div>
        <!-- 闪购 结束 -->
    </div>

    <!-- 首屏 结束 -->

    <!-- 商品展示 开始 -->
    <div class="goods">
        <div class="container">
            <div class="goods-box clearfix">
                <h2>家电</h2>
                <div class="goods-box-left fl">
                    <img src="./img/goods-left0.png" alt="">
                </div>
                <div class="goods-box-right fr">
                    <div class="goods-item fl">
                        <div class="item-img">
                            <a href="">
                                <img src="./img/item1.png" alt="">
                            </a>

                        </div>
                        <p class="item-title">
                            <a href="">商品名称</a>
                        </p>
                        <p class="item-price">1999元</p>
                    </div>
                    <div class="goods-item fl">
                        <div class="item-img">
                            <a href="">
                                <img src="./img/item2.png" alt="">
                            </a>
                        </div>
                        <p class="item-title">
                            <a href="">商品名称</a>
                        </p>
                        <p class="item-price">1999元</p>
                    </div>
                    <div class="goods-item fl">
                        <div class="item-img">
                            <a href="">
                                <img src="./img/item3.png" alt="">
                            </a>
                        </div>
                        <p class="item-title">
                            <a href="">商品名称</a>
                        </p>
                        <p class="item-price">1999元</p>
                    </div>
                    <div class="goods-item fl">
                        <div class="item-img">
                            <a href="">
                                <img src="./img/item4.png" alt="">
                            </a>
                        </div>
                        <p class="item-title">
                            <a href="">商品名称</a>
                        </p>
                        <p class="item-price">1999元</p>
                    </div>
                    <div class="goods-item fl">
                        <div class="item-img">
                            <a href="">
                                <img src="./img/item4.png" alt="">
                            </a>

                        </div>
                        <p class="item-title">
                            <a href="">商品名称</a>
                        </p>
                        <p class="item-price">1999元</p>
                    </div>
                    <div class="goods-item fl">
                        <div class="item-img">
                            <a href="">
                                <img src="./img/item3.png" alt="">
                            </a>

                        </div>
                        <p class="item-title">
                            <a href="">商品名称</a>
                        </p>
                        <p class="item-price">1999元</p>
                    </div>
                    <div class="goods-item fl">
                        <div class="item-img">
                            <a href="">
                                <img src="./img/item2.png" alt="">
                            </a>

                        </div>
                        <p class="item-title">
                            <a href="">商品名称</a>
                        </p>
                        <p class="item-price">1999元</p>
                    </div>
                    <div class="goods-item fl">
                        <div class="item-img">
                            <a href="">
                                <img src="./img/item1.png" alt="">
                            </a>

                        </div>
                        <p class="item-title">
                            <a href="">商品名称</a>
                        </p>
                        <p class="item-price">1999元</p>
                        <!-- <div class="item-memo">
                            <a href="">我真的是个天才，哈哈哈哈</a>
                        </div> -->
                    </div>

                </div>
            </div>
            <div class="goods-box clearfix">
                <h2>智能</h2>
                <div class="goods-box-left fl">
                    <a class="left-item first" href="">
                        <img src="./img/goods-left1.png" alt="">
                    </a>
                    <a class="left-item" href="">
                        <img src="./img/goods-left1.png" alt="">
                    </a>
                </div>
                <div class="goods-box-right fr">
                    <div class="goods-item fl">
                        <div class="item-img">
                            <a href="">
                                <img src="./img/item1.png" alt="">
                            </a>

                        </div>
                        <p class="item-title">
                            <a href="">商品名称</a>
                        </p>
                        <p class="item-price">1999元</p>
                    </div>
                    <div class="goods-item fl">
                        <div class="item-img">
                            <a href="">
                                <img src="./img/item2.png" alt="">
                            </a>
                        </div>
                        <p class="item-title">
                            <a href="">商品名称</a>
                        </p>
                        <p class="item-price">1999元</p>
                    </div>
                    <div class="goods-item fl">
                        <div class="item-img">
                            <a href="">
                                <img src="./img/item3.png" alt="">
                            </a>
                        </div>
                        <p class="item-title">
                            <a href="">商品名称</a>
                        </p>
                        <p class="item-price">1999元</p>
                    </div>
                    <div class="goods-item fl">
                        <div class="item-img">
                            <a href="">
                                <img src="./img/item4.png" alt="">
                            </a>
                        </div>
                        <p class="item-title">
                            <a href="">商品名称</a>
                        </p>
                        <p class="item-price">1999元</p>
                    </div>
                    <div class="goods-item fl">
                        <div class="item-img">
                            <a href="">
                                <img src="./img/item4.png" alt="">
                            </a>

                        </div>
                        <p class="item-title">
                            <a href="">商品名称</a>
                        </p>
                        <p class="item-price">1999元</p>
                    </div>
                    <div class="goods-item fl">
                        <div class="item-img">
                            <a href="">
                                <img src="./img/item3.png" alt="">
                            </a>

                        </div>
                        <p class="item-title">
                            <a href="">商品名称</a>
                        </p>
                        <p class="item-price">1999元</p>
                    </div>
                    <div class="goods-item fl">
                        <div class="item-img">
                            <a href="">
                                <img src="./img/item2.png" alt="">
                            </a>

                        </div>
                        <p class="item-title">
                            <a href="">商品名称</a>
                        </p>
                        <p class="item-price">1999元</p>
                    </div>
                    <div class="goods-item fl">
                        <div class="item-img">
                            <a href="">
                                <img src="./img/item1.png" alt="">
                            </a>

                        </div>
                        <p class="item-title">
                            <a href="">商品名称</a>
                        </p>
                        <p class="item-price">1999元</p>
                    </div>

                </div>
            </div>
            <!-- 热评产品 开始 -->
            <div class="goods-box clearfix">
                <h2>热评商品</h2>
                <div class="hot-goods-list">
                    <div class="hot-goods first fl">
                        <p class="hot-goods-img">
                            <a href="">
                                <img src="./img/item11.png" alt="">
                            </a>
                        </p>
                        <p class="hot-goods-msg">
                            <a>东西真心不错，用过最好用的插线板，做工外观没得挑剔，3个USB接口很实用，充电快，建议不包邮的应该在...</a>
                        </p>
                        <p class="hot-goods-author">
                            <a href="">来源于水军1号的评价</a>
                        </p>
                        <div class="hot-goods-info">
                            <h3 class="title">玉米插线板</h3>
                            <span>|</span>
                            <p class="price">49元</p>
                        </div>
                    </div>
                    <div class="hot-goods fl">
                        <p class="hot-goods-img">
                            <a href="">
                                <img src="./img/item12.png" alt="">
                            </a>
                        </p>
                        <p class="hot-goods-msg">
                            <a>东西真心不错，用过最好用的插线板，做工外观没得挑剔，3个USB接口很实用，充电快，建议不包邮的应该在...</a>
                        </p>
                        <p class="hot-goods-author">
                            <a href="">来源于水军1号的评价</a>
                        </p>
                        <div class="hot-goods-info">
                            <h3 class="title">玉米插线板</h3>
                            <span>|</span>
                            <p class="price">49元</p>
                        </div>
                    </div>
                    <div class="hot-goods fl">
                        <p class="hot-goods-img">
                            <a href="">
                                <img src="./img/item11.png" alt="">
                            </a>
                        </p>
                        <p class="hot-goods-msg">
                            <a>东西真心不错，用过最好用的插线板，做工外观没得挑剔，3个USB接口很实用，充电快，建议不包邮的应该在...</a>
                        </p>
                        <p class="hot-goods-author">
                            <a href="">来源于水军1号的评价</a>
                        </p>
                        <div class="hot-goods-info">
                            <h3 class="title">玉米插线板</h3>
                            <span>|</span>
                            <p class="price">49元</p>
                        </div>
                    </div>
                    <div class="hot-goods fl">
                        <p class="hot-goods-img">
                            <a href="">
                                <img src="./img/item12.png" alt="">
                            </a>
                        </p>
                        <p class="hot-goods-msg">
                            <a>东西真心不错，用过最好用的插线板，做工外观没得挑剔，3个USB接口很实用，充电快，建议不包邮的应该在...</a>
                        </p>
                        <p class="hot-goods-author">
                            <a href="">来源于水军1号的评价</a>
                        </p>
                        <div class="hot-goods-info">
                            <h3 class="title">玉米插线板</h3>
                            <span>|</span>
                            <p class="price">49元</p>
                        </div>
                    </div>
                </div>
            </div>
            <!-- 热评商品 结束 -->
        </div>
    </div>

    <!-- 商品展示 结束 -->

    <!-- 底部 开始 -->
    <div class="footer">
        <div class="container">
            <ul class="service-info clearfix">
                <li class="service-item first">
                    <a href="">预约维修</a>
                </li>
                <li class="service-item">
                    <a href="">7天无理由退货</a>
                </li>
                <li class="service-item">
                    <a href="">15天免费换货</a>
                </li>
                <li class="service-item">
                    <a href="">满150元包邮</a>
                </li>
                <li class="service-item">
                    <a href="">520余家售后网点</a>
                </li>
            </ul>

        </div>
        <div class="slogan">
            &copy;为发骚而生
        </div>
    </div>
    <!-- 底部 结束 -->
</body>

</html>
```
**mi.css**

```
a {
    text-decoration: none;
}

ul {
    list-style-type: none;
}

.fr {
    float: right
}

.fl {
    float: left;
}

.clearfix:before,
.clearfix:after {
    content: "";
    display: block;
    clear: both;
}

.container {
    width: 1226px;
    margin: 0 auto;
}

/* 导航条样式 */
.nav {
    background-color: #333;
    height: 40px;
    line-height: 40px;
    width: 100%;
    position: fixed;
    top: 0;
}

.nav-left,
.nav-right,
.top-menu,
.item-list,
.service-info {
    margin: 0;
    padding: 0;
}
.nav-left li {
    float: left;
    padding-right: 15px;
}

.nav-right li {
    float: left;
    padding-left: 15px;
}
.nav a {
    color: #b0b0b0;
}

.nav a:hover {
    color: #fff;
}


/* main区 样式 */
.main {
    width: 1226px;
    margin: 40px auto;
    background-color: white;
}

.logo>* {
    margin-right: 15px;
}

.main-header {
    height: 55px;
    line-height: 55px;
}

.top-menu a {
    color: #3d3d3d;
}
.top-menu a:hover {
    color: #ff6700;
}
.top-menu li {
    float: left;
    padding: 0 10px;
}

.main-search {
    width: 300px;
    height: 50px;
    margin-top: 15px;
}
.main-search input {
    border: 1px solid #e0e0e0;
    width: 80%;
}
.main-search button {
    border: 1px solid #e0e0e0;
    width: 19%;
}
.main-search input,
.main-search button {
    padding: 10px 0;
    display: block;
    float: left;
}


/* 左侧菜单和轮播carousel */
.menu {
    margin-bottom: 30px;
}

.menu > .sidebar {
    width: 234px;
    background-color: #0a3651;
}

.item-list {
    padding: 20px 0;
}

.item-list a {
    color: white;
    display: block;
    height: 42px;
    line-height: 42px;
    padding: 0 25px;
}

.item-list a:hover {
    background-color: tomato;
}

.menu > .carousel {
    width: 992px;
}


/* 目录下 二级广告区 */
.pic-item {
    margin-left: 8px;
}

.pic.first{
    margin-left: 0;
}

/* 闪购  */
.flash-item {
    margin-left: 8px;
}

.flash-item.first {
    margin-left: 0px;
}

/* 商品列表 */

.goods {
    background-color: #f5f5f5;
    padding-bottom: 60px;
}

.goods-box {
    padding-top: 20px;
}
.goods-box-left {
    width: 234px;
}
.left-item {
    display: block;
}

.left-item.first {
    margin-bottom: 10px;
}
.goods-box-right {
    width: 992px;
}
.goods-item {
    height: 246px;
    width: 234px;
    padding: 34px 0 20px;
    background-color: white;
    margin-left: 14px;
    margin-bottom: 14px;
}

.item-img {
    width: 150px;
    height: 150px;
    margin: 0 auto 18px;
}

.item-img>a {
    display: block;
}

.item-price {
    text-align: center;
    color: red;
}
.item-title {
    text-align: center;
}
.item-title>a {
    color: #3d3d3d;
}
.item-memo>a {
    display: block;
    padding: 8px 30px;
}

/* 热评商品 */
.hot-goods {
    width: 296px;
    height: 415px;
    background-color: white;
    margin-left: 14px;
}
.hot-goods.first {
    margin-left: 0px;
}
.hot-goods-img {
    margin: 0 0 28px;
}

.hot-goods-msg {
    padding: 0 28px;
    color: #333;
}

.hot-goods-author {
    height: 18px;
    margin: 0 28px 8px;
    padding: 0 10px 0 0;
    font-size: 12px;
}
.hot-goods-author>a {
    color: #b0b0b0;
}

.hot-goods-info {
    margin: 0 30px;
}
.hot-goods-info>* {
    display: inline;
}

.price {
    color: red
}

/* 页脚 */

.footer {
    background-color: white;
}
.service-info {
    padding: 27px 0;
    border-bottom: 1px solid #e0e0e0;
}

.service-item {
    float: left;
    width: 19%;
    height: 25px;
    border-left: 1px solid #e0e0e0;
    font-size: 16px;
    line-height: 25px;
    text-align: center;
}
.service-item.first {
    border-left: none;
}
.service-item>a {
    color: #616161;
}

.service-item>a:hover{
    color: #ff6700;
}

.slogan {
    padding: 5px 0;
    text-align: center;
    color: #616161;
    background-color: #f5f5f5;
}
```