---
title: Mac下使用rz、sz远程上传、下载
date: 2019-05-13 21:33:29
tags: ['linux']
---
## 写在前面
### 什么是rz/sz (lsz/lrz)

简单说就是，可以很方便地用这两个sz/rz工具，实现Linux下和Windows之间的文件传输(发送和接收)，速度大概为10KB/s，适合中小文件。rz/sz 通过Zmodem协议传输数据。
### 为什么要用rz/sz
普通Linux和Windows之间的文件共享方法，主要有建立nfs实现文件共享，和tftp之类的方法，但是都很麻烦，而如果只是小文件（几十K，几百K），那么直接用rz/sz，就显得极其地方便了。大文件的话，还是要考虑上面说得，其他的共享方法了，毕竟，rz/sz速度只有10K左右，传大文件会累死人的。
## Mac上如何使用
### 步骤
1.下载并安装iTerm2 (一般来说都安装过了)
http://www.iterm2.com/#/section/downloads
2.下载安装lrzsz
sudo brew install lrzsz(或者 brew install lrzsz)
3.下载并安装automatic zmoderm for iTerm2
-1 usr/local/bin
-2 sudo wget https://raw.github.com/mmastrac/iterm2-zmodem/master/iterm2-send-zmodem.sh
sudo wget https://raw.github.com/mmastrac/iterm2-zmodem/master/iterm2-recv-zmodem.sh
-3 sudo chmod 777 /usr/local/bin/iterm2-*
4.添加iTerm2 trigger
Term2 --> Profiles --> Advanced --> Edit Trigger

``` shell
  Regular expression      　　Action      　　　　　　　Parameters

  \*\*B0100　　　　　　　　Run Silent Coprocess　　/usr/local/bin/iterm2-send-zmodem.sh

  \*\*B00000000000000　  Run Silent Coprocess　　/usr/local/bin/iterm2-recv-zmodem.sh
```
![](https://github.com/chengheai/review-demo-image/blob/master/WX20190513-215337@2x.png?raw=true)
![](https://github.com/chengheai/review-demo-image/blob/master/WX20190513-215356@2x.png?raw=true)


