---
layout: 在docker
title: 在docker下删除一个或多个image及container
date: 2018-11-15 14:13:52
tags: docker
---
<table><tr><td bgcolor=orange>停止所有的container，这样才能够删除其中的images</td></tr></table>
### 删除一个container
#### 公式
```
docker rm container_name/ID
```
``` bash
➜  docker git:(master) ✗ docker ps -a
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS                           PORTS               NAMES
41c2ed15b32a        koa-demo            "/bin/bash"         About an hour ago   Exited (127) About an hour ago                       awesome_noyce
caec5440819a        hello-world         "/hello"            2 hours ago         Exited (0) 2 hours ago                               festive_austin
```
```
➜  docker git:(master) ✗ docker rm caec5440819a
caec5440819a
```
```
➜  docker git:(master) ✗ docker ps -a
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS                           PORTS               NAMES
41c2ed15b32a        koa-demo            "/bin/bash"         About an hour ago   Exited (127) About an hour ago                       awesome_noyce
```
### 删除多个container
```
docker rm $(docker ps -a -q)
```
### 删除一个image
#### 停止
```
➜  docker git:(master) ✗  docker stop $(docker  ps  -a -q)
41c2ed15b32a
```
#### 查看所有
```
➜  docker git:(master) ✗ docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
koa-demo            latest              7d24f0109057        About an hour ago   676MB
hello-world         latest              4ab4c602aa5e        2 months ago        1.84kB
node                8.4                 386940f92d24        14 months ago       673MB
```
#### 删除
```
➜  docker git:(master) ✗ docker rmi 4ab4c602aa5e
Untagged: hello-world:latest
Untagged: hello-world@sha256:0add3ace90ecb4adbf7777e9aacf18357296e799f81cabc9fde470971e499788
Deleted: sha256:4ab4c602aa5eed5528a6620ff18a1dc4faef0e1ab3a5eddeddb410714478c67f
Deleted: sha256:428c97da766c4c13b19088a471de6b622b038f3ae8efa10ec5a37d6d31a2df0b
```
#### 检查是否删除
```
➜  docker git:(master) ✗ docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
koa-demo            latest              7d24f0109057        About an hour ago   676MB
node                8.4                 386940f92d24        14 months ago       673MB
```
### 删除多个image
```
docker rmi  $( docker  images -q )
```
#### 强制删除
```
docker rmi  imagename 或者docker  rmi  -f   imagename
```

