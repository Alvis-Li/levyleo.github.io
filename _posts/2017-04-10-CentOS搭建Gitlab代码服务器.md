---
layout: post
title: CentOS7搭建Gitlab代码服务器
---

```
curl -sS https://packages.gitlab.com/install/repositories/gitlab/gitlab-ce/script.rpm.sh | sudo bash

sudo yum install gitlab-ce
```

```
启动

sudo gitlab-ctl reconfigure

```

```
查看端口号

netstat -anpt | grep 8888

```

修改端口号

1.

```
/etc/gitlab/gitlab.rb

删除# unicorn['port'] = 8080的注释，将8080修改为9091 (即#去掉,将8080改为9091)

配置重启 sudo gitlab-ctl reconfigure

```

2.

```
修改nginx配置文件：/var/opt/gitlab/nginx/conf/gitlab-http.conf，将端口改为9090

修改访问地址：

打开/etc/gitlab/gitlab.rb，将gitlab.example.com替换成192.168.10.114:9090

```

即改为:

```
external_url 'http://192.168.10.114:9090'
```



可能会遇到在其他电脑上无法访问的情况

```
 sudo /sbin/iptables -I INPUT -p tcp --dport 9090 -j ACCEPT
```



附加:

```
systemctl stop firewalld.service#停止firewall

systemctl disable firewalld.service#禁止firewall开机启动

停用firewall：

systemctl stop firewalld

systemctl mask firewalld

```

