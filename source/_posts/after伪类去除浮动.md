---
title: '清除浮动的常用方法'
date: 2017-02-23 15:11:11
tags: CSS
---

## 清楚浮动的常用方法

1. 额外标签法
2. 使用:after 伪元素
3. 给父元素定高
4. 利用overflow:hidden;属性
5. 父元素浮动
6. 父元素处于绝对定位

<!-- more -->

在开发网页的时候经常需要用到各种浮动，此时便需要及时的清除浮动，否则将会导致布局出现问题

引出问题：

```html
	<!DOCTYPE html>
	<html lang="en">
	<head>
	    <meta charset="UTF-8">
	    <title></title>
	    <style>
	        .outer{
	            border: 1px solid black;
	            width:300px;
	        }
	        .inner{
	            width: 50px;
	            height: 50px;
	            background-color: #ff4400;
	            margin-right: 20px;
	            float: left;
	        }
	        .footer{
	            background-color: #005FC3;
	            width: 200px;
	            height: 100px;
	        }
	    </style>
	</head>
	<body>
	    <div class="outer">
	        <div class="inner"></div>
	        <div class="inner"></div>
	        <div class="inner"></div>
	    </div>
	    <div class="footer"></div>
	</body>
	</html>
```

![](https://sfault-image.b0.upaiyun.com/226/034/2260342482-5635c8e886237)

可以看出本应包住3个 inner DIV的 outer DIV 却没有包住他们，此刻只剩一条由上下边框贴合组成的线，同时 footer DIV元素也跑到了三个浮动元素的底下

## 解决办法：

### 1.使用额外标签法

```html
	<!DOCTYPE html>
	<html lang="en">
	<head>
	    <meta charset="UTF-8">
	    <title></title>
	    <style>
	        .outer{
	            border: 1px solid black;
	            width: 300px;
	        }
	        .inner{
	            width: 50px;
	            height: 50px;
	            background-color: #ff4400;
	            margin-right: 20px;
	            float: left;
	        }
	        .footer{
	            background-color: #005FC3;
	            width: 200px;
	            height: 100px;
	        }
	        .clearfix{
	            clear: both;
	        }
	    </style>
	</head>
	<body>
	    <div class="outer">
	        <div class="inner"></div>
	        <div class="inner"></div>
	        <div class="inner"></div>
	        <div class="clearfix"></div>
	    </div>
	    <div class="footer"></div>
	</body>
	</html>
```

![](https://segmentfault.com/img/bVqGlY)

增加了一个class=clearfix的div标签

### 2.使用:after伪元素

```html	
	<!DOCTYPE html>
	<html lang="en">
	<head>
    <meta charset="UTF-8">
    <title></title>
    <style>
        .outer{
            border: 1px solid black;
            width: 300px;
        }
        .inner{
            width: 50px;
            height: 50px;
            background-color: #ff4400;
            margin-right: 20px;
            float: left;
        }
        .footer{
            background-color: #005FC3;
            width: 200px;
            height: 100px;
        }
        .clearfix:after{  /*最简方式*/
            content: '';
            display: block;
            clear: both;
        }
        /* 新浪使用方式
        .clearfix:after{ 
            content: '';
            display: block;
            clear: both;
            height: 0;
            visibility: hidden;
        }
        */
        .clearfix{ /*兼容 IE*/
            zoom: 1;
        }
    </style>
	</head>
	<body>
	    <div class="outer clearfix">
	        <div class="inner"></div>
	        <div class="inner"></div>
	        <div class="inner"></div>
	    </div>
	    <div class="footer"></div>
	</body>
	</html>
```

![](https://segmentfault.com/img/bVqGlY)
