---
title: Linux执行history命令显示命令执行时间
date: 2019-09-17 21:42:43
tags: ['linux']
---
## 附录
在线快速生成JSON对象api
【MYJSON】(http://myjson.com/1g17dp)
## 前言
在实际项目开发中，往往碰到这样一种情况，我明明没有敲这个指令啊，怎么代码被覆盖了？怎么回事？👺
用history命令查看 ：
![](https://github.com/chengheai/review-demo-image/blob/master/WX20190917-212406@2x.png?raw=true)
这时间也没有也不知道是谁敲的名字，这可咋办？
## 效果
![](https://github.com/chengheai/review-demo-image/blob/master/WX20190917-215133@2x.png?raw=true)
## 实现过程
在你的命令行中贱【键】入
``` bash
echo 'export HISTTIMEFORMAT="%Y-%m-%d %H:%M:%S  `whoami` "'>>/etc/profile
```
然后
再输入
``` bash
source /etc/profile
```
OK，完了！！