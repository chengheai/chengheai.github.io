---
title: jenkins+docker+github+nginx前端实现自动化部署
date: 2020-07-06 22:08:22
tags: ['nginx','docker','jenkins']
---
## 前言
由于之前写过react+nodejs+mongodb的项目，只是在本地开发完成，想尝试正常开发部署上线流出走一波，然而在网上搜索一番，大部分都是写的基础东西，标题大内容小，有的甚至写到关键位置就没了，当然也有些是好文章，下面是我总结一些问题，踩过坑之后的流程
## 准备
* 虚拟机一台，市面上的有VirtualBox【mac】，VMware【windows】系统为linux，笔者为linux CentOS7.6
## Docker安装

``` javascript
// 看你当前的内核版本
uname -r

// yum 包更新到最新
yum update -y

// 安装 docker
yum install docker

// 启动并加入开机启动
systemctl start docker
systemctl enable docker

// 下载docker-compose
curl -L https://github.com/docker/compose/releases/download/1.23.2/docker-compose-`uname -s`-`uname -m` > /usr/local/bin/docker-compose

// 添加可执行权限(这里不懂可以看一下菜鸟教程-linux教程-文件权限)
chmod +x /usr/local/bin/docker-compose
// 查看docker-compose版本
docker-compose --version

```
## Jenkins 安装与配置
``` javascript
docker search jenkins
```

选择第二个或者第三个，第一个有坑
### 安装jenkins
``` js
docker run -d -u 0 --privileged --name jenkins -p 8888:8080 -v /root/jenkins_home:/var/jenkins_home jenkins/jenkins
```
注意⚠️：本机需要在/root下创建一个jenkins_home文件夹【没有就加，有就要加】
其中:
-d 指的是在后台运行；
-u 0 指的是传入 root 账号 ID，覆盖容器中内置的账号；
-v /root/jenkins_home:/var/jenkins_home 指的是将 docker 容器内的目录 /var/jenkins_home 映射到宿主机 /root/jenkins_home 目录上；
--name jenkins 指的是将 docker 容器内的目录 /var/jenkins_home 映射到宿主机 /root/jenkins_home 目录上；
-p 49003:8080 指的是将容器的 8080 端口映射到宿主机的 8888 端口；
--privileged 指的是赋予最高权限。

整条命令的意思就是：
运行一个镜像为 jenkins/jenkins:latest 的容器，命名为 jenkins_home，使用 root 账号覆盖容器中的账号，赋予最高权限，将容器的 /var/jenkins_home 映射到宿主机的 /root/jenkins_home 目录下，映射容器中 8080 端口到宿主机 49003 端口

执行完成后，等待几十秒，等待 Jenkins 容器启动初始化。到浏览器中输入 http://your ip:49003 查看 Jenkins 是否启动成功

看到如下界面说明启动成功：

通过如下命令获取密码，复制到上图输入框中

进入到下个页面，选择【安装推荐的插件】。

由于墙的问题，需要修改 Jenkins 的默认下载地址。可以在浏览器另起一个 tab 页面，进入 http://your ip:49003/pluginManager/advanced，修改最下面的升级站点 URL 为 http://mirror.esuni.jp/jenkins/updates/update-center.json

然后重启容器，再次进入初始化页面，通常下载速度会加快。

然后就是创建管理员账号。

进入首页后，因为自动化部署中需要通过 ssh 登陆服务器执行命令以及 node 环境，所以需要下载 Publish Over SSH 和 NodeJS 插件，可通过系统管理 -> 管理插件 -> 可选插件进入，搜索选中并直接安装。如下图所示：

需要注意的是，Publish Over SSH 需要配置相关 ssh 服务器，通过系统管理 -> 系统设置 进入并拉到最下面，输入 Name、Hostname、Username、Passphrase / Password 等参数。如下图所示：

然后点击 Test Configuration 校验能否登陆。

至此 Jenkins 已经完成了全局配置。


关联 Jenkins 和 Github
在 GitHub 创建一个项目，以本项目为例，在项目根目录下创建 nginx.conf 和 docker-compose.yml 文件


