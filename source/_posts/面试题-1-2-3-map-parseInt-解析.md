---
title: '面试题[1,2,3].map(parseInt)解析'
date: 2019-07-16 10:11:03
tags: ['面试题', 'Javascript']
---
## 题目
``` javascript
let a = ['1','2','3'].map(parseInt);
console.log(a);
```
## 答案
``` javascript
// [ 1, NaN, NaN ]
```
## 解析
### 代码解析
1、传入一个参数
``` javascript
var arr = ["1", "2", "3"].map(function(item){
　// 这个地方只传入一个参数
  return item;
});
console.log(arr);
// ["1", "2", "3"]
```
2、 传入两个参数
``` javascript
var arr = ["1", "2", "3"].map(function(item, index){
　// 这个地方传入两个参数
  return "value:" + item + ", index:" + index;
});
console.log(arr);
// ["value:1, index:0", "value:2, index:1", "value:3, index:2"]
```
3、map标准的语句
``` javascript
arr.map(function(item, index, arr){
  .....
});
```
即，此时原题可以变换为：
``` javascript
var arr = ["1","2","3"];
arr.map((value,index,array) => parseInt(value,index));
//parseInt(string,radix)
```
### Map、parseInt 公式分析
[Map文档](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/map)
[parseInt文档](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/parseInt)

这里只讲parseInt
#### 语法
parseInt(string, radix);
#### 参数
<font size=6>string</font>
要被解析的值。如果参数不是一个字符串，则将其转换为字符串(使用  ToString 抽象操作)。字符串开头的空白符将会被忽略。
<font size=6>radix</font>
一个介于2和36之间的整数(数学系统的基础)，表示上述字符串的基数。比如参数"10"表示使用我们通常使用的十进制数值系统。始终指定此参数可以消除阅读该代码时的困惑并且保证转换结果可预测。当未指定基数时，不同的实现会产生不同的结果，通常将值默认为10。
相信到这里你应该懂了吧～～
#### 分解 求解步骤
parseInt('1', 0); // 1 (parseInt的处理方式，这个地方item没有以"0x"或者"0X"开始，8和10这个基数由实现环境来定，ES5规定使用10来作为基数，因此这个0相当于传递了10)
parseInt('2', 1); // NaN (因为parseInt的定义，超出了radix的界限)
parseInt('3', 2); // NaN (虽然没有超出界限，但是二进制里面没有3，因此返回NaN)

