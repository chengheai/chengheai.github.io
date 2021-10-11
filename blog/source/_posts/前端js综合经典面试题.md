---
title: 前端js综合经典面试题
date: 2019-07-15 21:09:18
tags: ['面试题']
---
## 前记
前些时间，面试中写的笔试题，偶然的机会做了两次，最后还是错了几道，结果回来在网上一挡，哎。。不过说实话，该公司面试题道道都是经典题，的确不错，长知识了。
## 题目
``` javascript
function Foo() {
	getName = function() {console.log(1);};
	return this;
}
Foo.getName = function() {console.log(2);};
Foo.prototype.getName = function() {console.log(3);};
var getName = function() {console.log(4);};
function getName() {console.log(5);}

//以下输出值为多少？
Foo.getName();
getName();
Foo().getName();
getName();
new Foo.getName();
new Foo().getName();
new new Foo().getName();
```
## 答案
2 4 1 1 2 3 3
## 分析
### 考察知识点分析
<table><tr><td bgcolor=yellow><font color=Blue>逻辑运算符、运算符的优先级、声明变量和声明函数的提升优先级、原型继承、闭包</font></td></tr></table>

### 步骤分析
#### 1.函数和变量提升
``` javascript
Foo.getName();  // 2
getName();      // 4
```
##### 第一问 Foo.getName();
第5行代码 Foo创建了一个叫getName的静态属性存储了一个匿名函数， 则第一问的答案就是Foo的静态函数返回 。 2

##### 第二问 getName();
 var getName 与 function getName 都是声明语句，区别在于 var getName 是函数表达式，而 function getName 是函数声明。函数声明会提前
 小例子
 ``` javascript
  console.log(a);//输出：function a(){}
  var a=1;
  function a(){}
 ```
原题中代码最终执行时的是
``` javascript
function Foo() {
 getName = function () { console.log(1); };
 return this;
}
var getName;//只提升变量声明
function getName() { console.log(5);}//提升函数声明，覆盖var的声明
 
Foo.getName = function () { console.log(2);};
Foo.prototype.getName = function () { console.log(3);};
getName = function () { console.log(4);};//最终的赋值再次覆盖function getName声明
getName();//最终输出4

```
#### 函数和作用域
##### 第三问 Foo().getName() 与第四问 getName();
Foo().getName(); 先执行了Foo函数，然后调用Foo函数的返回值对象的getName属性函数

1、Foo的函数体内，getName是没有用var声明的，所以getName是一个全局变量。

2、Foo()的意思是运行Foo的函数体，这个函数体中做的事情就是，把全局变量getName的函数体替换成console.log(1)。
3、Foo函数体的返回值是this，这里的this指向的是window对象。Foo().getName();就相当于window.getName();，也就是直接调用getName()函数，显示结果就是1。
4、此时的window.getName(), getName(), Foo().getName()是等价的，如不熟悉this指向问题，[可点击](https://chengheai.github.io/2019/07/08/%E6%9C%89%E5%85%B3%E5%89%8D%E7%AB%AFjs%E4%B8%ADthis%E6%8C%87%E5%90%91%E7%9A%84%E9%97%AE%E9%A2%98%E7%AC%94%E8%AE%B0/)
#### 运算符和优先级
[优先级汇总表](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/Operator_Precedence)

从以上的运算符优先级资料中可以看出，"."运算符的优先级是比"new"运算符的优先级高的。
##### 第五问 new Foo.getName()
分析
1、相当于new (Foo.getName)();
2、执行Foo.getName函数，这时候就已经输出2了
3、实例化一个对象

所以实际上将getName函数作为了构造函数来执行，故打印2。
##### 第六问 new Foo().getName()
1、相当于(new Foo()).getName();
.运算符的优先级是19，就是指.会优于其他运算符之前执行，但是执行点的时候，是按照点前和点后的表达式依次执行的，因为Foo后面有个括号，而圆括号的优先级是20，所以new运算符是归Foo()这个函数的。
2、函数中返回的是this，而this在构造函数中本来就代表当前实例化对象，遂最终Foo函数返回实例化对象。
之后调用实例化对象的getName函数，因为在Foo构造函数中没有为实例化对象添加任何属性，遂到当前对象的原型对象（prototype）中寻找getName，找到了。[构造函数中自定书写return问题](https://chengheai.github.io/2019/07/15/%E5%85%B3%E4%BA%8Ejs%E4%B8%AD%E6%9E%84%E9%80%A0%E5%87%BD%E6%95%B0%E6%89%8B%E5%86%99return%E7%9A%84%E5%BD%B1%E5%93%8D/)
3、执行new Foo()，实例化一个对象，这个对象中拥有Foo.prototype.getName方法。
4、执行.getName()，调用Foo.prototype.getName方法，输出为3。
##### 第七问 new new Foo().getName()

最终运行代码 new ((new Foo()).getName)();
先初始化Foo的实例化对象，然后将其原型上的getName函数作为构造函数再次new
1、.运算符的优先级是19，就是指.会优于其他运算符之前执行。执行点的时候，是按照点前和点后的表达式依次执行的。
看看左边的new new Foo()，带参数的new优先级是比不带参数的new优先级高的，所以左边的部分就相当于new (new Foo())
什么是带参数的new：new Foo()
什么是不带参数的new：new Foo
2、执行(new Foo()).getName()，相当于foo.getName()。
foo是new Foo()出来后的实例，这个结果和题目2是一样的。这时候已经输出3了。
3、执行new，实例化一个对象


