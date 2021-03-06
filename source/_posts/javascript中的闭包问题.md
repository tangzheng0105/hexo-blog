---
title: javascript中的闭包问题
date: 2017-03-09 14:14:03
tags: closesure 闭包
---


## 闭包简介

闭包是JavaScript的重要特性，那么什么是闭包？

《JavaScript高级程序设计(第3版)》中闭包的定义：

>闭包就是指有权访问另一个函数中的变量的函数。

<!-- more -->

《JavaScript权威指南(第6版)》中闭包的定义：

>函数对象可以通过作用域链相互关联起来，函数体内部的变量都可以保存在函数作用域内，这种特性在计算机科学文献中成为“闭包”。

简单来说，在JavaScript中，函数是对象，对象是属性的集合，属性的值也可以是对象，在函数内定义函数就成为一种常见的情况，在函数内部声明函数innerFunction，在函数外部调用innerFunction，在这个过程中，就产生了闭包。

我们来看一个简单的例子：
```
function checkScope(){
    var scope = "local scope";
    function f() { return scope; }
    return f();
}
checkScope();//输出为“local scope”
```
当函数第一次被调用时，会创建一个执行环境以及相应的作用域链，作用域链的前端，始终都是当前执行代码所在环境的变量对象，作用域链中的下一个变量对象来自包含外部环境，下一个变量则来自下一个外部环境，这样一直延续到全局执行环境。

在上边的例子中，访问scope时，内部的f()函数可以访问f()外部的变量scope，因为它在作用域链中一级一级往上找的时候可以找到scope变量。

## 闭包的作用

一、 模拟私有变量。在函数内创建一个闭包，闭包就可以通过自己的作用域链访问函数内部的变量，可以创建用于访问私有变量的方法。访问私有变量和私有函数的方法被称为特权方法。
```
function MyObject(){
    var privateVariable = 10;
    function privateFunction() {
        return false;
    }
    //特权方法
    this.publicFunction = function() {
        privateVariable++;
        return privateFunction();
    };
}
```
二、 模仿块级作用域。 JavaScript中没有块级作用域的概念，这意味着在块语句中定义的变量，实际上是包含在函数中的。如果临时需要一些变量，使用私有作用域。
```
function block() {
    var a = 1;
    var b = 2;
    (function () {
        var a = 3;//覆盖了父作用域中的变量a
        var c = 4;
        //访问到了当前作用域中的变量
        console.log(a);//3
        //访问了父作用域中的变量
        console.log(b);//2
        //访问当前作用域中的变量
        console.log(c);//4
    })()
    //访问块级作用域中的变量
    console.log(c);//c is not defined
}
```
这种技术经常在全局作用域中被用在函数外部，从而限制向全局作用域中添加过多的变量和函数。

## 循环中的闭包

使用闭包时一种常见的错误情况是循环中的闭包，很多初学者都遇到了这个问题。很常见的一种情况就是给页面中的多个按钮绑定点击事件，JavaScript代码如下所示：
```
window.onload = function(){
    var inupts = document.getElementsByTagName('input');
    for(var i = 0; i < inupts.length; i++){
        inupts[i].onclick = function(){
            console.log(i);//希望输出0,1,2,3,4...
        }
    }
}
```
页面中有5个按钮，根据上边的代码，我们需要的是依次点击按钮时，控制台分别打印出0，1，2，3，4，而实际上，控制台打印出来的，如下图所示：


这是为什么呢，当for循环执行完之后，i已经变成了按钮的个数5了，而所有点击函数绑定的都是同一个i，点击按钮时，打印出来的i也都变成了5了。

这一部分理解也可以参考http://www.cnblogs.com/qieguo/p/5457040.html。

那么我们为了得到想要的结果，需要在每次循环中创建变量i的拷贝，下面提供三种方法。

第一、使用匿名包装器
```
window.onload = function () {
    var inupts = document.getElementsByTagName('input');
    for (var i = 0; i < inupts.length; i++) {
        (function (e) {
            inupts[i].onclick = function () {
                console.log(e);
            }
        })(i);

    }
}
```
依次点击按钮，控制台输出如下：
![](https://pic1.zhimg.com/v2-aca633054a266adb5ebe5adf4adc6578_b.png)

第二、从匿名包装器中返回一个函数：
```
window.onload = function () {
    var inupts = document.getElementsByTagName('input');
    for (var i = 0; i < inupts.length; i++) {
        inupts[i].onclick = function (num) {
            return function () {
                console.log(num);
            };
        } (i);
    }
}
```
首先，定义了匿名函数，并将立即执行该匿名函数的结果赋值给数组，匿名函数有一个参数num，在调用每个函数时，我们传入了变量i，函数按值传递，就将变量i的当前值复制给参数num。而在这个匿名函数内部，又创建并返回了一个访问num的闭包，这样一来，每个按钮点击函数都有自己num变量的一个副本，因此可以输出各自不同的数值了。

第三、在循环中使用let
```
window.onload = function () {
    var inupts = document.getElementsByTagName('input');
    for (let i = 0; i < inupts.length; i++) {
        inupts[i].onclick = function () {
            console.log(i);
        };
    }
}
```
let是ES6新增的命令，用法类似于var，但是所声明的变量只能在let命令所在代码块内有效。上述代码中，变量i是let声明的，当前的i只在本轮循环有效。所以每一次循环的i其实都是一个新的变量。关于let的用法可参考《ECMAScript 6 入门》中第二章。

## 内存泄漏

产生内存泄漏的原因是IE9之前的版本对JScript对象和COM对象使用不同的垃圾收集例程，因此闭包在IE的这些版本中会导致一些问题。(JavaScript垃圾收集机制可参考《JavaScript高级程序设计(第3版)》4.3) 例如：
```
function assignHandle() {
    var element = document.getElementById('elementId');
    element.onclick = function () {
        //element的onclick引用了匿名函数，
        //匿名函数又通过闭包引用了element，造成了循环引用
        console.log(element.id);
    };
}
```
这个例子中，循环引用导致了element引用数至少为1，element所占内存就永远不会被回收，从而导致了内存泄漏问题。要解决这个问题，就需要解除对DOM对象的引用，减少引用数，确保正常回收其所占用的内存。

引用计数的含义是跟踪记录每个值被引用的次数。当声明了一个变量并将一个引用类型值赋给该变量时，则这个值的引用次数就是1。如果同一个值又被赋给另一个变量，则该值的引用次数加1。相反，如果包含对这个值引用的变量又取得了另外一个值，则这个值的引用次数减1。当这个值的引用次数变成0时，则说明没有办法再次访问这个值了，因而就可以将其占用的内存空间回收回来。

在采用引用计数策略的实现中，出现循环引用时，由于变量的引用次数永远不会是0，函数被多次调用时，就会导致大量内存得不到回收。 这一部分理解可以参考MDN中JavaScript内存管理。

## 结语

JavaScript闭包是极其有用的特性，但是由于闭包会携带包含它的函数的作用域，占用更多内存，过多使用闭包可能会导致内存占用过多。