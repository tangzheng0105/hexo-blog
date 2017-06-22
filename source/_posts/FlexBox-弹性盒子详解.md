---
title: FlexBox 弹性盒子详解
date: 2017-02-23 16:56:38
tags: FlexBox
---

flex布局模型不同于块和内联模型布局，块和内联模型的布局计算依赖于块和内联的流方向，而flex布局依赖于flex directions.**简单的说：Flexbox是布局模块，而不是一个简单的属性，它包含父元素(flex container)和子元素(flex items)的属性。**

<!-- more -->

- 主轴、主轴方向(main axis |main dimension)：用户代理沿着一个伸缩容器的主轴配置伸缩项目，主轴是主轴方向的延伸。 
- 主轴起点、主轴终点(main-start |main-end)：伸缩项目的配置从容器的主轴起点边开始，往主轴终点边结束。 
- 主轴长度、主轴长度属性(main size |main size property)：伸缩项目的在主轴方向的宽度或高度就是项目的主轴长度，伸缩项目的主轴长度属性是width或height属性，由哪一个对着主轴方向决定。 
- 侧轴、侧轴方向(cross axis |cross dimension)：与主轴垂直的轴称作侧轴，是侧轴方向的延伸。 
- 侧轴起点、侧轴终点(cross-start |cross-end)：填满项目的伸缩行的配置从容器的侧轴起点边开始，往侧轴终点边结束。
-  侧轴长度、侧轴长度属性(cross size |cross size property)：伸缩项目的在侧轴方向的宽度或高度就是项目的侧轴长度，伸缩项目的侧轴长度属性是"width"或"height"属性，由哪一个对着侧轴方向决定。

![](https://segmentfault.com/image?src=http://img.blog.csdn.net/20150614222502311&objectId=1190000002910324&token=fa57b0c157bd4d74243425778f0b707e)

# Flexbox使用示例 #

## 水平竖直居中 ##

下面这个例子是我们用的很多的完全居中（上下左右居中），我们可以用很多种方法实现，但目前只有用Flexbox实现是最为简单的。

```html
    <!DOCTYPE html>
    <html lang="en">
    <head>
    <meta charset="UTF-8">
    <title>center</title>
    <link rel="stylesheet" href="./center.css">
    </head>
    <body>
    <div class="parent"><div class="child"></div></div>
    </body>
    </html>
```

```CSS
	body{
	    padding: 0;
	    margin: 0;
	}	

	.parent {
	  display: flex;
	  height: 300px; /* Or whatever */
	  background-color: black;
	}	

	.child {
	  width: 100px;  /* Or whatever */
	  height: 100px; /* Or whatever */
	  margin: auto;  /* Magic! */
	  background-color: white;
	}
```

在Flex容器中，当项目边距设置为“auto”时，设置自动的垂直边距将使该项目完全集中两个轴。

## 六个子元素布局 ##

再看一个例子，将子元素的数量增加到六个。六个子元素随着浏览器大小改变布局而不需要用媒体查询。

```html
	<!DOCTYPE html>
	<html lang="en">
	<head>
	    <meta charset="UTF-8">
	    <title>six</title>
	    <link rel="stylesheet" href="./six.css">
	</head>
	<body>
	    <ul class="flex-container">
	        <li class="flex-item">1</li>
	        <li class="flex-item">2</li>
	        <li class="flex-item">3</li>
	        <li class="flex-item">4</li>
	        <li class="flex-item">5</li>
	        <li class="flex-item">6</li>
	    </ul>
	</body>
	</html>

```

```CSS
	body{
	    margin: 0;
	    padding: 0;
	}
	
	ul {
	    margin: 0;
	    padding: 0;
	
	}
	
	li{
	    list-style: none;
	}
	
	.flex-container {
	    /* We first create a flex layout context */
	    display: flex;
	
	    /* Then we define the flow direction and if we allow the items to wrap 
	    * Remember this is the same as:
	    * flex-direction: row;
	    * flex-wrap: wrap;
	    */
	    flex-flow: row wrap;
	    /* Then we define how is distributed the remaining space */
	    justify-content: space-around;
	}
	.flex-item {
	    background: tomato;
	    padding: 5px;
	    width: 200px;
	    height: 150px;
	    margin-top: 10px;
	
	    line-height: 150px;
	    color: white;
	    font-weight: bold;
	    font-size: 3em;
	    text-align: center;
	}

```

![](https://segmentfault.com/image?src=http://img.blog.csdn.net/20150616133212898&objectId=1190000002910324&token=75681601fadb05ec6c57f7d0af4a7bef)

实现图中效果，需要设置三个属性：flex-direction: row; flex-wrap: wrap; justify-content: space-around;下节将介绍各属性。

