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

开启：`sudo nginx`

6、配置防火墙：[参照](http://www.cnblogs.com/phpshen/p/5842118.html)

安装：`yum install firewalld`

开启：`systemctl start firewalld.service`

关闭：`systemctl stop firewalld.service`

开机自启动: `systemctl enable firewalld.service`

关闭开机自启动：`systemctl disable firewalld.service`

查看状态：`systemctl status firewalld`

开启某个端口: 

```
#firewall-cmd  --zone=public --add-port=80/tcp --permanent //永久
#firewall-cmd  --zone=public --add-port=80/tcp   //临时
```

删除某个端口：

```
firewall-cmd --zone=public --remove-port=8080/tcp --permanent //永久
firewall-cmd --zone=public --remove-port=8080/tcp //临时
```

重新加载：`firewall-cmd --reload`

7、配置Nginx

```
# For more information on configuration, see:
#   * Official English Documentation: http://nginx.org/en/docs/
#   * Official Russian Documentation: http://nginx.org/ru/docs/

user nginx;
worker_processes auto;
error_log /var/log/nginx/error.log;
pid /run/nginx.pid;

# Load dynamic modules. See /usr/share/nginx/README.dynamic.
include /usr/share/nginx/modules/*.conf;

events {
    worker_connections 1024;
}

http {
    log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                      '$status $body_bytes_sent "$http_referer" '
                      '"$http_user_agent" "$http_x_forwarded_for"';

    access_log  /var/log/nginx/access.log  main;

    sendfile            on;
    tcp_nopush          on;
    tcp_nodelay         on;
    keepalive_timeout   65;
    types_hash_max_size 2048;

    include             /etc/nginx/mime.types;
    default_type        application/octet-stream;

    # Load modular configuration files from the /etc/nginx/conf.d directory.
    # See http://nginx.org/en/docs/ngx_core_module.html#include
    # for more information.
    include /etc/nginx/conf.d/*.conf;

    server {
        listen       80 default_server;
        listen       [::]:80 default_server;
        server_name  _;
        root         /usr/share/nginx/html;

        # Load configuration files for the default server block.
        include /etc/nginx/default.d/*.conf;

        location / {
        }
	location ^~ /api/node {
		proxy_pass http://127.0.0.1:8080;
                proxy_redirect     off;
                proxy_set_header        Host 'sweet.ngrok.cc';
                proxy_set_header        X-Real-IP   $remote_addr;
                proxy_set_header        X-Forwarded-For $proxy_add_x_forwarded_for;
		#proxy_connect_timeout       600;
 		#proxy_send_timeout          600;
		#proxy_read_timeout          600;
		#send_timeout                600;
	}
        error_page 404 /404.html;
            location = /40x.html {
        }

        error_page 500 502 503 504 /50x.html;
            location = /50x.html {
        }
    }

# Settings for a TLS enabled server.
#
#    server {
#        listen       443 ssl http2 default_server;
#        listen       [::]:443 ssl http2 default_server;
#        server_name  _;
#        root         /usr/share/nginx/html;
#
#        ssl_certificate "/etc/pki/nginx/server.crt";
#        ssl_certificate_key "/etc/pki/nginx/private/server.key";
#        ssl_session_cache shared:SSL:1m;
#        ssl_session_timeout  10m;
#        ssl_ciphers HIGH:!aNULL:!MD5;
#        ssl_prefer_server_ciphers on;
#
#        # Load configuration files for the default server block.
#        include /etc/nginx/default.d/*.conf;
#
#        location / {
#        }
#
#        error_page 404 /404.html;
#            location = /40x.html {
#        }
#
#        error_page 500 502 503 504 /50x.html;
#            location = /50x.html {
#        }
#    }
}

```

在默认的配置中添加了如下配置，重启Nginx后 即可将请求地址中有/api/node的请求指向8080端口做处理。初步解决了跨域问题

```	
location ^~ /api/node {
		proxy_pass http://127.0.0.1:8080;
                proxy_redirect     off;
                proxy_set_header        Host 'sweet.ngrok.cc';
                proxy_set_header        X-Real-IP   $remote_addr;
                proxy_set_header        X-Forwarded-For $proxy_add_x_forwarded_for;
		#proxy_connect_timeout       600;
 		#proxy_send_timeout          600;
		#proxy_read_timeout          600;
		#send_timeout                600;
	}
	
```  
  
8、编写node server

```
var http = require('http');  
http.createServer(function (request, response) {  
    response.writeHead(200, {'Content-Type': 'text/plain'});
	console.log(request.url);
    response.end('Hello World\n');  
}).listen(8080);  
```

nginx中 ":80/api/node" 的请求转到8080端口，所以代码中listen 8080

9、安装pm2，运行server

此时直接请求IP:80/api/node作用等同于请求8080端口

在sunny-ngrok配置后台设置本地映射端口为127.0.0.1：80，对应域名为：http://sweet.ngrok.cc 此时直接访问http://sweet.ngrok.cc/api/node即可。
