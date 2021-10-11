---
title: css划圆形进度条的demo
date: 2018-10-19 11:12:06
tags: css
---
#### 效果
![](/images/zKX0PnVPvt.gif)
#### 代码
``` javascript
  <!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <style>
    .circle_process{
	position: relative;
	width: 199px;
	height : 200px;
}
.circle_process .wrapper{
	width: 100px;
	height: 200px;
	position: absolute;
	top:0;
	overflow: hidden;
}
.circle_process .right{
	right:0;
}
.circle_process .left{
	left:0;
}
.circle_process .circle{
	width: 160px;
	height: 160px;
	border:20px solid transparent;
	border-radius: 50%;
	position: absolute;
	top:0;
	transform : rotate(-135deg);
}
.circle_process .rightcircle{
	border-top:20px solid green;
	border-right:20px solid green;
	right:0;
	/* -webkit-animation: circle_right 5s linear infinite; */
}
.circle_process .leftcircle{
	border-bottom:20px solid green;
	border-left:20px solid green;
	left:0;
	/* -webkit-animation: circle_left 5s linear infinite; */
}
/* @-webkit-keyframes circle_right{
	0%{
		-webkit-transform: rotate(-135deg);
	}
	50%,100%{
		-webkit-transform: rotate(45deg);
	}
}
@-webkit-keyframes circle_left{
	0%,50%{
		-webkit-transform: rotate(-135deg);
	}
	100%{
		-webkit-transform: rotate(45deg);
	}
} */
#show{
  font-size: 60px;
  font-weight: 600;
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translateX(-50%) translateY(-50%);
}
</style>
</head>
  <body>
    <div class="circle_process">
      <div class="wrapper right">
        <div class="circle rightcircle" id="rightcircle">
        </div>
      </div>
      <div id="show"></div>
      <div class="wrapper left">
        <div class="circle leftcircle" id="leftcircle"></div>
      </div>
    </div>
    <script>
    function getTime(){
    var date = new Date();
    var second = date.getSeconds();

    var rightcircle = document.getElementById('rightcircle');
    var leftcircle = document.getElementById('leftcircle');
    var show = document.getElementById('show');

    show.innerHTML = second;

    if(second<=30){
        rightcircle.style.cssText = "transform: rotate("+ (-135+6*second) +"deg)";
        leftcircle.style.cssText = "transform: rotate(-135deg)";
    }else{
        rightcircle.style.cssText = "transform: rotate(45deg)";
        leftcircle.style.cssText = "transform: rotate("+ (-135+6*(second-30)) +"deg)";
    }
}
getTime();
setInterval(function(){
    getTime();
}, 1000)
  </script>
  </body>
</html>
```