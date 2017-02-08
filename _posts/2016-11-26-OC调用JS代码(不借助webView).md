---
layout: post
title: OC调用JS代码(不借助webView)
---

OC直接调用JS代码,要想使用WebView的请忽略,网上很多资料都是借助webView调用JS.

1.

```
#import <JavaScriptCore/JavaScriptCore.h>
```

2.

```
JSContext *context = [JSContext new];[context evaluateScript:JS代码];
```

当然此处的JS代码是OC的字符串,例如:

```
NSString * JS = @"function test(e){return e };";JSContext *context = [JSContext new];[context evaluateScript:JS];
```

3.

```
JSValue *value = [context evaluateScript:[NSString stringWithFormat:@"test('%@');",e]];
```

*注意:*此处只是简单的一个JS方法,而且是同步的,可以用return 返回值,能够直接取到结果,假如JS方法是异步的就需要用到JS调用OC的方法了,

```
JSContext *context = [JSContext new]; [context evaluateScript:@"function test(e){ 耗时操作 log(e); };"];    context[@"log"] = ^(){                NSArray *argueArr = [JSContext currentArguments];        for (id object in argueArr) {            NSLog(@"object of argurArray:%@",object);        }            };[context evaluateScript:[NSString stringWithFormat:@"test('%@');",e]];
```

类似代码注入,将OC的block注入JS中, 此时evaluateScript 没有返回值只能通过log取到结果.

将JS代码转成NSString的时候,将字符串输出,再转成JS看是否和之前的JS代码一直,能否正常运行,JS代码中有'\0'需要转成'\0',OC字符串遇到'\0'会以为字符串结束,JS代码中的换行不建议去掉,在OC中可以使用'\'转义.在JS代码中最后的分号可以省略,但是我遇到的问题是必须将分号加上,不然不执行,不知何故