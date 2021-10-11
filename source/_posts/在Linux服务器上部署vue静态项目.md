---
title: 在Linux服务器上Nginx部署vue静态项目
date: 2019-09-11 21:56:37
tags: ['linux','部署', 'vue']
---
## 前言
最近看了看nginx，觉得挺好玩，刚好自己有一个VPN测试服务器，就自己敲了一遍，部署一个vue-cli初始化项目
～另外给大家推荐一个免费试玩的服务器[腾讯云☁️实验室](https://cloud.tencent.com/developer/labs/lab/10078),每天有大约6个小时的时间可以使用，两个ubuntu，一个ubuntu是3小时
3 + 3 = 6
## 安装
### nginx安装
～命令行输入
``` s
wget http://nginx.org/download/nginx-1.12.2.tar.gz

```
``` s
# 安装依赖
yum -y install gcc zlib zlib-devel pcre-devel openssl openssl-devel
# 解压缩
tar -zxvf linux-nginx-1.12.2.tar.gz
cd nginx-1.12.2/
# 执行配置
./configure
# 编译安装(默认安装在/usr/local/nginx)
make
make install
```
一顿闪烁之后你将发现你的/usr/local下多了个nginx文件夹
验证信息：
nginx主配置文件：/usr/local/nginx/conf/nginx.conf
nginx日志文件：/usr/local/nginx/logs/access.log
启动Nginx：/usr/local/nginx/sbin/nginx

然后直接访问ip地址，比如\：http://192.168.1.1/\，如果能看到如下Nginx主页说明安装成功了

![](https://raw.githubusercontent.com/chengheai/review-demo-image/master/14795543-4ea3783126a0d378.webp)

## 上传部署文件
window机器可以使用winscp上传
mac的话可以使用lrzsz
这个使用mac
执行命令

``` shell
# 打包
tar zcvf dist.tar.gz dist/
# 解压
tar zxvf dist.tar.gz
```
在根目录下新建nginx文件夹
``` shell
mkdir ~/nginx
cp -rf dist ./../vue
```
运行下面这段话可以nginx的配置文件
``` shell
$ sudo /usr/local/nginx/sbin/nginx -t
# 下面这行代码说明 nginx 的配置文件是 /usr/local/nginx/conf/nginx.conf
nginx: the configuration file /usr/local/nginx/conf/nginx.conf syntax is ok
nginx: configuration file /usr/local/nginx/conf/nginx.conf test is successful
```
## 配置nginx
![](https://github.com/chengheai/review-demo-image/blob/master/WechatIMG211.png?raw=true)
修改完毕，保存退出。运行以下命令来重启 Nginx，让配置生效：
sudo /usr/local/nginx/sbin/nginx -s reload
刚开始因为没有配置权限，访问会出现403 Forbidden错误
``` shell
 sudo chmod 755 ~
 sudo chmod 755 ~/nginx
 sudo chmod 755 ~/nginx/vue
```
最后，你能看到下面的页面
![](https://github.com/chengheai/review-demo-image/blob/master/WX20190911-224210@2x.png?raw=true)