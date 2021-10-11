---
title: git分支上删除某次提交与获取其他分支上某个提交
date: 2019-10-15 22:10:02
tags: ['git']
---
## 前言
～在日常开发中，常常会碰到这样的问题，我们在dev分支上写好的代码，经过严格的测试确认无误没有问题，打包发不到正式环境上去了，后来产品经理说这个功能不是本期上的，但是正式服已经有了，那怎么弄呢？
## 例子
例如，现在当前分支上代码有三次提交，我想把其中间一次提交去掉，怎么做？
步骤：
 ```javascript
git log // 查看log信息记下commitId
```
![](https://github.com/chengheai/review-demo-image/blob/master/WX20191015-221740@2x.png?raw=true)
 ```javascript
git rebase -i commitId // [上一次提交的commitId]
```
![](https://github.com/chengheai/review-demo-image/blob/master/WX20191015-222936@2x.png?raw=true)
 ```javascript
vim  i[插入] // 将需要修改的commit的 pick改为drop
```

 ```javascript
esc wq // 退出插入模式，保存退出
```
 ```javascript
git status // 状态查看
```
![](https://img-blog.csdnimg.cn/20191015225855423.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2NoaTExMzA=,size_16,color_FFFFFF,t_70)
 ```javascript
git push origin branch -f // 提交并覆盖远程
 ```
![](https://github.com/chengheai/review-demo-image/blob/master/WX20191015-223346@2x.png?raw=true)
## 附加📎
当前分支拉取其他分支某次提交代码 [git cherry-pick commitID]

``` javascript
git cherry-pick commitID
```
属于复制的操作，对源分支不影响～