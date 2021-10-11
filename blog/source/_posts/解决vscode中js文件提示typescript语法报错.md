---
title: 解决vscode中js文件提示typescript语法报错
date: 2019-09-11 21:45:33
tags: ['vscode']
---
## 前言
最近在vue项目中，全局安装了一下typescript，再看看vue项目代码就有一大堆ts提示错误
然后网上一搜也没啥好答案
～例如：
❌❌解决办法：在设置里面加上 "javascript.validate.enable": false 禁用默认的 js 验证
这样禁用那我js写错了都不知道。。。
## 正解
～设置中搜索tsconfig ->Check JS Experimental Decorators 去掉勾选☑️🐳
![](https://github.com/chengheai/review-demo-image/blob/master/%E5%BE%AE%E4%BF%A1%E5%9B%BE%E7%89%87_20190911110647.png?raw=true)
