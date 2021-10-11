---
title: git撤销已经push到远端的commit
date: 2018-10-12 15:39:15
tags: git
categories: git使用笔记
---
#### 在使用git时，不小心把多余的文件commit了，而且还push到远程了，怎么回退到提交之前的版本呢？
###### 先在本地回退到相应的版本：
###### **版本号就是你的commitID**
``` bash
git reset --hard <版本号>
// 注意使用 --hard 参数会抛弃当前工作区的修改
// 使用 --soft 参数的话会回退到之前的版本，但是保留当前工作区的修改，可以重新提交
```
###### 如果此时使用push命令,会提示本地的版本落后于远端的版本；:
``` bash
git push origin <分支名>
```
![](/images/WX20181012-155029.png)
###### 为了覆盖掉远端的版本信息，使远端的仓库也回退到相应的版本，需要加上参数--force
```
git push origin <分支名> --force
```
