---
title: 怎么在npm官网上传自己的包
date: 2018-10-14 23:26:35
tags: [npm, nodejs]
categories:
  - nodejs后端开发
---
**注意：因为courage package已经发布到npm了，而npm包不存在重名的，你需要改个别的名字。**
##### demo：
\[git](https://github.com/chengheai/npm-publish)
- 第一步:
在项目的根目录下创建一个名字为node_modules的目录，此目录用来放置所有的node模块

- 第二步:
在node_modules目录下名字courage目录;

- 第三步:
courage目录下新建两个文件：
- index.js
``` javascript
'use strict'

var now = new Date(),
  hour = now.getHours(),
  greeting

if (hour < 6) {greeting = '凌晨好！';}
else if (hour < 9) {greeting = '早上好！';}
else if (hour < 12) {greeting = '上午好！';}
else if (hour < 14) {greeting = '中午好！';}
else if (hour < 17) {greeting = '下午好！';}
else if (hour < 19) {greeting = '傍晚好！';}
else if (hour < 22) {greeting = '晚上好！';}else {greeting = '夜深了，早点休息！';}
function greetings () {
  console.log(greeting)
}
module.exports = greetings

```
- package.json
```
{
  "name": "courage",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "keywords": [],
  "author": "https://github.com/chengheai",
  "license": "ISC"
}

```
- 发布courage包到npm
- 首先要注册一个npm账号(如果没有的话)
- 注册完后，在命令窗口运行`npm adduser(登陆npm)`,会提示你输入用户名和密码；
- 登陆成功后，在courage目录下执行命令npm publish(发布package)
- 发布成功后就可以登陆到npm官网去看自己发布的package。

###### 使用
1.先新建一个空的目录npm-test;然后在命令行中切换到npm-test目录;
2.然后执行npm install courage命令;
3.执行成功后在npm-test目录下看到多了一个node_modules的目录，在node_modules目录中就可以看到我们刚才发布的courage package;
4.然后再npm-test目录下新建一个greeting.js来测试一下greetings()方法，greeting.js中的内容如下：
``` javascript
const greeting = require('courage');
greeting()
```
##### 如何更新courage版本发布到npm的package？
- 进入到courage目录, 对index.js做一些修改(做为要更新的内容)
- 然后再命令行执行npm version patch, 此命令会把package.json的version更新到0.02
- 然后执行npm publish就可以更新到npm了


