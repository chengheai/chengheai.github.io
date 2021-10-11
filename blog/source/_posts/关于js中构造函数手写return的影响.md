---
title: 关于js中构造函数手写return的影响
date: 2019-07-15 20:44:55
tags: ['JavaScript']
---
## 例子
### 基础例子
构造函数正常的应该是这样的格式，new关键字调用函数，函数名首字母大写。
``` javascript
// 一个类
function Fun(name, age, sex) {
  this.name = name;
  this.age = age;
  this.sex = sex;
}
// 实例
var obj = new Fun('alex', 12, 'male');
console.log(obj);
```
### 不用new调用的构造函数this指向window
1、
``` javascript
function Fun(name, age, sex) {
  this.name = name;
  this.age = age;
  this.sex = sex;
}
// 不用new调用，函数直接调用，this window
Fun('alex', 12, 'male');
console.log(age) // 12  window
```
2、
``` javascript
function People(name, age){
  this.name = name;
  this.age = age;
}
var obj = People('小米', 12);
console.log(obj == null) // true
console.log(obj.age) // 报错 
// 没有用new调用 不是构造函数，没有返回值
```
### 构造函数末尾有return语句
``` javascript
function fun(name, age) {
  this.name = name;
  this.age = age;
  return 3;
}
var xiaoming = new fun('小明', 12);
console.log(xiaoming) // 忽略return 3；
```
### 构造函数在句中有return语句[基本类型值]
``` javascript
function fun(name, age) {
  this.name = name;
  return 3;
  this.age = age;
}
// 基本类型值，就会被忽视； 
// NaN undefined '' true, false
var xiaoming = new fun('小明', 12);
console.log(xiaoming) // 打断程序执行，age将不会执行
```
### 构造函数语句中return [引用类型值]
``` javascript
// 返回 {} []  Math /\d/
// 引用类型值
function fun(name, age) {
  this.name = name;
  return 　{
    a: 1,
    b: 2
  };
  this.age = age;
}
// 基本类型值，就会被忽视；
var xiaoming = new fun('小明', 12);
console.log(xiaoming) // 返回函数自己写的return里的值
```
### 工厂类型
``` javascript
function createPeople(name, age, sex) {
  var obj = {};
  obj.name = name;
  obj.age = age;
  obj.sex = sex;
  return obj;
}
// 不写new也可以的，名称为工厂模式

var xiaoming = createPeople('小明', 12, '男');
console.log(xiaoming) // { name: '小明', age: 12, sex: '男' }
console.log(xiaoming instanceof createPeople) // false
```
## 总结
### 构造函数运行
用new运算符调用一个函数的时候，分为4个步骤
1，函数内部自己创建一个局部变量吗是一个控对象{}
2,函数将自己的上下文设置为这个{}, 即所有语句中的this都指向这个控对象
3，函数执行所有语句
4，所有语句执行后，函数将return这个对象，函数将把自己的上下文返回
<table><tr><td bgcolor=yellow>上面的构造函数就是典型的四步走</td></tr></table>

### 含有return语句
构造函数不用谢return语句，自动return
如果写了一个return ：
如果return这个基本类型值，无视这个return值，该return什么还是return什么
但是return阻止了构造函数的执行