nginx.conf
``` bash
#user  nobody;
worker_processes 1;

#error_log  logs/error.log;
#error_log  logs/error.log  notice;
#error_log  logs/error.log  info;

#pid        logs/nginx.pid;


events {
  worker_connections 1024;
}


http {
  include mime.types;
  default_type application/octet-stream;

  #log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
  #                  '$status $body_bytes_sent "$http_referer" '
  #                  '"$http_user_agent" "$http_x_forwarded_for"';

  #access_log  logs/access.log  main;

  sendfile on;
  #tcp_nopush     on;

  #keepalive_timeout  0;
  keepalive_timeout 65;

  #gzip  on;

  server {
    listen 10000;
    server_name 192.168.56.101;

    #charset koi8-r;

    #access_log  logs/host.access.log  main;

    location / {
      index index.html index.htm;
      root /usr/share/nginx/html;
      # 所有静态资源均指向 /index.html
      try_files $uri $uri/ /index.html;
    }

    #error_page  404              /404.html;

    # redirect server error pages to the static page /50x.html
    #
    error_page 500 502 503 504 /50x.html;
    location = /50x.html {
      root  /usr/share/nginx/html;
    }
    # location /api {
    #   proxy_pass http://server:port;
    # }


    # proxy the PHP scripts to Apache listening on 127.0.0.1:80
    #
    #location ~ \.php$ {
    #    proxy_pass   http://127.0.0.1;
    #}

    # pass the PHP scripts to FastCGI server listening on 127.0.0.1:9000
    #
    #location ~ \.php$ {
    #    root           html;
    #    fastcgi_pass   127.0.0.1:9000;
    #    fastcgi_index  index.php;
    #    fastcgi_param  SCRIPT_FILENAME  /scripts$fastcgi_script_name;
    #    include        fastcgi_params;
    #}

    # deny access to .htaccess files, if Apache's document root
    # concurs with nginx's one
    #
    #location ~ /\.ht {
    #    deny  all;
    #}
  }
}
```

docker-compose.yml

```bash
version: '3'
services:
  ad-tpl-vue: #项目name
    container_name: 'webhtml' #容器名称
    image: nginx
    restart: always
    ports:
      - 10000:10000
    volumes:
      - ./nginx.conf:/etc/nginx/nginx.conf #挂载nginx配置
      - ./dist:/usr/share/nginx/html/ #挂载项目
    privileged: true
```


这里需要解释下 volumes 参数，在打包 Docker 镜像时，如果将 nginx.conf 和 dist 直接拷贝到镜像中，那么每次修改相关文件时，都需要重新打包新的镜像。通过 volumes 可以将宿主机的某个文件映射到容器的某个文件，那么改动相关文件，就不要重新打包镜像了，只需修改宿主机上的文件即可。

然后在 Jenkins 创建一个新的任务，选择【构建一个自由风格的软件项目】，并设置相关配置，如下图所示。

其中第三张图两部分命令含义如下：

第一部分 shell 命令是 build 前端项目，会在项目根目录下生成 dist 目录

``` bash
echo $PATH
node -v
npm -v
npm install
npm run build
```

第二部分 shell 命令就是通过 ssh 登陆服务器，通过 docker-compose 进行构建 docker 镜像并运行容器。相较于直接使用 docker ，当更新代码时无需执行停止删除容器，重新构建新的镜像等操作。
cd /root/jenkins_home/workspace/ad-tpl-vue \
&& docker-compose stop && docker-compose up -d

最后可以回到该任务页，点击【立即构建】来构建我们的项目了。
实现自动触发打包
不过仍有个问题，那就是当向 GitHub 远程仓库 push 代码时，需要能够自动触发构建，相关操作如下。
1.修改 Jenkins 安全策略

通过系统管理 -> 全局安全配置 进入，并如下图操作


2.生成 Jenkins API Token

通过用户列表 -> 点击管理员用户 -> 设置，点击添加新 token，然后复制身份验证令牌 token

3.在 Jenkins 项目对应任务的设置中配置【构建触发器】，将刚复制的 token 粘贴进去，如下图所示：

4.在 Github 相关项目中打开 Setting -> Webhooks -> Add webhooks，输入格式如下的 URL :
``` js
// 前面是 Jenkins 服务地址，ad-tpl-vue 指在 Jenkins 的任务名称，Token指上面获取的令牌
http://12x.xxx.xxx.xxx:xxxx/job/ad-tpl-vue/build?token=Token
```

这样，我们就实现了在 push 新的代码后，自动触发 Jenkins 打包项目代码，并打包 docker 镜像然后运行。