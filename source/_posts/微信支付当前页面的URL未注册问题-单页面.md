---
title: '微信支付当前页面的URL未注册问题[单页面]'
date: 2019-08-13 21:22:06
tags: ['微信支付']
---
## 前言
微信支付时，当调用微信支付的时候，微信会判断当前页面和微信公众号后台设置的支付授权目录是否一致，他会把页面最后一次刷新的url作为判断依据（如果用户刷新了任何页面，这个页面就是支付页面），这个时候，单页应用的路由中‘#’后面的内容也会被传递过去，在微信的判断流程里，这个url和设置的目录是不匹配的，因为涉及到多个页面都会发起支付请求，所有设置多个带页面参数的url是不合理的，所以这里在‘#’前面添加了‘?’，让微信忽略‘?’后面的内容。
## 设置支付目录
### 支付根目录
``` javascript
http://www.paytest.com/app/
```
### 支付页面
``` javascript
http://www.paytest.com/app/#/pay1

http://www.paytest.com/app/#/pay2

http://www.paytest.com/app/#/pay3
```
## 解决方式
当我在'#'前面添加'?',这个时候微信会把'?'后面的内容当做参数而vue可以识别'?#',这样既可以避免出现出现提示当前页面url未注册的错误在视图加载后，修改url（这样不会触发页面重新加载，其他框架也可做类似处理）
## 解决代码
``` javascript
  mounted() {
    if (window.location.href.indexOf("?#") < 0) {
      window.location.href = window.location.href.replace("#", "?#");
    }
  }
```