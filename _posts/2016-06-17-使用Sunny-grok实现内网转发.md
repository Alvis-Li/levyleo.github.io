---
layout: post
title: 使用Sunny-grok实现内网转发
---

Sunny-grok 申请地址：http://www.ngrok.cc

ngrok.cfg配置：

```
server_addr: "server.ngrok.cc:4443"
auth_token: "xxxxxxxxx" #授权token，在www.ngrok.cc平台注册账号获取
tunnels:
  sunny:
   subdomain: "yueshen1314" #定义服务器分配域名前缀，跟平台上的要一样
   proto:
    http: 4040 #映射端口，不加ip默认本机
```

 node 开启服务器：

```
node node.js
```

 node.js 文件

```
var restify = require('restify');
 
var server = restify.createServer();
server.use(restify.acceptParser(server.acceptable));
server.use(restify.queryParser());
server.use(restify.bodyParser());
 
function log(req, res, next) {
    console.log('hello!');
    res.send({
        's': 'hello!'
    });
}
 
server.get('/log', log);
server.listen(4040, function() {
    console.log('%s listening at %s', server.name, server.url);
});
```

 客户端命令：mac、Linux：./ngrok -config ngrok.cfg start sunny

```
ngrok                                                          
                                                                                 
Tunnel Status                 online                                           
Version                       1.7/1.7                                          
Forwarding                    http://yueshen1314.ngrok.cc -> 127.0.0.1:4040    
Web Interface                 127.0.0.1:4040                                   
# Conn                        0                                                
Avg Conn Time                 0.00ms                                           
```

开启成功！ 