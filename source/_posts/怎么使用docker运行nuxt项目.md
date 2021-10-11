---
title: 怎么使用docker运行nuxt项目
date: 2020-01-07 21:18:36
tags: ['docker']
---

## 个人日常 DEMO

```bash
git clone git@github.com:chengheai/ssr-project.git
```

## 添加 Dockerfile

```javascript
touch Dockerfile
vi Dockerfile
FROM node:8.11.0

WORKDIR /opt
ADD node_modules /opt/node_modules
ADD server /opt/server
ADD .nuxt /opt/.nuxt
ADD static /opt/static
ADD nuxt.config.js /opt
ADD package.json /opt
EXPOSE 3000
CMD ["npm", "run", "start"]

```

~FROM node:8.11.0：该 image 文件继承官方的 node image，冒号表示标签，这里标签是 8.11.0，即 8.11.0 版本的 node。
~ADD . /opt：将当前目录文件下的所有文件（除了.dockerignore 排除的路径），都拷贝进入 image 文件的/opt 目录。
~WORKDIR /opt：指定接下来的工作路径为/opt。
~RUN npm install 或者 ADD node_modules：在/opt 目录下，运行 npm install 命令安装依赖。注意，安装后所有的依赖，都将打包进入 image 文件。
~EXPOSE 3000: 3000 端口暴露出来， 允许外部连接这个端口。

## 添加.dockerignore

```bash
touch .dockerignore
vi .dockerignore
  .git
  # node_modules
  npm-debug.log

```

## build

```bash
docker image build -t ssr-project.
# 或者
docker image build -t ssr-project:0.0.1 .
# 最后面的空格和点不要漏
```

![](https://github.com/chengheai/review-demo-image/blob/master/WechatIMG370.png?raw=true)

## start

```bash
$ docker run -d --name ssr-server -p 3000:3000 test-ssr
ssr-server: 容器名称
test-ssr: 镜像
关闭
$ docker stop ssr-server
```

```bash
okcer image ls // 查看 image
docker image build -t ssr-project.  // 常见image
docker rmi [containerID] // 删除指定镜像
docker run -it --rm -p 3000:3000 ssr-project // docker container run命令会从 image 文件生成容器。--rm参数，在容器终止运行后自动删除容器文件
docker ps // 列出本机正在运行的容器
docker ps -a  // 列出本机所有容器，包括终止运行的容器
docker container ls // 列出本机正在运行的容器
docker container ls --all // 列出本机所有容器，包括终止运行的容器
docker container rm [containerID] // 删除指定容器
docker container kill [containerID] // 终止容器运行，相当于向容器里面的主进程发出 SIGKILL 信号
docker container stop [containerID] // 终止容器运行，相当于向容器里面的主进程发出 SIGTERM 信号，然后过一段时间再发出 SIGKILL 信号
docker container start [containerID] // 启动已经生成、已经停止运行的容器
docker container logs [containerID] // 用来查看 docker 容器的输出，即容器里面 Shell 的标准输出
docker container exec -it [containerID] /bin/bash // 用于进入一个正在运行的 docker 容器 进入容器就可以在容器的 Shell 执行命令了
docker container cp [containID]:[/path/to/file] . // 用于从正在运行的 Docker 容器里面，将文件拷贝到本机
```

## 附录

[docker 常用指令说明](https://www.cnblogs.com/DeepInThought/p/10896790.html)
