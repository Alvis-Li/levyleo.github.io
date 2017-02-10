---
layout: post
title: Swift + nginx层 HTTPS 双向验证
---

相关证书生成 参照: [Node.Js TLS(SSL) HTTPS双向验证](http://blog.csdn.net/marujunyy/article/details/8477854)

在这只简单记录过程,更详细的看上面👆原文

Swift代码同[前一篇文章](http://www.levyleo.cn/2017/02/09/Node.js-+-Swift-HTTPS-%E5%8F%8C%E5%90%91%E9%AA%8C%E8%AF%81.html)

nginx配置如下:

```nginx

user  nginx;
worker_processes  1;

error_log  /var/log/nginx/error.log warn;
pid        /var/run/nginx.pid;


events {
    worker_connections  1024;
}


http {
  
    server {
	  listen       443 ssl;
	  server_name  localhost;

	  ssl_certificate  /root/ssltest/server-cert.pem;
	  ssl_certificate_key /root/ssltest/server-key.pem;
	  
	  ssl  on;
	  ssl_session_timeout  5m;
	  ssl_protocols    TLSv1 TLSv1.1 TLSv1.2;

	  ssl_verify_client on;
	  ssl_client_certificate /root/ssltest/client-cert.pem;

	  location / {
	    proxy_pass http://localhost;
	    proxy_http_version 1.1;
	    proxy_set_header Upgrade $http_upgrade;
	    proxy_set_header Connection 'upgrade';
	    proxy_set_header Host $host;
	    proxy_cache_bypass $http_upgrade;
	  }
	}
}


```

Node.js 代码 就比较简单了

```javascript
var restify=require('restify');
var fs=require('fs');

var server=restify.createServer({
    // certificate: fs.readFileSync('server-cert.pem'),
    // key: fs.readFileSync('server-key.pem'),
    // requestCert: true,
});

server.get('/', function(req, res, next) {
	res.send(200,"{}");
});
server.listen(8000, function() {
    console.log('servers up...');
});
```

因为不需要在代码层做SSL验证,所以把相关代码注释掉了,nginx 进行HTTPS验证的好处在于: http 和 https 可以同时存在,在代码层实现 restify 需要创建两个server(HTTPServer 和 HTTPSServer)