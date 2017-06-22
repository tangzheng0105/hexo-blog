---
title: 一个FlexBox小任务
date: 2017-02-23 21:48:27
tags: FlexBox
---

## 任务目的 ##

学习如何flex进行布局，学习如何根据屏幕宽度调整布局策略。
任务描述

需要实现的效果如效果图（点击打开）所示，调整浏览器宽度查看响应式效果，红色的文字是说明，不需要写在 HTML 中。

<!-- more -->

## 任务注意事项 ##

只需要完成HTML，CSS代码编写，不需要写JavaScript
屏幕宽度小于 640px 时，调整 Flexbox 的属性以实现第四个元素移动到最前面的效果，而不要改动第一个元素的边框颜色与高度实现效果图。
思考 Flexbox 布局和网格布局的异同，以及分别适用于什么样的场景。可以搜索一下别人的结论，不过要保持思辨的态度，不可直接接受别人的观点。
HTML 及 CSS 代码结构清晰、规范

## 任务协作建议 ##

团队集中讨论，明确题目要求，保证队伍各自对题目要求认知一致
各自完成任务实践
交叉互相Review其他人的代码，建议每个人至少
看一个同组队友的代码
相互讨论，最后合成一份组内最佳代码进行提交

## 在线学习参考资料 ##

[Flexbox详解：了解 Flexbox 属性的含义](https://segmentfault.com/a/1190000002910324)

[图解 CSS3 Flexbox](https://web.tutorialonfree.com/tu-jie-css3-flexboxshu-xing/) 属性：看完这两篇，对 Flexbox 的了解就够了，多实践一下，理解会更深刻

[Flexbox——快速布局神器](http://www.w3cplus.com/css3/flexbox-basics.html)

[使用 CSS 弹性盒](https://developer.mozilla.org/zh-CN/docs/Web/CSS/CSS_Flexible_Box_Layout/Using_CSS_flexible_boxes)

![](http://7xrp04.com1.z0.glb.clouddn.com/task_1_10_1.png)

```htlm
	<!DOCTYPE html>
	<html lang="en">
	<head>
		<meta charset="UTF-8">
		<title>t1_10</title>
		<link rel="stylesheet" type="text/css" href="css/t1_10.css">
	</head>
	<body>
		<ul class="wrap">
			<li></li>
			<li></li>
			<li></li>
			<li></li>
		</ul>
	</body>
	</html>
```

```CSS
	@media screen and (min-width: 640px) {
		.wrap {
		/* We first create a flex layout context */
	    display: flex;
	    /* Then we define the flow direction and if we allow the items to wrap 
	    * Remember this is the same as:
	    * flex-direction: row;
	    * flex-wrap: wrap;
	    */
	    flex-flow: row nowrap;
	    /* Then we define how is distributed the remaining space */
	    justify-content: space-around;
	    align-items: center;
		}
	}
	
	@media screen and (max-width: 639px) {
		.wrap {
		/* We first create a flex layout context */
	    display: flex;
	    /* Then we define the flow direction and if we allow the items to wrap 
	    * Remember this is the same as:
	    * flex-direction: row;
	    * flex-wrap: wrap;
	    */
	    flex-flow: row wrap;
	    /* Then we define how is distributed the remaining space */
	    justify-content: flex-start;
	    align-items: flex-start;
		}
	
		ul li:last-child {
			order: -1;
		}
	
	}
	
	
	.wrap {
		margin: 20px;
		list-style-type: none;
	}
	
	li:nth-child(1) {
		border: 1px solid #f00;
		width: 150px;
		height: 120px;
	}
	
	li:nth-child(2) {
		border: 1px solid #f00;
		width: 150px;
		height: 100px;
	}
	
	li:nth-child(3) {
		border: 1px solid #f00;
		width: 150px;
		height: 40px;
	}
	
	li:nth-child(4) {
		border: 1px solid #0f0;
		width: 150px;
		height: 200px;
	}
```