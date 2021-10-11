---
title: js事件委托
date: 2018-10-12 14:56:58
tags: [JavaScript,HTML,事件]
categories: 原生js事件委托
---
####  事件委托
#####  利用冒泡的原理，把事件加到父级上，触发执行效果
####  好处
##### 提高性能  新添加的元素，還會有之前的事件

#### 代码
``` html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <title>Document</title>
  <script>
    ///提高性能  新添加的元素，還會有之前的事件
    // 事件委托：利用冒泡的原理，把事件加到父级上，触发执行效果
    window.onload = function(){
      var oUl = document.querySelector('ul');
      var oInput = document.querySelector('input');
      var aLi = oUl.querySelectorAll('li');
      var now = 4;
      console.log(aLi)
      // for(var i=0;i<aLi.length;i++){
      //   aLi[i].onmouseover = function(){
      //     this.style.background = 'red'
      //   }
      //   aLi[i].onmouseout = function(){
      //     this.style.background = ''
      //   }
      // }
      oInput.onclick = function(){
        now++;
        var oli = document.createElement('li');
        oli.innerHTML = now*111
        oUl.appendChild(oli)
      }
      oUl.onmouseover = function(ev){
       var ev = ev || window.ev;
       var target = ev.target || ev.srcElement;
      //  alert(target.innerHTML)
      if(target.nodeName.toLowerCase() == 'li'){
        target.style.background = 'red'; 
      }
      
      }
      oUl.onmouseout = function(ev){
       var ev = ev || window.ev;
       var target = ev.target || ev.srcElement;
      //  alert(target.innerHTML)
      target.style.background = ''; 
      }
    }
  </script>
</head>
<body>
<input type="button" value="添加">
  <ul>
    <li>111</li>
    <li>222</li>
    <li>333</li>
    <li>444</li>
  </ul>
</body>
</html>
```

