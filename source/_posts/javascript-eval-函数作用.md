---
title: javascript eval()函数作用
date: 2019-03-14 21:19:38
tags: ['JavaScript']
---
## 定义和用法
eval() 函数可计算某个字符串，并执行其中的的 JavaScript 代码。
### 语法
eval(string)

### eval函数是强大的数码转换引擎,字符串经eval转换后得到一个javascript对象
### 说明
该方法只接受原始字符串作为参数，如果 string 参数不是原始字符串，那么该方法将不作任何改变地返回。因此请不要为 eval() 函数传递 String 对象来作为参数。

如果试图覆盖 eval 属性或把 eval() 方法赋予另一个属性，并通过该属性调用它，则 ECMAScript 实现允许抛出一个 EvalError 异常。
### 例子
var temp = eval(“3″);等效于var temp = 3;
var temp = eval(“’3′”);等效于var temp = ’3′;
eval("x=10;y=20;document.write(x*y)")
document.write(eval("2+2"))
var x=10
document.write(eval(x+17))
### 实践
假设后端返回一个字段，data = '["image1.png", "image2.png", "image3.png"]'; 你怎么拿到里面的图片遍历出来呢🌚
还想着字符串截取？这样？
![](https://github.com/chengheai/review-demo-image/blob/master/WX20190314-213311@2x.png?raw=true)
用eval() 一套搞定
![](https://github.com/chengheai/review-demo-image/blob/master/WX20190314-213559@2x.png?raw=true)

## 提示和注释
<font color=#00ffff size=4 face=" 华文行楷">提示：虽然 eval() 的功能非常强大，但在实际使用中用到它的情况并不多。</font>
<table><tr><td bgcolor=#D1EEEE><font color=#EC6E3A size=4 face=" 华文行楷">提示：虽然 eval() 的功能非常强大，但在实际使用中用到它的情况并不多。</font></td></tr></table>


