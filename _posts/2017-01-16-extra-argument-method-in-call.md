---
layout: post
title: alamofire extra argument method in call
---

使用Alamofire时代码如下

```
Alamofire.request(urlString, method: method, parameters: parameters, encoding: Null, headers: headers)
```

总是提示

```
extra argument method in call
```

在stackoverflow找到相关解决方法

```
Alamofire.request(urlString, method: method, parameters: parameters, encoding: JSONEncoding.default, headers: headers)
```

**Fixed it!**