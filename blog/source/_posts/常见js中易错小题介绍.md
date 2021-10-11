---
title: 常见js中易错小题介绍
date: 2019-07-16 13:31:49
tags: ['面试题', 'JavaScript']
---
## 前说
在前端面试过程中，有时候会因为一些小题而使你与offer失之交臂，这也反应出你的基础知识点不扎实，或做事粗心大意，很多公司也喜欢出这样的题目来考察面试者的基本水平。下面总结了一些典型的面试易错题。
## 典题
### 1、
下面哪个是错误的？

  A : li:nth-of-child(1)
  B : li:nth-of-type(1)
  C : li:nth-last-child(1)
  D : li:nth-last-of-type(1)
答案：A
解析：只有nth-child() 没有nth-of-child()
### 2、
关于IE的window对象表述正确的有：()
  A. window.opener属性本身就是指向window
  B. window.reload()方法可以用来刷新当前页面
  C. window.location = 'a.html' 和 window.location.href = 'a.html'的作用都是把当前页面替换成a.html页面
  D. 定义了全局变量g， 可以通过window.g来取得该变量

 答案：ACD
 解析：window不存在reload()方法， 只有location.reload()
 ### 3、
 运行结果是什么？
``` javascript
var a = 'HelloWorld'
console.log(a.split('').sort().join(''));
```
答案：HWdellloor
解析：sort根据Unicode排序
资料： [ sort ](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/sort) [ Unicode ](https://baike.baidu.com/item/Unicode/750500?fr=aladdin) [ ASCII ](https://baike.baidu.com/item/ASCII/309296?fr=aladdin) [ Unicode与ASCII的区别 ](http://m.elecfans.com/article/601592.html)



