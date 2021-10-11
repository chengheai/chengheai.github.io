---
title: git中配置别名git config --global alias.xxx
date: 2019-07-08 20:40:19
tags: ['git']
---
# 前言
有没有经常敲错命令？
比如git status 打成 git stauts? stats...
你有没有见过别人敲指令的时候是 git st?
废话不多说，先呈上些实用的例子吧
# alias
``` shell
$ git status
||
$ git config --global alias.st status
******* br *******
$ git checkout
||
$ git config --global alias.co checkout
******* br *******
git commit
||
$ git config --global alias.ci commit
******* br *******
$ git branch
||
$ git config --global alias.br branch
```
以后你提交代码就可以这样输入
```
git ci -m "biling,biling.."
```
```

$ git reset HEAD
||
$ git config --global alias.unstage 'reset HEAD'
******* br *******
$ git last
||
$ git config --global alias.last 'log -1'

```
## gitk .
``` bash
git config --global alias.lg "log --color --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit"
```
### 美图献上

![]('https://github.com/chengheai/review-demo-image/blob/master/WX20190708-205604@2x.png?raw=true')

## 撤销与查看
```
$ vi .gitconfig
[alias]
    co = checkout
    ci = commit
    br = branch
    st = status
[user]
    name = Your Name
    email = your@email.com
```
别名就在[alias]后面，要删除别名，直接把对应的行删掉即可。
而当前用户的Git配置文件放在用户主目录下的一个隐藏文件.gitconfig中


