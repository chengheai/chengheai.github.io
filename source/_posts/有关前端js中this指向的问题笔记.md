---
title: 有关前端js中this指向的问题笔记
date: 2019-07-08 19:52:05
tags: ['JavaScript', 'this']
---
## 前言
在以往的ECMAScript 5中，this的指向可谓是多种多样，在各个公司面试题也是必考项，现收集几个面试题进行解析：
## 原题解析
### 题1:
``` javascript
var o = {
  fn: function(){
    var a = 1;
    console.log(this.a);
  }
}
o.a = 2;
o.fn.a = 3;
o.fn();
```
*分析:
``` javascript
o.a = 2; // 赋值, 即给对象o添加一个a属性，值为2
o.fn.a = 3; // 赋值, fn函数添加属性 a 为 3
o.fn(); // 对象调用，this是函数对象o里面

```
答案：2
### 题2:

``` javascript
var o = {
function fun(){
  var a = 1;
  this.a = 2;
  function fn(){
    return this.a;
  }
  fn.a = 3;
  return fn;
}
var result = fun()();
console.log(result)
```
*分析:
``` javascript
  var a = 1; // 全局变量
  this.a = 2; // 全局添加属性
  var result = fun()(); // 函数直接调用，this window对象

```
答案：2
### 题3:

``` javascript
var a = 1;
var obj = {
  a: 2,
  fn: (function(){
    this.a = 3;
    return function(){
      return this.a;
    }
  })()
}
var fn = obj.fn;
var result1 = obj.fn();
var result2 = fn();
console.log('result1: ', result1);
console.log('result2: ', result2);
```
*分析:
``` javascript
  补充知识点：
  // 自运行函数是先执行的；比如
  var o ={
    a: 1+1  // 先执行 1 + 1；
  }
  this.a = 3; // 全局变量
  var result1 = obj.fn(); // obj对象调用，指向对象内部 a值
  var result2 = fn(); // global调用a, 值为3
```
答案：2, 3
### 题4:

``` javascript
var arr = [fn1,fn2];
function fn1(){
  return this.length;
}
function fn2(){
  return this[0];
}
var a = arr[0]();
var b = arr[1]()();
console.log('a: ', a);
console.log('b: ', b);
```
*分析:
``` javascript
  // 枚举
  return this.length; // this是数组
  var b = arr[1]()(); // global调用
   // 在浏览器中与在弄的环境中运行结果都不一样
  //  node:
  //  a: 2;
  //  b: undefined;
```
答案：2, undefined;
### 题5:

``` javascript
function f(){
  console.log(this.a)
};
f.a = 100;
怎么使函数中打印100？
```
*分析:
``` javascript
  // 方法一 call 方法
  f.call(f);
  // 方法二  arguments.callee
  原函数更改为：
  function f(){
    console.log(arguments.callee.a)
  }
  f.a = 100;
  f();
```
答案：f.call(f), console.log(arguments.callee.a);