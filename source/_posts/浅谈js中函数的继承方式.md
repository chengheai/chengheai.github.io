---
title: 浅谈js中函数的继承方式
date: 2019-07-09 15:17:09
tags: ['JavaScript']
---
![]('https://github.com/chengheai/review-demo-image/blob/master/u=1418646752,692709038&fm=173&app=49&f=JPEG.jpeg?raw=true')
<center><font face="黑体" color=red size=4>～～ 回首往昔， 更近一步 ～～</font></center>

## 招式
### 1. ES5 构造函数继承
``` javascript
function Parent() {
  this.name = 'parent';
  this.colors = ['black', 'yellow', 'red']
}
function Child() {
  Parent.call(this);
  this.type = 'child';
}
var q1 = new Child();
console.log(q1.name); // parent
console.log(q1.colors); // [ 'black', 'yellow', 'red' ]
```
再看
``` javascript
function Parent() {
  this.name = 'parent';
  this.colors = ['black', 'yellow', 'red']
}
function Child() {
  Parent.call(this);
  this.type = 'child';
}
Parent.prototype.age = 12;
Parent.prototype.say = function(){
  console.log('hello');
}
var q1 = new Child();
console.log(q1);
console.log(q1.age); // undefined
console.log(q1.say()); // 报错
```
总结：Child无法继承Parent的原型对象，并没有真正的实现继承（部分继承）

### 2. ES5 原型链继承
``` javascript
function Parent() {
  this.name = 'parent';
  this.colors = ['black', 'yellow', 'red']
}
function Child() {
  this.type = 'child';
}
Parent.prototype.age = 12;
Parent.prototype.say = function(){
  console.log('hello');
}
Child.prototype = new Parent();
var q1 = new Child();
// console.log(q1);
console.log(q1.age); // 12
console.log(q1.say()); // hello
```
再看
``` javascript
function Parent() {
  this.name = 'parent';
  this.colors = ['black', 'yellow', 'red']
}
function Child() {
  this.type = 'child';
}
Parent.prototype.age = 12;
Parent.prototype.say = function(){
  console.log('hello');
}
Child.prototype = new Parent();
var q1 = new Child();
var q2 = new Child();
q1.colors.push('pink');
console.log(q1.colors) // [ 'black', 'yellow', 'red', 'pink' ]
console.log(q2.colors) // [ 'black', 'yellow', 'red', 'pink' ]
```
结论：被new出来的实例不能隔离
### 3. 组合继承(构造函数和原型链继承)
``` javascript
function Parent() {
  this.name = 'parent';
  this.colors = ['black', 'yellow', 'red']
}
function Child() {
  Parent.call(this);
  this.type = 'child';
}
Parent.prototype.age = 12;
Parent.prototype.say = function(){
  console.log('hello');
}
Child.prototype = new Parent();
var q1 = new Child();
var q2 = new Child();
q1.colors.push('pink');
console.log(q1.colors) // [ 'black', 'yellow', 'red','pink']
console.log(q2.colors) // [ 'black', 'yellow', 'red', ]

```
结论：父类的构造函数被执行了两次，第一次是Child.prototype = new Parent()，第二次是在实例化的时候，这是没有必要的。

优化
``` javascript
function Parent() {
  this.name = 'parent';
  this.colors = ['black', 'yellow', 'red']
}
function Child() {
  Parent.call(this);
  this.type = 'child';
}
Parent.prototype.age = 12;
Parent.prototype.say = function(){
  console.log('hello');
}
Child.prototype = Object.create(Parent.prototype);
Child.prototype.constructor = Child;
var q1 = new Child();
var q2 = new Child();
q1.colors.push('pink');
console.log(q1.colors) // [ 'black', 'yellow', 'red', 'pink' ]
console.log(q2.colors) // [ 'black', 'yellow', 'red', ]
```
结论：Object.create是一种创建对象的方式，它会创建一个中间对象
### 4. ES6 class继承
``` javascript
// es6的继承
class Parent {
  constructor(name) {
    this.name = name;
  }
  say() {
    console.log('parent has say method. ' + this.name);
  }
}

class Child extends Parent {
  constructor(name) {
    super(name);
  }

  fly() {
    console.log('chid can fly...');
  }
}
var p = new Child('tom');
p.say();
p.fly();

```
结论：文档，谢谢～ https://es6.ruanyifeng.com/?search=extend&x=0&y=0#docs/class-extends




