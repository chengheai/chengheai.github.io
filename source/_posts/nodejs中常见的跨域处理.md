---
title: nodejs中常见的跨域处理
date: 2020-02-23 21:29:53
tags: ['nodejs', '跨域']
---

## 跨域问题

ajax 同源策略 协议 主机(ip,域名) 端口号

1. 协议，端口
2. cors
3. header 头文件信息
4. jsonp
5. 服务器

## 解决 🧍‍♀️

> 协议，端口

将跨域的文件放入与服务器一样的文件下

> cors

以 express 为例

```js
const express = require('express');
const cors = require('cors');
...
your codes...
...
app.use(cors());
```

> header 头文件信息

以 express 为例

```js
const express = require('express');
...
your codes...
...
app.all('*', function(req, res, next) {
    res.header("Access-Control-Allow-Origin", "*");
    res.header("Access-Control-Allow-Headers", "X-Requested-With,Content-Type");
    res.header("Access-Control-Allow-Methods","PUT,POST,GET,DELETE,OPTIONS");
    next();
});
```

> jsonp

```js
JSONP.getJSON('http://api.com', data, function(data) {
  console.log(data);
});
```

> 服务器

```js
const express = require('express');
const request = require('request');
...
your codes...
...
// 本地api接口
app.get('/cors', (req, res) => {
  // 发送一个服务器请求
  console.log('cors.html的ajax');
  // 目标服务器，你想请求的服务器api
  request('http://www.google.com/xx/api/v1', (err, response, body) => {
    console.log(body);
    if (!err) {
      res.send(body);
    } else {
      console.log(err);
    }
  });
});
```
