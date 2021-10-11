---
title: Axios无法在Safari浏览器及微信中打开
date: 2019-09-09 21:31:05
tags: ['axios', '小技巧']
---

## 前言

最近在项目开发中碰到这样一个问题，刚登录完用 axios 调订单列表接口时，总是提示 token 过期失效。仔细想想，刚登录的返回的 token 都没存在 window.document.cookie 中，怎么会过期呢。经过沿路抽丝剥茧，debugger，打印出来的 token 依然是最新的返回，这使得我不禁陷入了沉思 🧕🏿👳🏿‍♂️，为啥？

## 正常代码

```javascript
  // list
  getList() {
    this.$axios
      .get(url, {
        headers: {
          token: 'xxx'
        },
        withCredentials: true,
        params: {
          page: this.page,
          per_page: 6,
          others: 'xxx'
        }
      })
      .then(res => {
        console.log('res: ', res);
      })
      .catch(err => {
        console.log('err: ', err);
      });
  },
```
这代码看起来没啥问题啊，为啥在safari浏览器就不行了呢？
于是。。
## 方法

### 修改后的代码[方法一]
``` javascript
  // list
  getList() {
    this.$axios
      .get(`${url}?nocache=${new Date().getTime()}`, {
        headers: {
          token: 'xxx'
        },
        withCredentials: true,
        params: {
          page: this.page,
          per_page: 6,
          others: 'xxx'
        }
      })
      .then(res => {
        console.log('res: ', res);
      })
      .catch(err => {
        console.log('err: ', err);
      });
  },
```
### 修改后的代码[方法二]
``` javascript
  // list
  getList() {
    this.$axios
      .get(url, {
        headers: {
          token: 'xxx',
          Pragma: no-cache, // 注释1
          'Cache-control': no-cache // 注释2
        },
        withCredentials: true,
        params: {
          page: this.page,
          per_page: 6,
          others: 'xxx'
        }
      })
      .then(res => {
        console.log('res: ', res);
      })
      .catch(err => {
        console.log('err: ', err);
      });
  },
```
这样一来的话就给接口添加了一个参数，而且这参数与上一次的又不一样，所以就不会存在缓存这一说了。
## 问题【缘】由
～据说是某些浏览器请求接口的时候，参数未变，浏览器不会发起新的请求，就用旧的数据去请求，而请求头headers存放在cookie里；故而没有更新cookie里的token。
～【注释1】
Pragma 是一个在 HTTP/1.0 中规定的通用首部，这个首部的效果依赖于不同的实现，所以在“请求-响应”链中可能会有不同的效果。它用来向后兼容只支持 HTTP/1.0 协议的缓存服务器，那时候 HTTP/1.1 协议中的 Cache-Control 还没有出来。
～【注释2】
Cache-Control 通用消息头字段，被用于在http请求和响应中，通过指定指令来实现缓存机制。缓存指令是单向的，这意味着在请求中设置的指令，不一定被包含在响应中。
~总的来说是换的原因。相信还有别的方法可以解决这个问题
## 相关文档
[Headers/Pragma](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Pragma)
[Headers/Cache-Control](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Cache-Control)


