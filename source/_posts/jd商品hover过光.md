---
title: jd商品hover过光
date: 2018-10-12 14:27:38
categories: jd商品hover过光效果
tags: [HTML, CSS3]
---
#### 效果
![](/images/wIQDW7wiZ8.gif)
#### 代码
``` html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <title>Document</title>
  <style>
    /*广告图片效果*/
    div {
      position:relative;
      height:450px;
      width:330px;
      left:150px;
      top:0;
      overflow:hidden;
    }
    div a {
      display:block;
    }
    a:hover::before {
      left:400px;
      transition:left 1s ease 0s;
    }
    div a::before {
      content:"";
      position:absolute;
      width:80px;
      height:450px;
      top:0;
      left:-150px;
      overflow:hidden;
      background:-moz-linear-gradient(left,rgba(255,255,255,0)0,rgba(255,255,255,.2)50%,rgba(255,255,255,0)100%);
      background:-webkit-gradient(linear,left top,righttop,color-stop(0%,rgba(255,255,255,0)),color-stop(50%,rgba(255,255,255,.2)),color-stop(100%,rgba(255,255,255,0)));
      background:-webkit-linear-gradient(left,rgba(255,255,255,0)0,rgba(255,255,255,.2)50%,rgba(255,255,255,0)100%);
      background:-o-linear-gradient(left,rgba(255,255,255,0)0,rgba(255,255,255,.2)50%,rgba(255,255,255,0)100%);
      -webkit-transform:skewX(-25deg);
      -moz-transform:skewX(-25deg);
      -o-transform:skewX(-25deg);
    }
  </style>
</head>
<body>
  <div>
    <a href="#">
      <img src="http://www.jq22.com/img/cs/500x500b.png" width="330" height="475">
    </a>
  </div>
</body>
</html>
```