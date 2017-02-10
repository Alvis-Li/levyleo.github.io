---
layout: post
title: EventSource (node.js 与 OC)
---

node.js服务器代码:

```javascript
var http = require('http');
 
http.createServer(function (req, res) {
    res.writeHead(200, { 'Transfer-Encoding': 'chunked', 'Content-Type': 'text/event-stream' });
  
    setInterval(function() {
        var packet = 'event: hello_event\ndata: {"message":"' + new Date().getTime() + '"}\n\n';
        res.write(packet);
    }, 1000);
}).listen(9009);
```



 OC代码(需要借助封装的类:[EventSource](https://github.com/levyleo/EventSource)) 

```objective-c
EventSource *source = [EventSource eventSourceWithURL:[NSURL URLWithString:@"http://127.0.0.1:9009/"]];
[source onReadyStateChanged:^(Event *event) {
    NSLog(@"READYSTATE: %@", event);
}];
 
[source addEventListener:@"hello_event" handler:^(Event *e) {
    NSLog(@"%@: %@", e.event, e.data);
}];
```

