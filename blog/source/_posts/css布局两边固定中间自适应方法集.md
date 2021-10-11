---
title: css布局两边固定中间自适应方法集
date: 2019-06-21 21:56:26
tags: ['css']
---
## 1、90后万能flex方法

``` html
<!DOCTYPE html>
  <html lang="en">
  <head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <style>
      .left{
        width: 100px;
        background: red;
      }
      .right{
        width: 100px;
        background: blue;
      }
      .wrap{
        display: flex;
      }
      .middle{
        flex: 1;
        background: rosybrown;
      }
    </style>
  </head>
  <!-- 中间响应式，两边固定 -->
  <body>
    <div class="wrap">
      <div class="left">left</div>
      <div class="middle"></div>
      <div class="right">right</div>
    </div>
  </body>
</html>
```
## 2、80后大佬们的table方法
``` html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <title>Document</title>
  <style>
    .left{
      width: 100px;
      background: red;
    }
    .right{
      width: 100px;
      background: blue;
    }
    .wrap{
      display: table;
    }
    .wrap > div{
      display: table-cell;
    }
    .middle{
      background: rosybrown;
    }
  </style>
</head>
<!-- 中间响应式，两边固定 -->
<body>
  <div class="wrap">
    <div class="left">left</div>
    <div class="middle">我是中间的</div>
    <div class="right">right</div>
  </div>
</body>
</html>
```
## 3、00后牛叉gird方法
``` html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <title>Document</title>
  <style>
    .left{
      background: red;
    }
    .right{
      background: blue;
    }
    .wrap{
      display: grid;
      width: 100%;
      grid-template-rows: auto;
      grid-template-columns: 100px auto 100px;
    }
    .middle{
      background: rosybrown;
    }
  </style>
</head>
<!-- 中间响应式，两边固定 -->
<body>
  <div class="wrap">
    <div class="left">left</div>
    <div class="middle">我是中间的</div>
    <div class="right">right</div>
  </div>
</body>
</html>
```
## 4、8090老套路position方法
``` html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <title>Document</title>
  <style>
    .left{
      left: 0;
      width: 100px;
      background: red;
    }
    .right{
      right: 0;
      width: 100px;
      background: blue;
    }
    .wrap{
      position: relative;
    }
    .wrap> div{
      position: absolute;
    }
    .middle{
      left: 100px;
      right: 100px;
      background: rosybrown;
    }
  </style>
</head>
<!-- 中间响应式，两边固定 -->
<body>
  <div class="wrap">
    <div class="left">left</div>
    <div class="middle">我是中间的</div>
    <div class="right">right</div>
  </div>
</body>
</html>
```