---
title: 深入理解js中防抖跟截流的区别
date: 2021-07-15 21:29:55
tags: [Javascript]
---
## 前言
在日常项目开发中，通常会碰到监听处理的操作，比如监听浏览器窗口大小改变，监听滚动事件，监听输入事件等等，然而做这方面功能的时候就要想着怎么优化性能，防抖跟截流就派上用场了
## 防抖
> 触发高频事件后delay内函数只会执行一次，如果delay内高频事件再次被触发，则重新计算时间

1. 实现方式：每次触发事件时设置一个延迟调用方法，并且取消之前的延时调用方法
2. 缺点：如果事件在规定的时间间隔内被不断的触发，则调用方法会被不断的延迟

``` javascript
//debounce
function debounce(method, delay) {
  let timer = null
  return function () {
    let self = this
    if (timer) {
      clearTimeout(timer)
      timer = null
    }
    timer = setTimeout(function () {
      method.apply(self, arguments)
    }, delay)
  }
}

// 处理函数
function handle() {
    console.log(xxx);
}
// 滚动事件
window.addEventListener('scroll', debounce(handle,1000));
```
# 节流
> 指在规定时间内只触发一次事件，减少事件执行的频率

1. 实现方式：每次触发事件时，如果当前有等待执行的延时函数，则直接return

``` javascript
function throttle(fn,delay) {
  let canRun = true
  return function () {
    let self = this
    if (!canRun) return
    canRun = false
    setTimeout(() => {
      fn.apply(self, arguments)
      canRun = true
    }, delay)
  }
}

```

# 其他版本

``` javascript
// 时间戳
function throttle1 (fn, delay) {
  let prev = Date.now()
  return () => {
    let now = Date.now()
    if (now - prev >= delay) {
      fn()
      prev = Date.now()
    }
  }
}

// 定时器
function throttle2 (fn, delay) {
  let timer = null
  return () => {
    if (!timer) {
      timer = setTimeout(() => {
        fn()
        timer = null
      }, delay)
    }
  }
}
```
# demo
[throttle 截流](https://chengheai.github.io/daily-vue-demo/#/throttle)
[debounce 防抖](https://chengheai.github.io/daily-vue-demo/#/debounce)
# 总结
一般情况下onresize，onkeyup事件使用防抖；onscroll、onmousemove等事件使用截流。