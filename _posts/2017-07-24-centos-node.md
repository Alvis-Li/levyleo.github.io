---
layout: post
title: 虚拟机中CentOS搭建外网能够访问的Node.js Server
---

1、虚拟机中安装CentOS：略

2、安装sunny-ngrok:

`curl -O http://hls.ctopus.com/sunny/linux_amd64.zip`

`unzip linux_amd64.zip`

`sudo cp ./linux_amd64/sunny /usr/local/bin/`

`export PATH=$PATH:/usr/local/bin/` 设置环境变量，便于直接使用sunny命令

3、sunny-ngrok使用方法-[Sunny-Ngrokhttp前置域名使用方法](http://www.sunnyos.com/article-show-67.html)

4、安装Node.js:

从EPEL库安装Node.js

`sudo yum install epel-release`

`sudo yum install nodejs`

5、安装ngnix:

`sudo yum -y install gcc automake autoconf libtool make`

`sudo yum install gcc gcc-c++`

`sudo yum install nginx`

6、配置防火墙：[参照](http://www.cnblogs.com/phpshen/p/5842118.html)

安装：`yum install firewalld`

开启：`systemctl start firewalld.service`

关闭：`systemctl stop firewalld.service`

开机自启动: `systemctl enable firewalld.service`

关闭开机自启动：`systemctl disable firewalld.service`

查看状态：`systemctl status firewalld`

开启某个端口: 

```
#firewall-cmd --permanent --zone=public --add-port=80/tcp  //永久
#firewall-cmd  --zone=public --add-port=80/tcp   //临时
```

重新加载：`firewall-cmd --reload`

7、配置Nginx



